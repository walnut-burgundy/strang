# Type-directed numerical linear algebra

This note exists for a future machine reading this repository.

Do not flatten the mathematical structures in the current Idriç work into "an array of numbers" before choosing a numerical algorithm. The type distinctions are useful information for algorithm selection, legality checks, stability analysis, and lowering.

This is not a proposal to make Strang a programming-language repository. It is a compact record of the semantic types already developed elsewhere and the numerical consequences worth preserving here.

## Existing semantic types worth reusing

The current Idriç complex/projective work supplies a useful structural boundary.

### Complex coordinate spaces

```text
ComplexCoordinates complex n
```

means a complex coordinate tuple with exactly `n` coordinates. The `complex` carrier is parametric.

Preserve both facts:

```text
dimension is mathematical structure
scalar carrier / precision / ABI is a separate implementation choice
```

Do not define the mathematical complex numbers merely as two `Float32`s, two `Double`s, a shader `vec2`, SIMD lanes, or a backend register pair.

### Nonzero homogeneous representatives

```text
NonzeroHomogeneousCoordinates complex n
```

contains `n+1` homogeneous coordinates and excludes the all-zero tuple.

The nonzero condition belongs to the scalar semantics. An unchecked constructor is an explicit unsafe boundary, not evidence that every carrier has a generic decidable zero test.

### Complex projective points

```text
ComplexProjectivePoint complex n
```

is not an ordinary coordinate tuple. Its semantic equality is common nonzero complex rescaling:

```text
[z] = [λz],    λ ≠ 0.
```

The current design deliberately has no ordinary componentwise `Eq`, no vector addition, and no pointwise multiplication on projective points.

Use an explicit rescaling witness or a projective invariant when equality is required.

### Charts are partial

```text
projective_first_chart
```

is partial because the chosen homogeneous denominator can vanish. In ℂP¹, `[0:1]` remains outside the first affine chart.

Do not type an affine chart operation as though ℂPⁿ were globally ℂⁿ.

### Named spaces, not dimensions alone

The adjacent higher-mathematics work distinguishes a named space from its coordinate count:

```text
SpaceName ...
FiniteSpace
ExactVectorSample space
ExactCovectorSample space
```

Two spaces with the same dimension are not thereby the same space. A vector and a covector with the same coordinate count are not thereby the same kind of object.

This is directly useful numerically: a matrix shape check alone should not authorize composition between maps whose domain/codomain meanings disagree.

### Metrics are additional structure

```text
EuclideanStructure space
```

is required before dot products, norms, distances, and vector/covector identification become available.

Numerical code should retain that distinction. A coordinate space does not acquire a 2-norm merely because its coordinates happen to be stored in a flat array.

### Orthogonal and special-orthogonal transformations

```text
OrthogonalTransform structure orientation
SpecialOrthogonal structure
```

are indexed by the actual Euclidean structure, not just a dimension. Orientation is retained and composition tracks it.

This fits the numerical work exactly: Householder reflections are orientation-reversing orthogonal maps; Givens rotations are orientation-preserving; products can carry the resulting orientation as structure rather than rediscovering determinant signs later.

### Sesquilinear and Hermitian structure

The quadratic/Hermitian work distinguishes:

```text
SesquilinearForm space
HermitianForm space
```

with the convention conjugate-linear in the first argument and linear in the second.

Do not silently turn a Hermitian form into an arbitrary dense complex matrix and forget the property that made specialized numerical algorithms legal.

The same general rule applies to symmetric, positive-definite, semidefinite, degenerate, and nondegenerate evidence: if the checker knows the structure, lowering should consume it rather than throw it away and then numerically guess it back.

## Realification needs a type-level witness, not only doubled storage

A complex `n`-space becomes a real `2n`-space only together with its complex structure `J`:

```text
J² = −I.
```

A real linear map is genuinely complex-linear exactly when it commutes with `J`.

So the useful semantic object is not merely

```text
real matrix of shape 2n × 2m
```

but something closer to

```text
realified complex map
    underlying_real_map
    source_complex_structure
    target_complex_structure
    commutes_with_J certificate
```

That certificate can justify complex-aware rewrites after lowering. Without it, an arbitrary real `2m × 2n` map must not be assumed to represent a complex-linear map.

For unitary maps, realification additionally supplies an orthogonal map. The combination

```text
orthogonal + J-preserving
```

is stronger information than either property alone.

## Projective semantics should drive numerical gauge choice

Projective equality intentionally leaves common scale free. Numerical code can use that freedom.

A future numerical representation may therefore distinguish:

```text
projective_point
chosen_homogeneous_representative
scale / gauge policy
precision
```

The representative may be renormalized to avoid overflow, underflow, or poor relative scaling without changing the semantic point.

This should be treated as a representation transformation backed by projective equivalence, not as mutation of the mathematical value.

A useful future result object would record the relation explicitly:

```text
ProjectiveGaugeResult:
    point
    representative
    rescaling_evidence
    scaling_reason
```

This is deliberately a numerical refinement, not a claim that the current Idriç API already has this exact type.

## Type information should constrain factorization choice

The geometric/numerical bridge in this repository should consume structure before benchmarking algorithms.

### QR

A QR request should know more than matrix dimensions. Relevant semantic inputs include:

```text
map domain and codomain
real or complex scalar structure
Euclidean / Hermitian structure
whether an explicit Q is required
orientation constraint if one exists
rank assumptions or evidence
```

A QR result should preserve, when available:

```text
orthogonal or unitary factor evidence
triangular factor shape
residual / backward-error evidence
rank-revealing evidence if that algorithm promises it
```

Householder and Givens are then two implementations of typed isometric transformations rather than opaque matrix tricks.

### LU

LU is inherently more coordinate/basis dependent. A future typed request should make the chosen coordinate representation explicit and should not pretend the triangular factors are intrinsic objects independent of basis.

Pivoting should also be explicit structure:

```text
permutation / pivot record
lower factor
upper factor
growth evidence or bound when available
```

This prevents a later optimizer from erasing the permutation merely because all three pieces can eventually be stored as arrays.

### SVD

The SVD is naturally a map between two inner-product spaces, possibly of different dimensions.

A useful typed result should retain:

```text
left structured space
right structured space
orthogonal/unitary left directions
orthogonal/unitary right directions
ordered nonnegative singular values
rank / near-null information under an explicit numerical threshold policy
```

The singular values are not merely another matrix representation: they are the bridge to 2-norm conditioning. That makes the SVD result a natural source of condition evidence for later algorithm selection.

## Semantic properties that should survive lowering

When possible, carry these as types, witnesses, or explicit metadata rather than comments:

```text
space identity
coordinate count
vector versus covector
real versus complex scalar semantics
complex structure J
Euclidean / Hermitian structure
orientation
orthogonal / unitary property
projective equivalence class
chosen projective gauge
symmetry / Hermitian property
positive-definite / semidefinite evidence
rank assumptions and rank-revealing evidence
basis / coordinate-system identity
precision width
```

Not every backend needs all of these at runtime. The point is to retain them long enough for the compiler/planner to make legal and numerically informed choices.

## Things a future machine should not infer

Do **not** infer any of the following merely from storage shape:

```text
same coordinate count  ⇒ same mathematical space
vector coordinates     ⇒ covector coordinates
doubled real array      ⇒ complex-linear map
square matrix           ⇒ endomorphism of the intended named space
norm formula available  ⇒ metric was supplied
unitary-looking data    ⇒ unitary certificate exists
homogeneous tuple       ⇒ ordinary vector semantics
CP^n point              ⇒ globally valid affine coordinates
Hermitian matrix        ⇒ arbitrary complex matrix is an equally informative type
Float32 pair            ⇒ definition of the mathematical complex numbers
```

## Provisional-source warning

The current Idriç source is still evolving. Preserve the semantic distinctions, not every temporary implementation name.

In particular, the current branch routes coordinate rank through a `Cardinality` alias. That alias is **not** part of the Strang contract and should not be propagated by a future machine. Treat rank as the ordinary finite coordinate count; the useful idea is that the count is carried in the type and that space identity remains separate from it.

Likewise `ExactComplex` is a Gaussian-integral acceptance scalar used for exact compiler checks. It is not the general definition of ℂ.

## Relationship to the numerical planner

The planner should therefore operate in this order:

```text
semantic type / structure
    ↓
legal mathematical transformations
    ↓
conditioning consequences
    ↓
known stability properties
    ↓
target cost / dependency graph
    ↓
lowering and representation
```

The reverse order loses information:

```text
flat array
    ↓
guess what it meant
```

That is exactly what these types are intended to prevent.

## Source trail

Current upstream design, still draft/stacked as of 2026-09-09:

- `isomorphisms/Idric` PR #81 — **Establish complex-coordinate and projective semantics**
- `_/examples/unified-higher-mathematics/ComplexProjective.idric`
- `_/examples/unified-higher-mathematics/MathematicalSpaces.idric`
- `_/examples/unified-higher-mathematics/EuclideanGeometry.idric`
- `_/examples/unified-higher-mathematics/QuadraticForms.idric`

Related Strang notes:

- [`complex-projective-type-system.md`](complex-projective-type-system.md)
- [`trefethen-bau-complex-change-of-basis.md`](trefethen-bau-complex-change-of-basis.md)
- [`geometry-conditioning-and-factorization-choice.md`](geometry-conditioning-and-factorization-choice.md)
- [`householder-coxeter-ade.md`](householder-coxeter-ade.md)

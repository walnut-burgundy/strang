# ℂPⁿ type system and its linear-algebra interpretation

This note cross-posts the current complex/projective type boundary being developed in Idriç and records how it fits the realification and change-of-basis picture used in numerical linear algebra.

The current source is Idriç PR #81, **Establish complex-coordinate and projective semantics**, branch `math/complex-projective-semantics`. It is still a draft/stacked PR as of 2026-09-09, so this note records that design rather than pretending it is already the final canonical Idriç interface.

## The distinctions the checker is supposed to retain

The central rule is that the following are not interchangeable merely because they can all eventually be stored as arrays of numbers:

- a coordinate tuple in ℂⁿ;
- a nonzero homogeneous representative in ℂⁿ⁺¹;
- a point of ℂPⁿ, which is an equivalence class of those representatives;
- a coordinate tuple in ℝ²ⁿ obtained by realifying ℂⁿ;
- a named real vector space carrying a chosen Euclidean structure;
- an orthogonal or special-orthogonal transformation of that particular structured space.

The types make these distinctions explicit instead of expecting comments or runtime conventions to remember them.

## Complex coordinate spaces

The current structural type is

```text
ComplexCoordinates complex n
```

It means: exactly `n` coordinates, each drawn from some concrete complex scalar carrier `complex`.

Dimension is part of the type. A value in ℂ² cannot silently be consumed where ℂ³ is required.

The scalar carrier is deliberately parametric. The structural layer does **not** declare that a complex number is two `Double`s, two `Float32`s, a shader `vec2`, SIMD lanes, or any particular ABI representation. Those are implementation choices underneath the mathematical type.

That separation matters for the numerical work: scalar precision and machine layout should not decide what mathematical space a value inhabits.

## Nonzero homogeneous coordinates

A point of ℂPⁿ is represented by `n+1` complex homogeneous coordinates, but the all-zero tuple is forbidden.

The current type is

```text
NonzeroHomogeneousCoordinates complex n
```

whose stored coordinates have structural type

```text
ComplexCoordinates complex (n+1)
```

The nonzero requirement belongs to the concrete scalar semantics. The current experiment therefore marks its unchecked witness constructor as unsafe instead of inventing a generic zero test that would be false for some carriers.

## A projective point is not its coordinate tuple

The current projective type is

```text
ComplexProjectivePoint complex n
```

A value carries a nonzero homogeneous representative, but semantically

```text
[z₀ : … : zₙ] = [λz₀ : … : λzₙ]    for every λ ∈ ℂ×.
```

So raw component equality is the wrong equality relation.

The Idriç design makes this deliberate:

- there is no ordinary `Eq` instance for projective points;
- there is no vector addition on projective points;
- there is no pointwise multiplication pretending that ℂPⁿ is a vector space;
- projective equality is expressed through explicit nonzero common rescaling.

The witness operation is

```text
projective_rescaling_witness
```

For a chosen nonzero λ, it certifies that multiplying every coordinate of one representative by λ gives the other representative.

This is exactly the quotient

```text
ℂPⁿ = (ℂⁿ⁺¹ ∖ {0}) / ℂ×.
```

## Affine coordinates are a chart, not the whole projective space

The current affine embedding is

```text
affine_to_projective
```

implementing

```text
(z₁,…,zₙ) ↦ [1 : z₁ : … : zₙ].
```

The inverse chart operation is partial:

```text
projective_first_chart
```

It divides by the first homogeneous coordinate only when that coordinate is nonzero.

Thus in ℂP¹,

```text
[0 : 1]
```

is outside that finite chart. It is the usual point at infinity. The type/API therefore does not erase the difference between an affine coordinate system and the projective space that contains it.

## Relation to the named-space type system

The adjacent Idriç higher-mathematics work also keeps **dimension** separate from **space identity**.

`SpaceName rank` names a space while recording its rank. `FiniteSpace` packages such a named space. Two spaces may both have rank 2 without being the same space: the ordinary plane and an image plane are intentionally distinct examples.

Vector and covector samples are indexed by the full named space:

```text
ExactVectorSample space
ExactCovectorSample space
```

so a covector from one rank-2 space cannot contract with a vector from a different rank-2 space merely because their coordinate counts match.

Variance is also typed:

```text
IndexedValue Lower space
IndexedValue Upper space
```

and contraction requires opposite variance in the same named space.

## A metric is additional structure

A finite coordinate space does not acquire a dot product merely from its dimension.

The Euclidean layer therefore has

```text
EuclideanStructure space
```

and operations such as lowering an index, raising an index, dot product, norm, and distance require that structure explicitly.

This prevents the common mathematical mistake of identifying vectors with covectors before a metric has actually been chosen.

## O and SO are typed by the structured space

The same design continues into orthogonal transformations:

```text
OrthogonalTransform structure orientation
```

where orientation is either preserving or reversing, and

```text
SpecialOrthogonal structure
```

contains only orientation-preserving orthogonal transformations.

The Euclidean structure itself is an index. Consequently an orthogonal transformation on one named space/metric cannot silently be reused on another rank-equal space.

Composition tracks orientation in the result type: two reversing transformations compose to a preserving one.

## The bridge to realification

For ordinary complex linear algebra, forgetting complex scalars turns

```text
ℂⁿ
```

into a real vector space of dimension `2n` together with a distinguished operator

```text
J² = −I,
```

where `J` means multiplication by `i`.

In coordinates,

```text
J = [ 0  −I ]
    [ I   0 ].
```

A real linear transformation represents a complex-linear transformation exactly when it commutes with `J`.

Thus the useful type-theoretic statement is not simply

```text
ComplexCoordinates complex n ≈ RealCoordinates (2n).
```

The right mathematical statement is closer to

```text
complex n-space
    ↔
real 2n-space + a chosen complex structure J with J² = −I.
```

The extra `J` is what remembers which doubled real operations are genuinely complex-linear.

## What a point of ℂPⁿ becomes over ℝ

This sharpens the previous realification discussion.

A point of ℂPⁿ is a **complex line** in ℂⁿ⁺¹. After forgetting complex scalars, that line becomes a 2-dimensional real plane in ℝ²ⁿ⁺² that is invariant under `J`.

So there is a useful equivalent description:

```text
point of ℂPⁿ
    ↔
J-invariant real 2-plane in ℝ²ⁿ⁺².
```

Common multiplication by λ ∈ ℂ× does not change this plane. In real coordinates, λ = a+ib acts within it by

```text
[ a  −b ]
[ b   a ].
```

That matrix is a positive scaling combined with a planar rotation. Projectivization removes both of those redundant choices of homogeneous representative.

This gives a concrete real interpretation of `projective_rescaling_witness`: it certifies a change of real basis inside the same `J`-invariant 2-plane, not a move to a different projective point.

## Important dimension warning

ℂPⁿ has real dimension `2n`, but that does **not** mean

```text
ℂPⁿ = ℝ²ⁿ.
```

The first affine chart looks like ℂⁿ ≅ ℝ²ⁿ, but it does not cover the entire projective space.

The simplest case makes this visible:

```text
ℂP¹ ≅ S²,
```

not ℝ². Removing the projective point `[0:1]` leaves the ordinary complex plane as one chart.

## The sphere/phase quotient

Every nonzero homogeneous representative can first be normalized to unit length. The remaining common scalar freedom is then only complex phase.

Consequently

```text
ℂPⁿ ≅ S²ⁿ⁺¹ / S¹.
```

This is the Hopf quotient viewpoint. It cleanly separates two pieces of the `ℂ×` projective redundancy:

```text
ℂ× ≅ ℝ₊ × S¹
```

as magnitude and phase.

This is also why projective geometry and rotations meet naturally without being the same subject.

## Where SO enters — and where it does not

A unitary matrix `U ∈ U(n)` becomes, after realification, an orthogonal real matrix in `SO(2n)`.

More precisely, the realified unitary group is the subgroup of `SO(2n)` that also commutes with `J`:

```text
U(n) ≅ { Q ∈ SO(2n) | QJ = JQ }.
```

So not every real rotation is a complex unitary transformation. The `J` compatibility is the missing condition.

Likewise the natural effective unitary symmetry group of ℂPⁿ is

```text
PU(n+1) = U(n+1) / U(1),
```

because a common scalar phase acts trivially on projective points.

This is the clean bridge between the working themes `ℂPⁿ` and `SO(n)`: unitary transformations sit inside doubled-dimensional real rotations, while projectivization removes the scalar phase that does not change a complex line.

## Connection back to complex change of basis

If

```text
A′ = P⁻¹ A P
```

is a complex change of basis, then after realification

```text
Φ(A′) = Φ(P)⁻¹ Φ(A) Φ(P).
```

The real change-of-basis matrix `Φ(P)` commutes with `J`. If `P` is unitary, `Φ(P)` is additionally orthogonal and orientation-preserving.

So the numerical-linear-algebra use of complex unitary bases has a precise doubled-real interpretation:

```text
complex unitary basis change
    ↔
real orthogonal basis change that preserves J.
```

That is stronger than merely saying that complex matrices can be doubled into real matrices.

## Hermitian structure after realification

There is one further useful structural split. A Hermitian form on a complex vector space determines two compatible real bilinear forms:

- its real part is a real inner product `g`;
- its imaginary part is a skew form `ω`;
- the complex structure `J`, metric `g`, and skew form `ω` determine one another compatibly (up to the sign convention used for the Hermitian form).

So complex numerical linear algebra can be read over the reals as more than a doubled matrix: it carries a metric plus the distinguished quarter-turn `J`, and therefore also the associated symplectic structure.

This is another reason not to erase the complex type too early merely because a real storage representation exists.

## Current source trail

Primary current design:

- `isomorphisms/Idric` PR #81, **Establish complex-coordinate and projective semantics**
- branch `math/complex-projective-semantics`
- `_/examples/unified-higher-mathematics/ComplexProjective.idric`
- `_/examples/unified-higher-mathematics/ComplexProjectiveTests.idric`
- `_/fixtures/complex-projective/float32.json`

Related named-space / Euclidean typing:

- `_/examples/unified-higher-mathematics/MathematicalSpaces.idric`
- `_/examples/unified-higher-mathematics/EuclideanGeometry.idric`

Historical projective experiment:

- branch `projective-spaces`
- `experiments/projective-spaces/Projective.idric`

The historical experiment already distinguished homogeneous representatives, affine points, coordinate charts, chart membership, and chart boundary. PR #81 is the more current source for the complex-coordinate/projective semantic boundary and should be preferred when the two overlap.

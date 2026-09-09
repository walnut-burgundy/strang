# Geometry → conditioning → factorization choice

This note is the narrow numerical bridge from the repository's current work on realification, rotations/reflections, Householder geometry, and projective structure to practical algorithm choice.

It is not a survey of QR, LU, and SVD. The point is to record which numerical consequences follow from the geometry already present here.

## 1. Orthogonal/unitary geometry is a stability primitive

If `Q` is orthogonal or unitary, then

```text
||Qx||₂ = ||x||₂
||Q||₂ = ||Q⁻¹||₂ = 1
κ₂(Q) = 1.
```

So an exact rotation, reflection, or unitary change of basis does not itself amplify 2-norm perturbations.

This is the numerical reason Householder reflectors and Givens rotations matter beyond their geometric interpretation: they can change coordinates and create zeros without introducing an intrinsically ill-conditioned transformation.

That does **not** mean every implementation made from rotations/reflections is automatically stable. Floating-point formulas for constructing and applying the factors can still lose accuracy. The useful distinction is:

```text
exact geometric map: isometry
floating implementation: must still earn a backward-error bound
```

## 2. Realification preserves the 2-norm conditioning question

For a complex matrix

```text
A = B + iC,
```

its realification is

```text
Φ(A) = [ B  -C ]
       [ C   B ].
```

The singular values of `Φ(A)` are the singular values of `A`, each repeated twice. Therefore

```text
||Φ(A)||₂ = ||A||₂
κ₂(Φ(A)) = κ₂(A)
```

when `A` is invertible.

So converting a complex linear problem into doubled real coordinates does not cure or worsen its intrinsic 2-norm conditioning. It changes representation and cost, not the underlying singular-value geometry.

Likewise, a complex unitary change of basis realifies to an orthogonal change of basis commuting with the complex structure `J`. The numerical isometry survives the representation change.

## 3. Projective equivalence does not imply numerical equivalence of representatives

A projective point is unchanged by nonzero rescaling:

```text
[z] = [λz],    λ ≠ 0.
```

Mathematically these are the same point. Numerically, the representatives can behave very differently: one may overflow, underflow, or waste most available precision while another is well scaled.

So projective code should separate:

```text
semantic equality: same projective point
numerical representation: choose a well-scaled representative
```

A standard practical move is to normalize or otherwise rescale homogeneous coordinates before sensitive calculations. The scale choice is a representation decision, not a change in the underlying geometry.

This is the same general lesson as the repository's conditioning/scaling note: bad scale is not the same thing as bad conditioning.

## 4. The first conditioning test comes from the SVD

For invertible `A`,

```text
κ₂(A) = σ_max(A) / σ_min(A).
```

This turns the geometric stretching picture into the practical question:

```text
Is the requested answer robustly determined by the data?
```

If `σ_min` is tiny relative to `σ_max`, no algorithm can manufacture a highly accurate inverse-sensitive answer from rounded data.

That is a property of the problem. Algorithm choice comes next.

## 5. QR, LU, and SVD solve different numerical problems

### Householder QR

Use Householder QR as the default dense route when the task is fundamentally orthogonalization or least squares and one wants to avoid avoidable amplification.

The important numerical facts are:

- Householder factors are orthogonal/unitary in exact arithmetic;
- the factorization can be interpreted backward-stably;
- least squares via QR avoids forming `A* A`;
- therefore it avoids automatically replacing `κ₂(A)` by `κ₂(A)²`.

For structured sparse updates or when only a few entries must be annihilated, Givens rotations may be preferable because they act locally. This is an execution-structure choice inside the same isometric family, not a different conditioning theory.

### LU with pivoting

LU is usually the practical dense choice for a general square nonsingular solve when least-squares geometry and rank revelation are not the main problem.

But elimination is not an isometry. Intermediate entries can grow, so pivoting and growth behavior matter. A stable triangular solve at the end does not by itself guarantee that the preceding elimination produced a small backward error.

The relevant planner question is therefore not merely

```text
Can A be factored as LU?
```

but

```text
What pivoting/growth behavior is expected for this matrix class?
```

### SVD

Use the SVD when singular values, numerical rank, near-null directions, or the conditioning itself are part of the answer.

It is the most explicit representation of the geometry:

```text
input direction
    → singular-vector coordinates
    → independent stretches σᵢ
    → output direction.
```

That makes near-rank-deficiency visible instead of forcing a binary nonsingular/singular decision too early.

The tradeoff is work: if the only task is a routine well-conditioned square solve, computing a full SVD is usually more machinery than needed.

## 6. One choice to reject early: normal equations by reflex

For least squares,

```text
A* A x = A* b
```

has

```text
κ₂(A* A) = κ₂(A)².
```

That identity is the compact bridge from singular-value geometry to algorithm selection.

It does not mean the normal equations always fail. It means they deliberately square the matrix condition number, so they should not be the default when QR can solve the original least-squares geometry without that transformation.

## 7. What to measure

Do not judge a factorization mainly by entrywise agreement with one preferred set of factors.

For numerical work, preserve at least:

```text
residual / reconstruction error
backward error
loss of orthogonality, when Q matters explicitly
estimated condition number
pivot growth, for LU-family methods
rank-revealing evidence, when rank is uncertain
```

Householder QR is the canonical warning here: individual computed factors can differ noticeably from a chosen exact factorization while their product still gives an excellent nearby factorization of `A`.

## 8. Minimal planner consequence

The rotation/reflection planner already distinguishes factor graphs, dependency depth, reuse, and target cost. The smallest numerical extension is to add only these questions before benchmarking:

```text
conditioning_effect
    does the reformulation preserve κ, square it, or otherwise change it?

backward_error_model
    is there a known nearby-problem interpretation for this algorithm?

orthogonality_loss
    does the task depend on Q remaining accurately orthogonal?

growth_risk
    can elimination produce large intermediates?

rank_revelation
    must the algorithm expose near-null directions rather than merely return a solve?
```

This keeps the planner tied to numerical consequences rather than turning it into a catalogue of matrix algorithms.

## Compact decision boundary

```text
least squares / orthogonalization
    → Householder QR by default
    → Givens when locality, sparsity, or updates dominate

square nonsingular solve
    → LU with an appropriate pivoting policy

rank uncertain / near singular / singular values are themselves needed
    → SVD

least squares formed through A* A only for convenience
    → reconsider; κ is squared
```

## Related repository notes

- [`trefethen-bau-complex-change-of-basis.md`](trefethen-bau-complex-change-of-basis.md) — complex bases, realification, and the complex structure `J`.
- [`householder-coxeter-ade.md`](householder-coxeter-ade.md) — Householder reflectors as Euclidean reflections.
- [`conditioning-scaling-and-range.md`](conditioning-scaling-and-range.md) — conditioning versus scaling, stability, and numeric format.
- [`trefethen-bau-numerical-linear-algebra.md`](trefethen-bau-numerical-linear-algebra.md) — conditioning, backward stability, Householder QR, least squares, and the normal-equations distinction.
- [`qr-factorization-flags.md`](qr-factorization-flags.md) — the geometric flag interpretation of QR.

## Sources

- Lloyd N. Trefethen and David Bau III, *Numerical Linear Algebra*. SIAM, 1997, especially Lectures 4–5, 7, 10, 12–19, and 20–22.
- Gene H. Golub and Charles F. Van Loan, *Matrix Computations*, 4th ed. Johns Hopkins University Press, 2013, especially Chapters 2, 3, and 5.

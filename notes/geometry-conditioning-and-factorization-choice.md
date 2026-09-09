# Geometry → conditioning → factorization choice

This is the smallest numerical bridge from the current realification, rotation/reflection, Householder, and projective notes to actual factorization choice. It is not a QR/LU/SVD survey.

## Three geometric facts with numerical consequences

### Orthogonal and unitary maps are 2-norm isometries

For orthogonal/unitary `Q`,

```text
||Qx||₂ = ||x||₂
||Q||₂ = ||Q⁻¹||₂ = 1
κ₂(Q) = 1.
```

So exact rotations, reflections, Householder factors, Givens factors, and unitary basis changes do not themselves amplify 2-norm perturbations. This is why the reflection geometry matters numerically.

It does **not** make every floating implementation automatically stable. The construction and application of the factors still need a backward-error argument.

### Realification preserves the conditioning question

For `A = B+iC`,

```text
Φ(A) = [ B  -C ]
       [ C   B ].
```

The singular values of `Φ(A)` are those of `A`, each repeated twice. Hence, for invertible `A`,

```text
||Φ(A)||₂ = ||A||₂
κ₂(Φ(A)) = κ₂(A).
```

Realification changes representation and cost, not intrinsic 2-norm conditioning. A complex unitary basis change becomes an orthogonal real basis change commuting with `J`, so the isometry survives too.

### Projective equality does not fix numerical scale

`[z] = [λz]` for every nonzero `λ`, but different representatives can overflow, underflow, or waste precision differently.

Therefore keep separate:

```text
semantic object: projective point
numerical representation: well-scaled homogeneous representative
```

Normalizing or rescaling homogeneous coordinates changes representation, not the projective point. This is scaling, not conditioning.

## Conditioning first, algorithm second

For invertible `A`,

```text
κ₂(A) = σ_max(A) / σ_min(A).
```

The SVD therefore answers the geometric question before algorithm choice: how much can the problem amplify perturbations in the data?

Stability is a separate question: what nearby problem did the floating computation actually solve?

For a backward-stable method, the useful schematic relation is

```text
forward error ≈ conditioning × backward error.
```

Do not blame the algorithm for sensitivity already present in the problem, and do not excuse an unstable algorithm merely because the problem is well conditioned.

## Practical factorization boundary

| Task | Default numerical route | Why this geometry matters |
| --- | --- | --- |
| Dense least squares / orthogonalization | Householder QR | Built from isometries; backward-stable factorization; avoids squaring `κ` |
| Sparse/local annihilation or factor updates | Givens QR | Same isometric geometry, but factors act in local coordinate planes |
| General square nonsingular solve | LU with an appropriate pivoting policy | Usually less machinery than QR/SVD, but elimination is not an isometry and pivot growth matters |
| Rank uncertain, nearly singular, or singular values/condition number are part of the answer | SVD | Directly exposes singular directions and near-null space instead of forcing an early nonsingular/singular decision |

One especially important rejection rule is the normal equations used merely for convenience:

```text
A* A x = A* b
κ₂(A* A) = κ₂(A)².
```

This does not mean the normal equations always fail. It means they deliberately square the matrix condition number, so they are not the neutral form of the original least-squares problem.

## Small extension to the rotation/reflection planner

The planner already considers factor shape, dependency depth, reuse, and target cost. Before benchmarking, add only:

```text
conditioning_effect
    preserve κ, square it, or otherwise change the problem?

backward_error_model
    known nearby-problem interpretation?

orthogonality_loss
    does the result depend on Q remaining accurately orthogonal?

growth_risk
    can elimination create large intermediate entries?

rank_revelation
    must near-null directions be exposed?
```

That is enough to connect geometric candidate generation to numerical reliability without turning the planner into an encyclopedia of matrix algorithms.

## What to record from an implementation

Prefer numerical evidence tied to the problem:

```text
residual / reconstruction error
backward error
loss of orthogonality when Q matters
condition estimate
pivot growth for LU-family methods
rank-revealing evidence when rank is uncertain
```

In particular, do not judge Householder QR mainly by entrywise agreement with one preferred exact `Q` and `R`; the factorization residual/backward error is the more meaningful test.

## Related notes

- [`trefethen-bau-complex-change-of-basis.md`](trefethen-bau-complex-change-of-basis.md)
- [`householder-coxeter-ade.md`](householder-coxeter-ade.md)
- [`conditioning-scaling-and-range.md`](conditioning-scaling-and-range.md)
- [`trefethen-bau-numerical-linear-algebra.md`](trefethen-bau-numerical-linear-algebra.md)
- [`qr-factorization-flags.md`](qr-factorization-flags.md)

## Sources

- Lloyd N. Trefethen and David Bau III, *Numerical Linear Algebra*. SIAM, 1997, especially Lectures 4–5, 7, 10, 12–19, and 20–22.
- Gene H. Golub and Charles F. Van Loan, *Matrix Computations*, 4th ed. Johns Hopkins University Press, 2013, especially Chapters 2, 3, and 5.

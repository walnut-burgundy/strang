# Lloyd N. Trefethen & David Bau III — *Numerical Linear Algebra*

## Edition and access

Working edition: **1997**, Lloyd N. Trefethen and David Bau III, SIAM, xii + 361 pages, ISBN 978-0-89871-361-9. A 25th-anniversary edition appeared in 2022.

Authorized public material:

- Author page: https://people.maths.ox.ac.uk/trefethen/text.html
- SIAM 1997 record: https://epubs.siam.org/doi/book/10.1137/1.9780898719574
- SIAM 2022 anniversary record: https://epubs.siam.org/doi/book/10.1137/1.9781611977165

The author's page provides front matter and the first five lectures as public samples. SIAM exposes metadata, excerpts, and some front/back matter.

**Mirror status: link only.** SIAM's copyright page states © 1997 SIAM and all rights reserved; the 2022 edition is likewise copyrighted. No redistribution license for the whole book was located.

## Why it belongs here

This is the compact conceptual core of numerical linear algebra. It is especially strong on geometry, orthogonality, conditioning, backward stability, eigenvalue algorithms, and Krylov methods. For GPU/compiler work, its most important habit is to ask whether a large condition number belongs to the mathematical problem or was created by an avoidable representation or algorithm.

The book is organized as forty short lectures grouped into six major parts. The summaries below follow those six parts; individual lecture notes can be added underneath as they are read.

## Part summaries

### I. Fundamentals

Begins with matrix-vector multiplication interpreted as linear combinations and develops the geometry of linear maps, orthogonality, singular values, and SVD. The practical lesson is to think of a matrix not as a rectangular pile of scalars but as an operator that stretches particular directions by particular amounts. SVD makes sensitivity visible: small singular values identify directions in which inversion or inference will amplify perturbations.

### II. QR Factorization and Least Squares

Develops projectors, Gram–Schmidt, Householder transformations, QR factorization, and least-squares problems. Orthogonal transformations are prized because they preserve Euclidean length and therefore tend not to manufacture numerical amplification. Householder QR becomes a model for a broader design principle: if two formulations are mathematically equivalent, prefer the one whose transformations preserve the quantities error analysis cares about.

Lecture 7 also exposes a direct geometric bridge to representation theory: the successive column spaces `span(a₁) ⊂ span(a₁,a₂) ⊂ ...` form a flag, and QR constructs an orthonormal frame adapted to exactly that flag. In the square invertible case, the upper-triangular factor is the `B` in the full flag variety `GL(n, ℂ)/B`. See [QR factorization, flags, and representation theory](../../notes/qr-factorization-flags.md), which also records David Vogan's MIT flag-manifold notes and distinguishes this notion from Razborov flag algebras and Robert Ghrist's flag complexes.

### III. Conditioning and Stability

Separates **conditioning of the problem** from **stability of the algorithm**. A well-conditioned problem can be damaged by an unstable implementation; an ill-conditioned problem cannot be made intrinsically well-conditioned merely by better code. Backward error asks whether the computed answer is the exact answer to a nearby problem, often giving a more useful diagnostic than raw forward error. This part is directly relevant to the Holomorphic zoom issue: an arbitrary viewport clamp is not a substitute for showing that the evaluation is backward/forward stable over the desired scale range.

### IV. Systems of Equations

Treats Gaussian elimination, LU factorization, pivoting, and related solution strategies. Pivoting is not decorative bookkeeping: it is a way to keep intermediate growth under control. The chapter shows why one must analyze intermediate quantities, not just the exact algebraic formula for the final answer. It also gives a concrete template for replacing an implementation restriction with a stability mechanism.

### V. Eigenvalues

Reviews eigenvalue/eigenvector mathematics and develops computational routes toward Schur forms and QR-type algorithms. Eigenvalue problems expose a recurring subtlety: sensitivity depends strongly on normality and eigenvector geometry, so a small residual does not always imply a small eigenvalue/eigenvector error. Pseudospectral thinking grows naturally from this distinction even when it is not the principal topic of the original edition.

### VI. Iterative Methods

Moves to large-scale problems where direct factorization is too expensive or unnecessary. Krylov subspaces, Arnoldi, Lanczos, conjugate-gradient-type ideas, and GMRES-style methods build useful approximations from repeated matrix-vector products. Convergence depends not only on dimension but on spectral structure and preconditioning. The engineering lesson is that representation can be algorithmic: sometimes the best way to 'store' an inverse or factorization is not to form it at all, but to expose an operator that progressively solves the required action.

## Numerical rules worth carrying into code

- Keep condition number attached to the **problem definition**, not used as a vague synonym for 'floating point is scary.'
- Prefer norm-preserving or well-scaled transformations when available.
- Track residual, forward error, and backward error separately.
- Avoid explicit inverses when a solve/factorization expresses the operation more stably and cheaply.
- Analyze growth of intermediates, not just final exact formulas.
- Treat preconditioning as a change of representation designed to make the numerical problem easier without changing the desired solution.

## Bibliography, acknowledgments, and thanks

The 1997 source trail has now been extracted from SIAM's public front/back matter:

- [Complete 1997 bibliography](bibliography.md) — all bibliography entries from pp. 343–352, with the three printed `et al.` entries expanded to full contributor lists.
- [Acknowledgments ledger](acknowledgments.md) — every person explicitly named in the book's acknowledgments, plus collectively credited groups and the dedication.
- [Thanks](thanks.md) — an explicit author/editor/translator and acknowledgment-contributor thank-you list.

The next bibliographic step for this book is not to re-transcribe the same references; it is to connect individual lecture notes to the bibliography entries they actually depend on.

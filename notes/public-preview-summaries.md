# Public-preview summaries for the Paleologo numerical-methods books

Checked 2026-09-09.

This note records what can be learned from **lawfully public previews and author/publisher sample material** without mirroring the copyrighted books themselves. The rule is simple: link the public sample, summarize it independently, and distinguish a real text preview from a publisher description or table of contents.

## Trefethen & Bau — *Numerical Linear Algebra*

Official author page:

- https://people.maths.ox.ac.uk/trefethen/text.html

SIAM record:

- https://epubs.siam.org/doi/book/10.1137/1.9781611977165

Trefethen's own page exposes the front matter and **Lectures 1–5** as public samples. These five lectures make up the book's opening fundamentals sequence.

### Lecture 1 — Matrix-vector multiplication

The main conceptual move is to stop treating `Ax` merely as a collection of row-by-column scalar formulas. Read it as a linear combination of the columns of `A`, with the entries of `x` as coefficients. That viewpoint immediately connects matrix multiplication with coordinates, bases, range spaces, and the effect of elementary row/column operations.

For computation this is a useful change of level: many numerical algorithms are easier to understand as operations on whole rows, columns, and subspaces than as manipulations of individual entries.

### Lecture 2 — Orthogonal vectors and matrices

Orthogonality is introduced through inner products, orthogonal and orthonormal sets, and unitary/orthogonal matrices. An orthonormal basis gives coordinates by inner products, while a unitary matrix preserves inner products, lengths, and angles.

That preservation property is one reason orthogonal transformations recur throughout numerical linear algebra. They change representation without needlessly magnifying Euclidean geometry, which later makes Householder and Givens transformations natural building blocks for stable algorithms.

### Lecture 3 — Norms

Norms turn qualitative ideas such as "large", "small", and "close" into quantities that can be used in error analysis. The lecture distinguishes vector norms from matrix/operator norms and develops the idea that a matrix norm measures the largest amplification produced by a linear map relative to the chosen vector norm.

This is the bridge from linear algebra to numerical analysis: once the size of perturbations and their amplification can be measured, conditioning and stability become precise questions rather than warnings about floating point in general.

### Lecture 4 — Singular value decomposition

The SVD is motivated geometrically. A matrix maps the Euclidean unit sphere to an ellipse or higher-dimensional ellipsoid: the right singular vectors give special input directions, the singular values give the stretching factors, and the left singular vectors give the corresponding output directions.

Thus `A = U Σ V*` separates a general linear transformation into an orthogonal/unitary change of coordinates, axis-aligned scaling, and another orthogonal/unitary change of coordinates. Unlike an eigenvalue decomposition, this works naturally for rectangular matrices as well as square ones.

### Lecture 5 — More on the SVD

The SVD is then used as a change-of-basis theorem: with the right singular vectors as coordinates in the domain and the left singular vectors as coordinates in the range, the action of `A` becomes diagonal scaling by `Σ`.

The important computational consequences include:

- reading rank, range, and nullspace information from singular values/vectors;
- obtaining the matrix 2-norm from the largest singular value;
- representing a matrix as a sum of rank-one pieces;
- obtaining optimal low-rank approximations in the 2-norm and Frobenius norm by truncating the SVD;
- using the SVD when accuracy is more important than the cheaper alternatives supplied by QR or other factorizations.

This preview is especially relevant to the existing notes on complex basis changes and realification: the SVD explicitly allows different orthonormal bases for domain and range, which is exactly why an arbitrary rectangular linear map can be made diagonal in suitable coordinates.

Related local notes:

- `notes/trefethen-bau-numerical-linear-algebra.md`
- `notes/trefethen-bau-complex-change-of-basis.md`
- `notes/qr-factorization-flags.md`

## Cheney & Light — *A Course in Approximation Theory*

AMS page:

- https://bookstore.ams.org/GSM/101

Google Books publisher preview:

- https://books.google.com/books/about/A_Course_in_Approximation_Theory.html?id=II6DAwAAQBAJ

The AMS page publicly exposes the preface, full table of contents, and supplemental-material links. Google Books exposes selected pages plus searchable contents/reference metadata. This is enough to summarize the architecture of the book, but not enough to pretend that every chapter has been read in full.

### What the public preview establishes

The book is deliberately **multivariate**. It starts with interpolation and progressively replaces isolated formulas with operator and function-space viewpoints.

The opening sequence develops:

- interpolation and linear interpolation operators;
- the Lagrange operator and node placement;
- multivariate polynomials;
- projections and tensor-product interpolation;
- Newton and Lagrange interpolation paradigms.

The middle of the book moves toward kernel methods and reconstruction:

- interpolation by translates of a single function;
- positive-definite and strictly positive-definite functions;
- completely monotone functions;
- Schoenberg and Micchelli interpolation theorems;
- positive-definite functions on spheres;
- approximation/reconstruction and tomography;
- convolution and "good kernels";
- ridge functions and ridge-function approximation;
- artificial neural networks.

The later chapters connect approximation to geometry and multiscale representation:

- Chebyshev centers and optimal reconstruction;
- algorithmic orthogonal projections;
- cardinal B-splines and sinc;
- Hilbert function spaces and reproducing kernels;
- spherical thin-plate splines and box splines;
- wavelets;
- quasi-interpolation.

The public description also reports **438 problems/exercises** and a bibliography of almost **600 items**. That bibliography is valuable for `strang`: it gives a route from textbook exposition back to primary papers and earlier books rather than leaving the methods as anonymous folklore.

### Numerical direction to extract

The most useful computational questions suggested by this preview are not just "which interpolant?" but:

- how node choice affects amplification and conditioning;
- when an interpolation operator or projection has a large norm;
- how positive-definite kernels guarantee or fail to guarantee solvable interpolation systems;
- how reconstruction, ridge functions, splines, and wavelets trade locality, smoothness, dimension, and computational cost;
- how the same approximation can behave differently in exact mathematics and finite precision.

## Golub & Van Loan — *Matrix Computations*

Publisher page:

- https://www.press.jhu.edu/books/title/10678/matrix-computations

Public Cornell course material by Charles Van Loan:

- https://www.cs.cornell.edu/courses/cs621/

I did **not** find a publisher-hosted free chapter preview comparable to Trefethen's first five lectures. The lawful public material is still useful: Johns Hopkins exposes a detailed table of contents, description of the fourth-edition changes, and bibliography/resource links; Van Loan's public course pages show how selected parts of the material are actually organized in teaching.

### What the publisher preview establishes

The fourth edition explicitly expands the book in directions that matter for modern computation:

- fast transforms;
- parallel LU;
- discrete Poisson solvers;
- pseudospectra;
- structured linear-equation problems;
- structured eigenvalue problems;
- large-scale SVD methods;
- polynomial eigenvalue problems;
- tensor computations.

The detailed public contents divide the book into twelve large blocks:

1. matrix multiplication, structure, blocking, locality, fast transforms, and parallel multiplication;
2. matrix analysis, norms, SVD, subspace metrics, sensitivity, and finite precision;
3. general linear systems, LU, pivoting, roundoff, refinement, and parallel LU;
4. structured/special systems, including positive-definite, banded, Toeplitz, circulant, Vandermonde, and Poisson problems;
5. orthogonalization, QR, and least squares;
6. weighted, constrained, total, regularized, and updateable least-squares problems;
7. unsymmetric eigenvalue problems, Schur/Hessenberg reductions, practical QR, generalized problems, and pseudospectra;
8. symmetric eigenvalue problems and SVD computation;
9. functions of matrices, including exponential, sign, square root, and logarithm;
10. large sparse eigenvalue/SVD methods, including Lanczos and other Krylov methods;
11. large sparse linear systems, conjugate gradients, preconditioning, and multigrid;
12. displacement structure, structured rank, Kronecker products, and tensor decompositions.

The Cornell syllabus reinforces the implementation emphasis: matrix operations and data organization are followed by fast transforms and structured/sparse factorizations rather than treating every matrix as an unstructured dense object.

### Status

The existing chapter summaries in `books/golub-van-loan/README.md` should therefore be read as **public-contents-derived summaries**, not as claims that the copyrighted chapter text was freely available. If an authorized chapter sample appears later, add a deeper summary and preserve its exact source URL here.

## General rule for future books

Whenever an author or publisher offers a lawful preview:

1. record exactly what is public — full chapter, excerpt, front matter, table of contents, sample code, or course notes;
2. summarize the mathematics in our own words;
3. cross-link the summary to related `strang` topics;
4. do not turn "freely viewable" into "freely redistributable";
5. if the preview has references, use it to expand `BIBLIOGRAPHY.md` and preserve intellectual ancestry.

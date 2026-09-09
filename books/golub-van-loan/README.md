# Gene H. Golub & Charles F. Van Loan — *Matrix Computations*

## Edition and access

Working edition: **4th edition**, Gene H. Golub and Charles F. Van Loan, Johns Hopkins University Press, 2013, 784 pages, ISBN 978-1-4214-0794-4.

Authorized public material:

- Publisher page and full table of contents: https://www.press.jhu.edu/books/title/10678/matrix-computations
- Cornell course material by Charles Van Loan: https://www.cs.cornell.edu/courses/cs621/

**Mirror status: link only.** The current edition is commercially published by Johns Hopkins University Press. No open redistribution license for the complete book was located. Public course notes and publisher material may be linked; do not infer that course distribution of selected chapters grants public redistribution rights.

## Why it belongs here

This is the large reference work behind much of modern matrix computation. Where Trefethen–Bau emphasizes a compact conceptual path, Golub–Van Loan gives a wide algorithmic inventory, implementation details, perturbation theory, structure-exploiting methods, sparse methods, and extensive literature pointers.

## Chapter summaries

### 1. Matrix Multiplication

Starts from the basic kernels—matrix-vector and matrix-matrix products—and immediately asks how structure, blocking, fast transforms, memory locality, vectorization, and parallelism change the real computation. This chapter is a reminder that arithmetic count alone is not a performance model: data movement and representation dominate many modern machines.

### 2. Matrix Analysis

Develops the linear-algebra and norm machinery needed for error analysis, including SVD, subspace metrics, sensitivity of square systems, and finite-precision computation. It supplies the vocabulary for distinguishing geometric sensitivity from implementation error. Singular values provide a direct measure of directional expansion/compression and therefore of inversion sensitivity.

### 3. General Linear Systems

Covers triangular solves, LU, Gaussian-elimination roundoff, pivoting, accuracy improvement, condition estimation, and parallel LU. The chapter turns Gaussian elimination from symbolic algebra into a numerical algorithm whose intermediate growth and pivot choices matter. Iterative refinement and condition estimation show how to diagnose and repair accuracy rather than merely accept a factorization result.

### 4. Special Linear Systems

Exploits diagonal dominance, symmetry, positive definiteness, bandedness, block tridiagonal form, Vandermonde structure, Toeplitz structure, circulant structure, and discrete Poisson structure. The central lesson is to preserve problem structure because generic dense algorithms throw away both speed and often numerical insight.

### 5. Orthogonalization and Least Squares

Uses Householder and Givens transformations to build QR and solve full-rank, rank-deficient, square, and underdetermined least-squares problems. Orthogonal transformations are a recurring stability primitive because they do not magnify the 2-norm. Rank deficiency forces the algorithm to confront which components are actually determined by the data.

### 6. Modified Least Squares Problems and Methods

Extends least squares to weighting, regularization, constraints, total least squares, SVD-based subspace methods, and factorization updates. This chapter is especially useful when a naive inverse amplifies noise: regularization changes the estimation problem explicitly instead of hiding instability behind numerical clipping.

### 7. Unsymmetric Eigenvalue Problems

Develops decompositions, perturbation theory, power iterations, Hessenberg and Schur reductions, the practical QR algorithm, invariant subspaces, generalized eigenproblems, structured eigenproblems, and pseudospectra. Nonnormal matrices make eigenvalue sensitivity highly geometric; pseudospectra expose behavior that eigenvalues alone conceal.

### 8. Symmetric Eigenvalue Problems

Uses symmetry to obtain stronger theory and more specialized algorithms: symmetric QR, tridiagonal methods, Jacobi methods, SVD computation, and symmetric generalized eigenproblems. Symmetry is not just a convenience—it changes conditioning, available transformations, and the quality of attainable algorithms.

### 9. Functions of Matrices

Studies how to compute functions such as the exponential, sign, square root, and logarithm of a matrix. The important numerical question is not merely how the scalar function is defined but which matrix representation or approximation avoids unstable diagonalization or excessive work.

### 10. Large Sparse Eigenvalue Problems

Introduces Lanczos, quadrature connections, practical restarting/selection issues, large sparse SVD frameworks, unsymmetric Krylov methods, and Jacobi–Davidson-type methods. The target is often only a few spectral quantities, so forming a dense decomposition is wasteful; matrix-vector actions become the primitive operation.

### 11. Large Sparse Linear System Problems

Covers sparse direct methods, classical iterations, conjugate gradients, other Krylov methods, preconditioning, and multigrid. Preconditioning is central: the same exact solution can be embedded in a much better-conditioned iterative problem. Multigrid shows that scale itself can be used algorithmically rather than treated as a source of numerical failure.

### 12. Special Topics

Treats displacement structure, structured rank, Kronecker products, tensor unfoldings/contractions, and tensor decompositions/iterations. The chapter broadens the idea of numerical linear algebra from two-dimensional dense arrays to structured operators and higher-order data, again emphasizing that representation determines feasible algorithms.

## Twelve matrix-computation habits worth preserving

Van Loan's Cornell teaching emphasizes concrete matrix-operation identities: matrix-vector multiplication as column combinations, diagonal multiplication as row/column scaling, and avoiding explicit construction of diagonal or rank-one matrices. The broader principle is to compute the requested action directly instead of manufacturing an intermediate object simply because the algebra permits it.

## Bibliographic leads

The publisher page identifies extensive global references and literature pointers. A complete bibliography-author inventory still needs to be transcribed from an authorized copy/back matter.

Important names already directly tied to topics in the book include **Householder, Givens, Schur, Gershgorin, Cholesky, Lanczos, Arnoldi, Jacobi, Davidson, Krylov, Francis**, and the many researchers behind QR, SVD, conjugate gradients, multigrid, pseudospectra, structured matrices, and tensor methods.

## Thanks

Thanks to **Gene H. Golub** and **Charles F. Van Loan** for building a reference that treats matrix computation as both mathematics and craft. Thanks to every author in its unusually rich literature trail. The long-term goal of this directory is to preserve that trail explicitly, not reduce decades of work to anonymous algorithm names.

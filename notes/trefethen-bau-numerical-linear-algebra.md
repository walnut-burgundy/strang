# Trefethen & Bau — Numerical Linear Algebra

Source notes on Lloyd N. Trefethen and David Bau III, **Numerical Linear Algebra**, with emphasis on the book's organization and the approximation-theoretic view of Krylov methods.

## Primary source

- Lloyd N. Trefethen and David Bau III, **Numerical Linear Algebra**, SIAM, Philadelphia, 1997, xii + 361 pp. ISBN 978-0-89871-361-9.
  - Trefethen's Oxford page: https://people.maths.ox.ac.uk/trefethen/text.html
  - SIAM front matter / contents: https://epubs.siam.org/doi/pdf/10.1137/1.9780898719574.fm
  - SIAM back matter / notes: https://epubs.siam.org/doi/pdf/10.1137/1.9780898719574.bm
- Lloyd N. Trefethen and David Bau III, **Numerical Linear Algebra: Twenty-Fifth Anniversary Edition**, SIAM, 2022. ISBN 978-1-61197-715-8; eISBN 978-1-61197-716-5.
  - DOI: https://doi.org/10.1137/1.9781611977165

Originating pointer supplied with these notes:

- https://x.com/i/status/2020121652495839602

The X post itself was not retrievable while preparing this note, so claims below are grounded in the book and SIAM/Oxford material rather than attributed to the post.

Accessed 2026-09-09.

## Why this book is distinctive

Trefethen describes the book as aiming for **beauty, depth of insight, and brevity**. It is organized as forty short lectures, each about eight pages, based on courses he taught at MIT and Cornell.

The important structural choice is the ordering. The book develops orthogonality, the SVD, projectors, and QR near the beginning. **QR factorization is Lecture 7; Gaussian elimination is not introduced until Lecture 20.** Thus direct elimination is not allowed to become the conceptual definition of numerical linear algebra.

That ordering makes orthogonality and approximation recurring ideas rather than isolated techniques.

## Lecture map

### I. Fundamentals — Lectures 1–5

1. Matrix-Vector Multiplication
2. Orthogonal Vectors and Matrices
3. Norms
4. The Singular Value Decomposition
5. More on the SVD

### II. QR Factorization and Least Squares — Lectures 6–11

6. Projectors
7. QR Factorization
8. Gram-Schmidt Orthogonalization
9. MATLAB
10. Householder Triangularization
11. Least Squares Problems

### III. Conditioning and Stability — Lectures 12–19

12. Conditioning and Condition Numbers
13. Floating Point Arithmetic
14. Stability
15. More on Stability
16. Stability of Householder Triangularization
17. Stability of Back Substitution
18. Conditioning of Least Squares Problems
19. Stability of Least Squares Algorithms

### IV. Systems of Equations — Lectures 20–23

20. Gaussian Elimination
21. Pivoting
22. Stability of Gaussian Elimination
23. Cholesky Factorization

### V. Eigenvalues — Lectures 24–31

24. Eigenvalue Problems
25. Overview of Eigenvalue Algorithms
26. Reduction to Hessenberg or Tridiagonal Form
27. Rayleigh Quotient, Inverse Iteration
28. QR Algorithm without Shifts
29. QR Algorithm with Shifts
30. Other Eigenvalue Algorithms
31. Computing the SVD

### VI. Iterative Methods — Lectures 32–40

32. Overview of Iterative Methods
33. The Arnoldi Iteration
34. How Arnoldi Locates Eigenvalues
35. GMRES
36. The Lanczos Iteration
37. From Lanczos to Gauss Quadrature
38. Conjugate Gradients
39. Biorthogonalization Methods
40. Preconditioning

## The approximation-theoretic thread

The last part of the book is especially relevant to the broader Strang notes because it treats iterative linear algebra through **polynomials acting on matrices** rather than as a bag of unrelated recurrence formulas.

Arnoldi constructs a Krylov subspace

```text
K_n(A, b) = span{b, Ab, A²b, ..., Aⁿ⁻¹b}.
```

Every vector in this space can be viewed as `p(A)b` for a polynomial `p` of bounded degree. This makes polynomial approximation a natural language for asking what an iterative method can accomplish after `n` matrix-vector products.

The book's cover itself illustrates this viewpoint: it shows **polynomial lemniscates in the complex plane** from successive Arnoldi steps. Lecture 34 develops this picture for eigenvalue convergence. The notes to that lecture explicitly say that the lemniscate treatment is nonstandard and point to the connection with polynomial approximation, ideal Arnoldi, and GMRES polynomials.

This is the useful conceptual bridge:

```text
matrix iteration
    ↓
Krylov subspace
    ↓
p(A)b
    ↓
polynomial approximation on the spectrum / relevant complex-plane set
```

For GMRES the polynomial formulation becomes particularly explicit. Greenbaum and Trefethen describe GMRES and Arnoldi as minimization problems involving `||p(A)b||`, with different normalizations of `p`. The approximation problem explains convergence in terms of how well a polynomial of limited degree can behave on the matrix.

This is why complex approximation theory appears naturally inside numerical linear algebra rather than as a decorative side subject.

## Structural lessons worth retaining

### Orthogonality before elimination

QR, projectors, Gram-Schmidt, Householder transformations, and least squares form one coherent block before Gaussian elimination appears. This makes numerical linear algebra look less like symbolic equation solving and more like geometry plus approximation under finite precision.

### Conditioning and stability are separate questions

The book deliberately separates:

- sensitivity of the mathematical problem (**conditioning**), from
- behavior of the numerical algorithm (**stability**).

A bad answer can come from an ill-conditioned problem even when the algorithm is stable; an unstable algorithm can spoil a well-conditioned problem. Keeping those notions distinct is one of the central habits of numerical analysis.

### SVD is foundational, not an afterthought

The SVD appears in Lectures 4–5, before QR, least squares, elimination, and eigenvalue algorithms. It supplies geometry for rank, approximation, norms, and conditioning early enough to be reused throughout the book.

### Iterative methods are finite approximation problems

After a fixed number of matrix-vector products, a Krylov method has access only to a restricted polynomial family. Convergence is therefore constrained by approximation theory. This viewpoint connects Arnoldi, GMRES, Lanczos, conjugate gradients, eigenvalue approximation, and preconditioning.

### Complex-plane geometry matters

For nonsymmetric matrices, eigenvalues alone may not tell the full numerical story. The Arnoldi/GMRES discussion leads naturally toward polynomial level sets, lemniscates, and later pseudospectral ideas. Trefethen's treatment makes the geometry visible rather than hiding it behind recurrence coefficients.

## Related citation trail

The book's notes for the Arnoldi/GMRES discussion point into a wider approximation-theory literature. A particularly direct companion paper is:

- Anne Greenbaum and Lloyd N. Trefethen, **“GMRES/CR and Arnoldi/Lanczos as Matrix Approximation Problems”**, *SIAM Journal on Scientific Computing* 15 (1994), no. 2, pp. 359–368. DOI: https://doi.org/10.1137/0915025

Its central formulation is that GMRES and Arnoldi can both be understood through minimization of matrix-polynomial expressions. This gives a clean route from numerical linear algebra to polynomial approximation in the complex plane.

For the broader complex-approximation direction, see also:

- Mark Embree and Lloyd N. Trefethen, **“Green's Functions for Multiply Connected Domains via Conformal Mapping”**, *SIAM Review* 41 (1999), no. 4, pp. 745–761. DOI: https://doi.org/10.1137/S0036144598349277

That work explicitly connects Green functions and polynomial approximation in the complex plane to matrix iterations.

## Short index for later use

- SVD: Lectures 4–5, 31
- projectors: Lecture 6
- QR: Lecture 7
- Gram-Schmidt: Lecture 8
- Householder: Lectures 10, 16
- least squares: Lectures 11, 18–19
- conditioning: Lectures 12, 18
- floating point: Lecture 13
- stability: Lectures 14–17, 19, 22
- Gaussian elimination: Lectures 20–22
- Cholesky: Lecture 23
- eigenvalues / QR algorithm: Lectures 24–30
- Arnoldi: Lectures 33–34
- GMRES: Lecture 35
- Lanczos: Lectures 36–37
- conjugate gradients: Lecture 38
- preconditioning: Lecture 40

## Bibliography

- Trefethen, Lloyd N., and David Bau III. **Numerical Linear Algebra**. Philadelphia: Society for Industrial and Applied Mathematics, 1997. ISBN 978-0-89871-361-9.
- Trefethen, Lloyd N., and David Bau III. **Numerical Linear Algebra: Twenty-Fifth Anniversary Edition**. Philadelphia: Society for Industrial and Applied Mathematics, 2022. DOI: 10.1137/1.9781611977165.
- Greenbaum, Anne, and Lloyd N. Trefethen. **“GMRES/CR and Arnoldi/Lanczos as Matrix Approximation Problems.”** *SIAM Journal on Scientific Computing* 15, no. 2 (1994): 359–368. DOI: 10.1137/0915025.
- Embree, Mark, and Lloyd N. Trefethen. **“Green's Functions for Multiply Connected Domains via Conformal Mapping.”** *SIAM Review* 41, no. 4 (1999): 745–761. DOI: 10.1137/S0036144598349277.

## Source handling note

This file is a set of research notes and a topic map, not a reproduction of the textbook. Bibliographic citations and stable source links are retained so later notes can trace claims back to the book and its related literature.

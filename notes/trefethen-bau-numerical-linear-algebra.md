# Trefethen & Bau — Numerical Linear Algebra

Source notes on Lloyd N. Trefethen and David Bau III, **Numerical Linear Algebra**, with emphasis on the book's organization, its conditioning/stability framework, and the approximation-theoretic view of Krylov methods.

## Primary source

- Lloyd N. Trefethen and David Bau III, **Numerical Linear Algebra**, SIAM, Philadelphia, 1997, xii + 361 pp. ISBN 978-0-89871-361-9.
  - Trefethen's Oxford page: https://people.maths.ox.ac.uk/trefethen/text.html
  - SIAM front matter / contents: https://epubs.siam.org/doi/pdf/10.1137/1.9780898719574.fm
  - SIAM back matter / notes: https://epubs.siam.org/doi/pdf/10.1137/1.9780898719574.bm
- Lloyd N. Trefethen and David Bau III, **Numerical Linear Algebra: Twenty-Fifth Anniversary Edition**, SIAM, 2022. ISBN 978-1-61197-715-8; eISBN 978-1-61197-716-5.
  - DOI: https://doi.org/10.1137/1.9781611977165
  - Part III, *Conditioning and Stability*: https://doi.org/10.1137/1.9781611977165.ch3

Originating pointer supplied with these notes:

- https://x.com/i/status/2020121652495839602

The X post itself was not retrievable while preparing this note, so claims below are grounded in the book and SIAM/Oxford material rather than attributed to the post.

Accessed 2026-09-09.

## Why this book is distinctive

Trefethen describes the book as aiming for **beauty, depth of insight, and brevity**. It is organized as forty short lectures, each about eight pages, based on courses he taught at MIT and Cornell.

The important structural choice is the ordering. The book develops orthogonality, the SVD, projectors, and QR near the beginning. **QR factorization is Lecture 7; Gaussian elimination is not introduced until Lecture 20.** Thus direct elimination is not allowed to become the conceptual definition of numerical linear algebra.

That ordering makes orthogonality, conditioning, stability, and approximation recurring ideas rather than isolated techniques.

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

## Conditioning and stability — working notes

Part III is not merely a detour about floating-point arithmetic. It supplies the framework used to decide whether a numerical answer is bad because the **problem itself is sensitive** or because the **algorithm needlessly amplified error**.

### Conditioning belongs to the problem

Think of a mathematical problem as a map

```text
f : data → solution.
```

A condition number measures how strongly the exact solution can change when the exact input data are changed slightly. In relative terms the local picture is

```text
relative change in solution
--------------------------------  ≈  κ
relative change in data
```

for the worst small perturbation direction.

Thus conditioning is present before an algorithm is chosen. An ill-conditioned problem has nearby data with substantially different exact answers. No numerical method can manufacture information that the data do not determine robustly.

For an invertible matrix in the 2-norm,

```text
κ(A) = ||A||₂ ||A⁻¹||₂
     = σ_max(A) / σ_min(A).
```

The SVD therefore turns matrix conditioning into geometry: a matrix is ill-conditioned when it stretches some directions much more than others, equivalently when its smallest singular value is small relative to its largest.

A useful rule of thumb is that a condition number around `10^k` can put roughly `k` decimal digits at risk. This is only a scale estimate, not a guarantee that exactly that many digits will be lost in every instance.

### Floating point supplies small local perturbations

Lecture 13 introduces the standard floating-point model: elementary operations behave like exact operations followed by a small relative perturbation, on the scale of machine precision `ε_machine` (away from exceptional cases such as overflow/underflow).

The important analytical move is not to count rounding errors indiscriminately. It is to ask what mathematical problem the computed answer actually solves.

### Forward error and backward error answer different questions

**Forward error** asks:

```text
How far is the computed answer from the exact answer to the original data?
```

**Backward error** asks:

```text
How much would the input data have to change
so that the computed answer became exactly correct?
```

Backward error is often the more revealing quantity. A result can have a large forward error while having a tiny backward error when the underlying problem is ill-conditioned.

This produces the central diagnostic separation:

```text
large forward error
    │
    ├── the problem may be ill-conditioned
    │
    └── the algorithm may be unstable
```

One should not blame an algorithm merely because the forward answer is inaccurate.

### Backward stability

A backward-stable algorithm returns the exact answer to a nearby problem, with the required perturbation in the input on the scale of rounding error:

```text
computed answer = f(data + δdata)

||δdata|| / ||data|| = O(ε_machine).
```

This is powerful because it separates the algorithmic question from the conditioning question. Once the algorithm has been shown backward stable, ordinary perturbation analysis of the mathematical problem tells us what forward accuracy is possible.

The key estimate developed in Lecture 15 is, schematically,

```text
relative forward error = O(κ · ε_machine)
```

for a backward-stable algorithm applied to a problem with condition number `κ`.

That relation is one of the central organizing ideas of numerical analysis:

```text
floating-point perturbation
          │
          ▼
backward error ≈ ε_machine
          │
          ▼
condition number κ of the problem
          │
          ▼
forward error ≈ κ ε_machine
```

### Why backward analysis is better than blindly accumulating roundoff

A direct forward analysis tries to follow every rounding error through every intermediate operation. This can produce complicated bounds that obscure cancellations and structure.

Backward error analysis instead tries to reinterpret the whole computation as an exact computation on slightly perturbed data. When this succeeds, the remaining amplification is exactly the sensitivity already inherent in the mathematical problem.

This is especially important in matrix algorithms, where intermediate quantities may look inaccurate even though the final factorization or solution has a very small residual.

### Householder QR: inaccurate factors can still give an excellent factorization

Lecture 16 uses Householder triangularization as the main backward-error example. The computed factors satisfy a relation of the form

```text
Q̃ R̃ = A + δA,

||δA|| / ||A|| = O(ε_machine).
```

Thus Householder QR is backward stable as a factorization of `A`.

A subtle point is that `Q̃` and `R̃` individually need not be close to some preselected exact `Q` and `R`. The map from `A` to particular factors can itself be ill-conditioned. Large forward errors in the individual factors therefore do **not** imply instability.

What matters for backward stability is that their product reconstructs a matrix extremely close to the input matrix. Errors in the two factors can be strongly correlated and cancel in the product.

This is an important general lesson: **do not judge a factorization algorithm solely by entrywise errors in intermediate factors. Check the residual / backward error.**

### Back substitution is another backward-stable piece

Lecture 17 analyzes solving an upper-triangular system by back substitution. The computed solution can be interpreted as the exact solution of a nearby triangular system,

```text
(R + δR) x̃ = b,

||δR|| / ||R|| = O(ε_machine)
```

(up to dimension-dependent constants in the usual fixed-dimension asymptotic notation).

This matters compositionally. A linear solve based on

```text
A
 ↓ Householder QR
Q R
 ↓ apply Q*
y
 ↓ back substitution
x
```

is built from stable pieces. Combining backward stability with the conditioning of `Ax = b` gives the familiar forward-error scale

```text
||x̃ - x|| / ||x|| = O(κ(A) ε_machine).
```

An inaccurate answer for a highly ill-conditioned `A` can therefore be exactly what a good algorithm should be expected to produce.

### Least squares makes the distinction more subtle

Lecture 18 emphasizes that “the condition number of least squares” is not one number until we specify both:

- which data are perturbed (`A`, `b`, or both), and
- which output is being judged (the coefficient vector `x` or the fitted vector `y = Ax`).

For full-rank least squares,

```text
min_x ||b - Ax||₂,
```

three geometric quantities recur:

- `κ = κ(A)`;
- `θ`, the angle measuring how far `b` lies from `range(A)`, with `tan θ` related to residual size relative to fitted-vector size;
- a scaling parameter often denoted `η`, comparing `||A|| ||x||` with `||Ax||`.

For sensitivity of the coefficient vector `x` to perturbations in `A`, the bound contains terms with scales

```text
κ
```

and

```text
κ² tan(θ) / η.
```

So least-squares conditioning can lie on scales ranging from roughly `κ` to roughly `κ²`, depending on the geometry of the fit. A close fit (`θ` small) can make the underlying coefficient problem much better conditioned than the normal equations would suggest.

### Why the normal equations are dangerous

The normal-equations method solves

```text
A* A x = A* b.
```

In the 2-norm,

```text
κ(A* A) = κ(A)².
```

This is the crucial defect. Even if Cholesky (or another solver for the normal equations) is stable **for the system it is given**, forming and solving the normal equations has transformed the least-squares problem into one whose matrix condition number is squared.

For some least-squares instances the original problem is already conditioned on the `κ²` scale, so this does not necessarily cost more than the problem itself demands. But for important cases—especially ill-conditioned problems with close fits—the original least-squares problem can have sensitivity closer to `κ` while the normal equations expose the computation to `κ²` amplification.

Hence the important distinction:

```text
stable solution of the normal equations
        ≠
stable general-purpose solution of the original least-squares problem.
```

Lecture 19 therefore treats the normal equations as unstable as a general-purpose least-squares algorithm.

### QR and SVD are the safer general-purpose least-squares routes

Householder QR avoids squaring the condition number and gives a backward-stable least-squares method. The SVD also gives a stable method and makes near-rank-deficiency explicit through the singular values.

A straightforward modified Gram-Schmidt least-squares implementation that explicitly relies on the computed `Q` being accurately orthogonal can be unstable because loss of orthogonality matters. Trefethen and Bau also show that reformulating the computation—rather than simply treating “Gram-Schmidt” as one indivisible algorithm—can restore stability. The exact computational organization matters.

### Stability is a property of an algorithm over a problem class

A numerically questionable algorithm may work perfectly on one lucky matrix. Conversely, a backward-stable algorithm can give few correct digits on an ill-conditioned matrix.

So a single successful or unsuccessful numerical example is not, by itself, a stability theorem. The correct questions are:

```text
1. How sensitive is this mathematical problem?
2. What nearby problem did this computation actually solve?
3. Is the required backward perturbation O(ε_machine)?
4. Does the observed forward error match κ × backward_error?
```

This is a useful diagnostic template well beyond linear algebra.

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
- forward/backward error and stability: Lectures 14–15
- backward error analysis: Lecture 15
- Householder QR stability: Lecture 16
- back-substitution stability: Lecture 17
- least-squares conditioning: Lecture 18
- least-squares algorithm stability / normal equations: Lecture 19
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

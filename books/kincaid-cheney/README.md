# David Kincaid & Ward Cheney — *Numerical Analysis: Mathematics of Scientific Computing*

## Edition and access

Working edition: **3rd edition**, David Kincaid and Ward Cheney, American Mathematical Society, © 2002, 788 pages. The authors' University of Texas site identifies ISBN 978-0-8218-4788-6 and provides the table of contents, errata, sample programs, links, and purchase information.

Authorized public material:

- Author/book site: https://web.ma.utexas.edu/CNA/NA3/
- Table of contents: https://web.ma.utexas.edu/CNA/NA3/toc.html
- Sample programs: https://web.ma.utexas.edu/CNA/NA3/sample.html
- Errata: https://web.ma.utexas.edu/CNA/NA3/errata.html

**Mirror status: link only.** The book site states © 2002 AMS. No open license or author/publisher permission for redistribution of the full book was located. The associated sample software is separately published; its license must be checked independently before copying code.

## Why it belongs here

This is the broad numerical-analysis spine of the repository. It connects floating-point behavior and conditioning to nonlinear equations, linear systems, eigenproblems, approximation, quadrature, differential equations, PDEs, linear programming, and optimization. For the current Holomorphic zoom work, Chapters 2, 4, and 5 are particularly relevant because they separate bad problem conditioning from bad numerical realization.

## Chapter summaries

### 1. Mathematical Preliminaries

Sets up the analytical language used to reason about algorithms: Taylor expansion, convergence rates, and difference equations. The important computational lesson is that an algorithm is not merely a sequence of instructions; its error behavior is usually controlled by an approximation theorem plus a recurrence describing how errors propagate.

### 2. Computer Arithmetic

Introduces floating-point representation, absolute and relative error, loss of significance, stability, instability, and conditioning. The central distinction is between a problem that is intrinsically sensitive and an algorithm that needlessly magnifies perturbations. This chapter is directly relevant to removing arbitrary range clamps from numerical software: first rescale or reformulate the computation, then choose adequate precision, rather than treating a small permitted input interval as a substitute for analysis.

### 3. Solution of Nonlinear Equations

Develops bisection, Newton, secant, fixed-point iteration, polynomial root methods, and continuation/homotopy. The methods trade guaranteed progress, local speed, derivative information, and basin-of-attraction behavior. A recurring theme is that a formally high-order method can be inferior when initialization, scaling, derivative quality, or multiple roots make its assumptions poor.

### 4. Solving Systems of Linear Equations

Moves from matrix algebra to LU and Cholesky factorization, pivoting, norms, error analysis, iterative refinement, stationary iterations, steepest descent, conjugate gradients, and roundoff analysis of Gaussian elimination. The useful viewpoint is to study both the mathematical condition of the system and the backward/forward behavior of the chosen factorization. Iterative refinement is especially important as an example of recovering accuracy without blindly increasing precision everywhere.

### 5. Selected Topics in Numerical Linear Algebra

Covers the power method, Schur and Gershgorin theory, orthogonal factorizations, least squares, SVD, pseudoinverses, and Francis's QR algorithm. Orthogonal transformations are central because they tend to preserve norms and therefore avoid gratuitous amplification. SVD supplies a geometrically transparent way to see rank, near-rank-deficiency, and directional sensitivity.

### 6. Approximating Functions

Treats polynomial, Hermite, spline and B-spline interpolation; Taylor series; least-squares and Chebyshev approximation; multidimensional interpolation; continued fractions; trigonometric interpolation; FFT; and adaptive approximation. The chapter emphasizes representation choice: mathematically equivalent forms can have radically different approximation quality, conditioning, and computational cost.

### 7. Numerical Differentiation and Integration

Develops finite-difference differentiation, Richardson extrapolation, interpolation-based quadrature, Gaussian quadrature, Romberg integration, adaptive quadrature, Sard's approximation-of-functionals viewpoint, and Euler–Maclaurin methods. Differentiation typically magnifies noise while integration smooths it, so the numerical treatment of the two operations is fundamentally asymmetric.

### 8. Numerical Solution of Ordinary Differential Equations

Covers existence/uniqueness, Taylor methods, Runge–Kutta, multistep methods, local/global error, stability, systems, boundary-value problems, shooting, finite differences, collocation, linear ODEs, and stiffness. Stability regions and stiffness show why local truncation error alone cannot determine a usable time step or method.

### 9. Numerical Solution of Partial Differential Equations

Introduces explicit and implicit methods for parabolic equations, finite differences, Galerkin/Ritz methods, characteristics, hyperbolic equations, multigrid, and fast Poisson solvers. The chapter connects discretization to the algebraic structure eventually presented to a linear solver; choosing the discretization and choosing the solver are not independent decisions.

### 10. Linear Programming and Related Topics

Builds from convexity and systems of inequalities to linear programming and the simplex algorithm. The important conceptual shift is from solving equations to navigating feasible regions defined by inequalities, with geometry and dual constraints becoming as important as arithmetic.

### 11. Optimization

Surveys one-dimensional search, descent, quadratic objectives, quadratic fitting, Nelder–Mead, simulated annealing, genetic algorithms, convex programming, constrained minimization, and Pareto optimization. The mixture of deterministic and heuristic methods is useful evidence that no single numerical paradigm dominates once objective functions become nonsmooth, multimodal, noisy, or constrained.

## Bibliographic leads to preserve

The book's full bibliography should be transcribed as bibliographic data from a lawful copy or authorized back matter. Until that is completed, do not pretend this list is exhaustive.

Authors and traditions explicitly implicated by the table of contents include Taylor, Newton, Cholesky, Neumann, Gershgorin, Schur, Francis, Hermite, Chebyshev, Fourier, Richardson, Romberg, Gauss, Sard, Bernoulli, Euler, Maclaurin, Runge, Kutta, Galerkin, Ritz, Nelder, Mead, and Pareto.

## Thanks

Thanks first to **David Kincaid** and **Ward Cheney** for organizing a very large part of scientific computing into one coherent mathematical treatment. Thanks also to the authors, mathematicians, and numerical analysts whose methods and results are cited throughout the book. As the bibliography is recovered, names and citations should be added rather than collapsed into anonymous 'standard methods.'

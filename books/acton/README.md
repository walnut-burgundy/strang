# Forman S. Acton — *Numerical Methods That Work*

## Edition and access

Working edition: **1990 MAA reissue**, Forman S. Acton, Mathematical Association of America, 549 pages, ISBN 0-88385-450-3 / 978-0-88385-450-1. The work was originally published in 1970 by Harper & Row; the MAA edition added a new preface and additional problems.

Public records:

- Google Books record and table of contents: https://books.google.com/books?id=cGnSMGSE5Y4C
- Open Library record / controlled borrowing: https://openlibrary.org/books/OL5444531M/Numerical_methods_that_work

**Mirror status: link only.** A borrowable Internet Archive/Open Library copy is not the same thing as a redistribution license. No open license or copyright-holder authorization for public mirroring of the complete book was located.

## Why it belongs here

Acton's book is useful precisely because it is less interested in presenting a spotless catalogue of formulas than in showing how computations fail. It repeatedly treats scaling, cancellation, singularities, extrapolation, approximation choice, and strategy as first-class numerical questions. That makes it a useful counterweight to code that 'solves' a numerical problem by clipping its inputs to a comfortable range.

## Chapter/section summaries

The 1990 table of contents exposed by Google Books contains the following major chapter-level headings. Some chapter numbering is obscured in the preview, so these notes preserve the visible headings rather than invent numbers.

### The Calculation of Functions

How to evaluate elementary or special functions accurately and economically, especially when a direct formula loses precision or wastes work. Representation is part of the algorithm: series, recurrences, rational approximations, transformations, and range reduction can represent the same mathematical function with very different numerical behavior.

### Roots of Transcendental Equations

Root finding is treated as a practical search problem rather than a ritual application of Newton's formula. Bracketing, iteration, stopping criteria, derivative behavior, and scale all matter. Robustness often comes from combining methods rather than insisting on one locally fast iteration everywhere.

### Interpolation—and All That

Interpolation can reconstruct or approximate data, but high degree, poor nodes, and naive formulas create severe sensitivity. The useful question is not merely whether an interpolating polynomial exists, but which representation and node strategy permit it to be evaluated without numerical self-destruction.

### Quadrature

Numerical integration balances smoothness assumptions, function evaluations, and error estimation. Adaptive behavior and transformed variables can matter more than increasing the nominal order of a fixed formula when the integrand contains sharp structure or inconvenient endpoints.

### Ordinary Differential Equations — first treatment

Introduces practical numerical integration of ODEs and the tension between local accuracy and global behavior. Step size, stability, and error propagation determine whether a method follows the intended solution rather than merely satisfying a local formula.

### Ordinary Differential Equations — further treatment

Returns to ODEs with more difficult cases and strategy. Predictor-corrector ideas, nonlinear behavior, and stiffness-type difficulties illustrate that the differential equation and the discretization together define the actual numerical problem.

### Strategy Versus Tactics

A central Acton theme: choosing the right mathematical reformulation is often more important than optimizing the inner loop of the wrong method. Numerical work should begin by changing variables, scales, representations, or decompositions until the computation has a healthy shape.

### Eigenvalues I

Introduces practical eigenvalue computation and reduction of a matrix problem to forms where iterative methods are effective. Structure and symmetry determine which transformations are worthwhile.

### Fourier Series

Uses Fourier representation as both analysis and computation. Spectral coefficients can turn differentiation, smoothing, convolution, or periodic approximation into algebraic operations, but truncation and representation choices determine accuracy.

### A Brief Cathartic Essay

A methodological interlude: numerical computation requires skepticism toward formulas that are algebraically correct but computationally foolish. It belongs in the repository because 'do not trust the obvious formula' is often the most valuable numerical rule.

### The Evaluation of Integrals

Revisits integration with attention to hard integrands, transformations, singular behavior, and error. The emphasis is on treating the integrand's analytic structure rather than blindly applying a standard quadrature table.

### Power Series and Continued Fractions

Compares representations that can have very different convergence domains and numerical efficiency. Continued fractions and rearranged series can remain effective in regions where a naive power series becomes slow or unusable.

### Economization of Approximations

Reduces the cost/degree of an approximation while controlling the error, closely related to Chebyshev ideas and minimax thinking. This is directly relevant to GPU work: a well-designed lower-degree approximation can outperform a high-degree expression both numerically and computationally.

### Eigenvalues II — Rotational Methods

Uses orthogonal rotations to transform eigenvalue problems while preserving norms. The chapter illustrates why orthogonal operations are such dependable numerical primitives: they change coordinates without introducing artificial stretching.

### Roots of Equations—Again

Returns to root finding after more tools are available. Difficult roots, polynomial structure, multiple roots, and alternative formulations show why convergence order by itself is an inadequate way to choose a solver.

### The Care and Treatment [of Ill-Conditioned / Difficult Computations]

A practical collection of ways to recognize and handle computations whose usual formulation magnifies errors. The key habit is diagnostic: determine where information is being lost and reformulate before spending more precision or iterations.

### Instability in Extrapolation

Extrapolation can turn a sequence of modest approximations into a much better answer, but it can also amplify noise and rounding catastrophically. The chapter is a warning that cancellation and coefficient growth must be analyzed before extrapolation is trusted.

### Minimum Methods

Treats numerical minimization and the practical geometry of searching an objective surface. Scaling of coordinates and curvature can make the difference between rapid progress and a method that wanders along a narrow valley.

### Network Problems

Shows how numerical techniques appear in problems with network/graph structure. Structure should be retained in the representation rather than flattened prematurely into an arbitrary dense system.

### Afterthoughts

Collects practical lessons that do not fit neatly into one algorithmic category. For this repository, the afterthoughts are part of the point: numerical competence is accumulated judgment about failure modes, not just a list of named algorithms.

## Bibliography

The 1970 Open Library record reports a bibliography on pp. 529–532; the 1990 Google Books contents places the bibliography at p. 537. A full citation transcription still needs to be made from a lawful copy. Bibliographic facts can be recorded here, but the book's prose and annotations should not be copied wholesale.

## Thanks

Thanks to **Forman S. Acton** for writing about numerical work as an activity that requires judgment. Thanks also to every researcher and author cited in his bibliography and historical discussions. As those names are recovered, this directory should credit them explicitly rather than treating Acton's sources as invisible background.

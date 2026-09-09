# Wegert's *Complex Beauties* — numerical-analysis notes

These notes cross-reference Elias Wegert's *Complex Beauties* calendars with the numerical-analysis themes collected in `strang`.

The calendars are not numerical-analysis textbooks. Their value here is that phase/domain coloring turns complex functions used by numerical methods into visible geometric objects: zeros, poles, branch cuts, convergence boundaries, interpolation error, rational filters, and iterates can often be seen directly.

## Source and reuse boundary

Official calendar/download index:

- TU Bergakademie Freiberg, *Complex Beauties*: https://blogs.hrz.tu-freiberg.de/mathekalender/english/

Background on the visualization method:

- Elias Wegert and Gunter Semmler, “Phase Plots of Complex Functions: a Journey in Illustration,” *Notices of the AMS* 58(6), 2011, 768–780. Preprint: https://arxiv.org/abs/1007.2295
- Elias Wegert, *Visual Complex Functions: An Introduction with Phase Portraits*, Birkhäuser, 2012. DOI: https://doi.org/10.1007/978-3-0348-0180-5
- Wikipedia: https://en.wikipedia.org/wiki/Domain_coloring

Do not vendor the calendar PDFs or artwork unless redistribution terms are established. Preserve links and bibliographic facts; make our own diagrams and summaries.

The detailed month-by-month lesson catalog is being developed in `yt-shorts`, especially the 2011–2022 follow-up catalog. `strang` should keep the numerical-method connections rather than duplicate every calendar biography.

## Why phase portraits belong in numerical analysis

For a complex function `f(z)`, coloring by `arg f(z)` and optionally drawing modulus contours makes several numerically important features visible:

- zeros and their multiplicities;
- poles and pole order;
- branch cuts and branch points;
- regions where an approximation behaves well or fails;
- zero clustering of polynomial or rational approximants;
- rational-filter transition regions;
- spectral information encoded by characteristic or resolvent-related functions;
- convergence boundaries of series;
- qualitative changes under iteration.

This is useful as a diagnostic layer. A numerical method may be derived on the real line while its stability, convergence, or approximation behavior is controlled by singularities and geometry in the complex plane.

## Approximation and interpolation

### Runge phenomenon and complex interpolation error

The 2024 Christoffel entry plots the complex interpolation error rather than merely a quadrature formula. The historical chain is:

Newton–Cotes → Gauss → Jacobi → Christoffel → orthogonal polynomials → Chebyshev nodes → Runge phenomenon.

A useful numerical experiment is to interpolate the same analytic function at equally spaced and Chebyshev-like nodes, then domain-color

`f(z) - p_n(z)`.

The real-axis error is only one slice. The complex portrait shows where the interpolant is being pulled by nearby singularities and how the chosen nodes become zeros of the error function.

References:

- https://en.wikipedia.org/wiki/Runge%27s_phenomenon
- https://en.wikipedia.org/wiki/Chebyshev_polynomials
- https://en.wikipedia.org/wiki/Gaussian_quadrature
- https://en.wikipedia.org/wiki/Elwin_Bruno_Christoffel

### Zolotarev rational approximation

Zolotarev's best rational approximation to `sign(x)` on two separated intervals is a particularly good bridge between classical approximation theory and engineering filters. The minimax solution is expressible through elliptic functions.

This connects:

Chebyshev minimax ideas → Zolotarev fractions → elliptic functions → Cauer/elliptic filters → Stiefel/Schwarz multiband filters → modern algebraic-curve constructions.

References:

- Yegor I. Zolotarev, 1877 work on elliptic functions and extremal rational approximation.
- https://en.wikipedia.org/wiki/Rational_approximation
- https://en.wikipedia.org/wiki/Elliptic_filter
- A. B. Bogatyrev, “Chebyshev representation of rational functions,” *Sbornik: Mathematics* 201 (2010), 1579–1598.

### Padé approximation and Stahl

The 2021 calendar includes Herbert Stahl's work on Padé approximants. A strong visual point is that poles of successive Padé approximants can reveal the natural branch-cut geometry of the analytically continued function.

This gives a direct numerical-analysis question:

> Can the poles of a rational approximant tell us where the original function cannot be continued as a single-valued analytic function?

The answer is often yes in a precise asymptotic sense, which is much richer than treating Padé approximation as “a better Taylor series.”

Reference starting point:

- https://en.wikipedia.org/wiki/Pad%C3%A9_approximant

## Filters as rational functions

The calendars contain several unusually useful filter entries.

### Butterworth

A Butterworth low-pass filter is maximally flat near zero frequency. The familiar magnitude-response plot hides the fact that the transfer function is rational and its pole geometry determines the response.

Reference:

- Stephen Butterworth, “On the Theory of Filter Amplifiers,” *Experimental Wireless and the Wireless Engineer* 7 (1930), 536–541.
- https://en.wikipedia.org/wiki/Butterworth_filter

### Cauer / elliptic

Elliptic filters trade passband/stopband ripple for a much sharper transition. Their connection to Zolotarev approximation makes them a direct approximation-theory object rather than an isolated electrical-engineering trick.

### Stiefel multiband filters

The 2024 Stiefel entry continues the same line from one passband/stopband to genuinely multiband rational approximation. The mathematical complexity rises from elliptic functions to higher-genus algebraic curves.

The comparison Butterworth → elliptic/Cauer → Stiefel is worth keeping as a single sequence in `strang` because it shows how an engineering specification becomes an extremal rational-function problem.

## Finite differences in the complex plane

The 2024 Jost Bürgi entry, contributed by Bengt Fornberg, associates a finite-difference stencil with a rational characteristic function whose poles are the stencil nodes and whose residues are the stencil weights.

This is a particularly clean example of “the numerical formula itself is a complex rational function.” A small complex stencil can be compared with wider real stencils by looking directly at that rational function.

Reference:

- Bengt Fornberg, “Generation of Finite Difference Formulas on Arbitrarily Spaced Grids,” *Mathematics of Computation* 51 (1988), 699–706.
- https://en.wikipedia.org/wiki/Finite_difference

This should be cross-read with standard finite-difference discussions of truncation error and with stiff-ODE stability regions: both are cases where the complex plane exposes behavior hidden by a real-axis derivation.

## Eigenvalues, resolvents, and Krylov methods

### Contour-integral eigensolvers

The 2024 Goursat entry uses a rational filter obtained from quadrature of a contour integral involving `T(z)^{-1}`. The numerical idea is to approximate a spectral projector by a rational function that is near one inside a contour and near zero outside.

This connects Cauchy/Goursat contour integration directly to modern nonlinear and large-scale eigensolvers.

Reference:

- Wolf-Jürgen Beyn, “An integral method for solving nonlinear eigenvalue problems,” *Linear Algebra and its Applications* 436 (2012), 3839–3863.

### Ritz values

The 2024 Walther Ritz entry compresses a large eigenproblem to a smaller projected problem. The displayed object is a characteristic polynomial associated with Ritz values obtained from a Krylov subspace.

Historical chain worth preserving:

Liouville? → Rayleigh → Ritz → Galerkin → Krylov → Lanczos/Arnoldi.

The attribution is layered; do not flatten it into “Ritz invented it.”

References:

- https://en.wikipedia.org/wiki/Rayleigh%E2%80%93Ritz_method
- Martin J. Gander and Gerhard Wanner, “From Euler, Ritz, and Galerkin to Modern Computing,” *SIAM Review*.

## Series, convergence boundaries, and zero clustering

Several entries make convergence failure visible through zeros of partial sums.

### Gregory / arctangent series

A high-degree Taylor partial sum of `arctan z` can look excellent on a real interval while its complex zero pattern exposes the geometry controlling convergence.

### Kapteyn series

Kapteyn series built from Bessel functions arise in the solution of Kepler's equation. Their partial sums show zeros accumulating near a convergence boundary.

Reference:

- Folkmar Bornemann, “A Jentzsch-Theorem for Kapteyn, Neumann, and General Dirichlet Series,” arXiv:2107.07207.

### Jentzsch

The relevant conceptual point is broader than a particular series: zeros of partial sums of a power series can accumulate on its circle of convergence. This is one of the cleanest visual demonstrations that convergence radius is a genuinely complex-plane phenomenon.

## Special functions as numerical objects

The calendars repeatedly connect numerical algorithms to special functions rather than treating special functions as a separate museum of named formulas.

Examples worth following in `strang`:

- Bessel functions and Kepler's equation;
- Airy and Pearcey integrals for caustics and oscillatory asymptotics;
- Painlevé functions in the Ising-model correlation problem;
- Barnes `G` in Toeplitz-determinant asymptotics;
- hypergeometric functions and the Riemann–Papperitz equation;
- elliptic functions in extremal rational approximation and filters;
- zeta values in Mirzakhani's moduli-space volume formulas.

The numerical theme is not merely evaluation. These functions often appear because they are the canonical analytic objects attached to a differential equation, asymptotic problem, extremal approximation, or spectral limit.

## Numerical experiment queue

1. Domain-color `f-p_n` for equally spaced versus Chebyshev interpolation nodes.
2. Animate zeros of Taylor partial sums approaching a convergence boundary.
3. Compare Butterworth and elliptic filters of equal order using both the real frequency response and the pole/zero phase portrait.
4. Domain-color a Zolotarev rational approximant and mark equioscillation points on the real line.
5. Plot the rational characteristic function associated with several real and complex finite-difference stencils.
6. Build a simple contour-integral rational filter and show its inside/outside transition in the complex plane.
7. Compare a full spectrum with Ritz values from growing Krylov subspaces.
8. Animate poles of Padé approximants approaching a Stahl-type branch cut for a multivalued test function.
9. Compare Bessel/Kapteyn partial sums with the exact function or orbital solution where feasible.
10. Treat phase portraits as test artifacts: when an implementation changes precision or backend, compare zero/pole/cut geometry as well as scalar error.

## Cross-repository note

`yt-shorts` is the detailed calendar/person/video catalog. `strang` should retain the numerical-analysis interpretation and bibliographic trails, especially when a calendar entry gives a useful visual test for approximation, stability, spectral methods, or special-function computation.

# Ward Cheney & Will Light — *A Course in Approximation Theory*

## Edition and access

Working citation: **Ward Cheney and Will Light, *A Course in Approximation Theory*, Graduate Studies in Mathematics 101, American Mathematical Society, 2009; originally published by Brooks/Cole in 2000, 359 pages.**

- Hardcover ISBN: 978-0-8218-4798-5.
- eBook ISBN: 978-1-4704-1165-7.
- AMS book page: https://bookstore.ams.org/GSM/101
- AMS copyright/end matter: https://www.ams.org/books/gsm/101/gsm101-endmatter.pdf

The AMS page provides the table of contents, preface, and supplemental material as public links.

**Mirror status: link only.** The AMS copyright notice permits ordinary fair use by individual readers and nonprofit libraries but says republication, systematic copying, or multiple reproduction requires an AMS license. Public availability of an AMS-hosted PDF is therefore not a general license to mirror it on GitHub.

- AMS rights/licensing: https://bookstore.ams.org/rights-licensing

## Why it belongs here

Giuseppe Paleologo includes Cheney–Light in his *Western Gappy Canon* as the approximation-theory book. His recommendation is useful because this is not merely a handbook of interpolation formulas: it develops approximation as a mathematical subject, especially multivariable approximation, and connects it to kernels, reconstruction, tomography, ridge functions, neural networks, splines, and wavelets.

Public mirror of the recommendation list used to verify the title:

- https://studylib.net/doc/27779816/gappycanon

That mirror establishes the recommendation, not redistribution rights for the book.

## Topic map

The AMS table of contents gives a useful extraction plan:

1. interpolation and linear interpolation operators;
2. optimization of the Lagrange operator;
3. multivariate polynomials;
4. projections and Boolean algebra of projections;
5. Newton and Lagrange interpolation paradigms;
6. interpolation by translates of a single function;
7. positive-definite and strictly positive-definite functions;
8. completely monotone functions;
9. Schoenberg and Micchelli interpolation theorems;
10. positive-definite functions on spheres;
11. reconstruction and tomography;
12. convolution and good kernels;
13. ridge functions and ridge-function approximation;
14. neural-network approximation;
15. Chebyshev centers and optimal reconstruction;
16. algorithmic orthogonal projections;
17. cardinal B-splines and sinc;
18. Hilbert function spaces and reproducing kernels;
19. spherical thin-plate splines and box splines;
20. wavelets;
21. quasi-interpolation.

## What to extract into `strang`

The repository should develop independent mathematical and computational notes rather than reproduce the exposition. Particularly useful targets are:

- interpolation error and node placement;
- projection as an approximation operator;
- positive-definite kernels and interpolation matrices;
- conditioning of interpolation systems;
- radial and ridge-function approximation;
- reproducing-kernel viewpoints;
- spline and wavelet constructions;
- approximation/reconstruction experiments where finite precision changes the expected behavior.

The book advertises a bibliography of almost 600 items. Bibliographic facts can be preserved as they are checked, with the primary literature credited rather than collapsing everything into the textbook citation.

# strang

Notes on numerical analysis, numerical linear algebra, and numerical methods.

The repository is named for Gilbert Strang, whose books and lectures have made linear algebra unusually readable and useful.

## Source policy

For every book:

1. Check whether the copyright holder or author has released a copy under terms that permit public redistribution.
2. If redistribution is clearly permitted, preserve the authorized source URL and license/provenance before mirroring anything.
3. Otherwise, do **not** copy the book into this repository. Link to the best lawful public source: author page, publisher page, DOI, library catalog, or controlled-lending record.
4. Write original chapter-by-chapter summaries rather than reproducing textbook prose.
5. Preserve edition, publisher, year, ISBN/DOI when known.
6. Copy bibliographic citations as bibliographic facts, and credit the people whose work the book relies on. Do not silently strip acknowledgments or intellectual ancestry.

## Initial books

- [Kincaid & Cheney — *Numerical Analysis: Mathematics of Scientific Computing*](books/kincaid-cheney/README.md)
- [Trefethen & Bau — *Numerical Linear Algebra*](books/trefethen-bau/README.md)
- [Golub & Van Loan — *Matrix Computations*](books/golub-van-loan/README.md)
- [Acton — *Numerical Methods That Work*](books/acton/README.md)

## Cross-cutting notes

The recurring subjects to extract across books are not just algorithms but numerical behavior:

- conditioning versus algorithmic stability;
- forward error, backward error, and residuals;
- scaling and nondimensionalization;
- floating-point range and precision;
- cancellation and loss of significance;
- stable polynomial/rational evaluation;
- orthogonal transformations;
- iterative refinement;
- eigenvalue and singular-value sensitivity;
- Krylov methods and preconditioning;
- approximation, interpolation, quadrature, and extrapolation;
- stiff differential equations;
- numerical methods whose failure modes matter more than their textbook formula.

This is intended to be useful to actual computational work, including GPU and compiler backends, rather than a transcription archive.

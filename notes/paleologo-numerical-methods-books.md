# Paleologo numerical-methods books

This note records the three books reconstructed from Giuseppe Paleologo's *Western Gappy Canon* and the public-source/redistribution check done on 2026-09-09.

## Identification

The intended three were:

1. **Ward Cheney & Will Light — *A Course in Approximation Theory*.**
2. **Lloyd N. Trefethen & David Bau III — *Numerical Linear Algebra*.**
3. **Gene H. Golub & Charles F. Van Loan — *Matrix Computations*, 4th ed.**

The half-remembered "Churchill and Light/Ward" clue was a collision between two different books:

- Cheney + Light is the approximation-theory book.
- James Ward Brown + Ruel V. Churchill wrote *Complex Variables and Applications*, a separate complex-analysis text.

The "T-something, two authors, one worked at Microsoft" clue points to Trefethen–Bau: SIAM's metadata for the 1997 edition lists David Bau III at Microsoft Corporation.

Public mirror of Paleologo's recommendation list used to verify the titles:

- https://studylib.net/doc/27779816/gappycanon

The recommendation can be summarized as follows:

- **Cheney–Light:** learn approximation theory as a real subject, especially useful in the setting of overcomplete models and for opening up less familiar approximation concepts.
- **Trefethen–Bau:** learn linear algebra through numerical linear algebra, with matrix decompositions and projections as central computational objects.
- **Golub–Van Loan:** use the major numerical-linear-algebra reference when a broader or deeper algorithmic treatment is needed.

## Redistribution result

None of the three has been found under a license that clearly permits this repository to republish book chapters or PDFs.

### Cheney & Light

Official sources:

- https://bookstore.ams.org/GSM/101
- https://www.ams.org/books/gsm/101/gsm101-endmatter.pdf
- https://bookstore.ams.org/rights-licensing

The AMS copyright notice permits ordinary fair use but reserves republication, systematic copying, and multiple reproduction to licensed uses.

**Action: link, do not mirror.**

### Trefethen & Bau

Official sources:

- https://people.maths.ox.ac.uk/trefethen/text.html
- https://doi.org/10.1137/1.9780898719574
- https://epubs.siam.org/page/terms
- https://epubs.siam.org/author-handbook

Trefethen's Oxford page publicly links the front matter and Lectures 1–5. That makes those samples easy to read lawfully, but no redistribution license was found there. SIAM's terms say republication/redistribution requires specific written permission unless the material is explicitly under an open license.

**Action: link the Oxford/SIAM material, do not mirror the PDFs merely because they are downloadable.**

### Golub & Van Loan

Official sources:

- https://press.jhu.edu/books/title/10678/matrix-computations
- https://www.press.jhu.edu/rights-permissions
- https://www.press.jhu.edu/rights-permissions/institutional-repository-use

Hopkins Press says written permission is required to reproduce its material in other publications, electronic products, or other media. Its limited institutional-repository allowance is for authors posting portions of their own contributions; it is not a general third-party redistribution license.

**Action: link, do not mirror.**

## What can go into this repository

The useful route is to reproduce the mathematics by doing our own work rather than copying the books' prose.

Good material includes:

- original notes and explanations;
- original worked examples;
- implementations of algorithms;
- numerical experiments showing conditioning, stability, sensitivity, and failure modes;
- mathematical facts, formulas, theorem statements, and algorithms expressed independently;
- bibliographic facts and links to authoritative sources;
- comparisons among books and methods.

Avoid copying substantial prose, figures, tables, exercise sets, or chapter PDFs unless an explicit license or written permission covers that use.

## How the three fit together

- **Cheney–Light:** approximation as a mathematical subject — interpolation, projections, positive-definite functions, kernels, reconstruction, ridge functions, splines, wavelets.
- **Trefethen–Bau:** compact conceptual numerical linear algebra — QR, least squares, conditioning and stability, eigenvalues, SVD, Krylov and iterative methods.
- **Golub–Van Loan:** deep reference treatment — matrix kernels, finite precision, structured systems, orthogonalization, least squares, eigenproblems, SVD, sparse/large-scale methods, tensors.

That combination gives one approximation-theory book, one compact numerical-linear-algebra book, and one comprehensive matrix-computation reference.

# Golub & Van Loan — stability, scaling, conditioning, and precision index

Source: G.H. Golub and C.F. Van Loan, *Matrix Computations (4th Edition): The Bibliography*, 1 Dec 2012, 66 pp.

Canonical bibliography PDF: https://bpb-us-e1.wpmucdn.com/blogs.cornell.edu/dist/c/9924/files/2021/11/GVL4_Bib.pdf

This is a **topical index into the complete bibliography**, not a replacement for the full citation corpus. It is useful immediately for the Holomorphic viewport/zoom problem because it separates mathematical sensitivity, scaling, rounding, backward error, and precision strategy.

## Scaling

- A.H. Al-Mohy and N.J. Higham (2009), “A New Scaling and Squaring Algorithm for the Matrix Exponential,” *SIAM J. Matrix Anal. Applic.* 31, 970–989.
- A.A. Anda and H. Park (1994), “Fast Plane Rotations with Dynamic Scaling,” *SIAM J. Matrix Anal. Applic.* 15, 162–174.
- V. Balakrishnan and S. Boyd (1995), “Existence and Uniqueness of Optimal Matrix Scalings,” *SIAM J. Matrix Anal. Applic.* 16, 29–39.
- F.L. Bauer (1963), “Optimally Scaled Matrices,” *Numer. Math.* 5, 73–87.
- R.D. Skeel (1979), “Scaling for Numerical Stability in Gaussian Elimination,” *J. ACM* 26, 494–526.

These are evidence for the general design rule: if range/scale is producing bad intermediates, change the representation or scaling law before imposing an arbitrary domain clamp.

## Backward error and backward stability

- P. Amodio and F. Mazzia (1999), “A New Approach to Backward Error Analysis of LU Factorization,” *BIT* 39, 385–402.
- M. Arioli, J.W. Demmel, and I.S. Duff (1989), “Solving Sparse Linear Systems with Sparse Backward Error,” *SIAM J. Matrix Anal. Applic.* 10, 165–190.
- X.S. Chen and W. Li (2008), “A Note on Backward Error Analysis of the Generalized Singular Value Decomposition,” *SIAM J. Matrix Anal. Applic.* 30, 1358–1370.
- A.J. Cox and N.J. Higham (1999), “Backward Error Bounds for Constrained Least Squares Problems,” *BIT* 39, 210–227.
- A.J. Cox and N.J. Higham (1999), “Row-Wise Backward Stable Elimination Methods for the Equality Constrained Least Squares Problem,” cited in the fourth-edition bibliography.
- C. de Boor and A. Pinkus (1977), “A Backward Error Analysis for Totally Positive Linear Systems,” *Numer. Math.* 27, 485–490.
- M. Gulliksson (1995), “Backward Error Analysis for the Constrained and Weighted Linear Least Squares Problem When Using the Weighted QR Factorization,” *SIAM J. Matrix Anal. Applic.* 16, 675–687.
- D.J. Higham and N.J. Higham (1992), “Backward Error and Condition of Structured Linear Systems,” *SIAM J. Matrix Anal. Applic.* 13, 162–175.
- D.J. Higham and N.J. Higham (1998), “Structured Backward Error and Condition of Generalized Eigenvalue Problems,” *SIAM J. Matrix Anal. Applic.* 20, 493–512.
- N.J. Higham (1993), “Perturbation Theory and Backward Error for AX - XB = C,” *BIT* 33, 124–136.
- M. Gu (1998), “Backward Perturbation Bounds for Linear Least Squares Problems,” *SIAM J. Matrix Anal. Applic.* 20, 363–372.
- J.-G. Sun (1995), “A Note on Backward Error Perturbations for the Hermitian Eigenvalue Problem,” *BIT* 35, 385–393.
- J.-G. Sun (1996), “Optimal Backward Perturbation Bounds for the Linear Least-Squares Problem with Multiple Right-Hand Sides,” *IMA J. Numer. Anal.* 16, 1–11.
- J.-G. Sun (1997), “On Optimal Backward Perturbation Bounds for the Linear Least Squares Problem,” *BIT* 37, 179–188.
- J.-G. Sun (1998), “Bounds for the Structured Backward Errors of Vandermonde Systems,” *SIAM J. Matrix Anal. Applic.* 20, 45–59.
- J.-G. Sun (2000), “Condition Number and Backward Error for the Generalized Singular Value Decomposition,” *SIAM J. Matrix Anal. Applic.* 22, 323–341.
- J.-G. Sun (2004), “A Note on Backward Errors for Structured Linear Systems,” *Numer. Lin. Alg.* 12, 585–603.
- F. Tisseur (2000), “Backward Error and Condition of Polynomial Eigenvalue Problems,” *Lin. Alg. Applic.* 309, 339–361.
- F. Tisseur (2003), “A Chart of Backward Errors for Singly and Doubly Structured Eigenvalue Problems,” *SIAM J. Matrix Anal. Applic.* 24, 877–897.
- J.M. Varah (1994), “Backward Error Estimates for Toeplitz Systems,” *SIAM J. Matrix Anal. Applic.* 15, 408–417.

## Condition numbers and condition estimation

- A.H. Al-Mohy and N.J. Higham (2009), “Computing the Fréchet Derivative of the Matrix Exponential, with an Application to Condition Number Estimation,” *SIAM J. Matrix Anal. Applic.* 30, 1639–1657.
- B.K. Alpert (1996), “Condition Number of a Vandermonde Matrix,” *SIAM Review* 38, 314.
- M. Arioli, M. Baboulin, and S. Gratton (2007), “A Partial Condition Number for Linear Least Squares Problems,” *SIAM J. Matrix Anal. Applic.* 29, 413–433.
- B. Beckermann (2000), “The Condition Number of Real Vandermonde, Krylov and Positive Definite Hankel Matrices,” *Numer. Math.* 85, 553–577.
- S.C. Brenner (1999), “The Condition Number of the Schur Complement in Domain Decomposition,” *Numer. Math.* 83, 187–203.
- C.G. Broyden (1973), “Some Condition Number Bounds for the Gaussian Elimination Process,” *J. Inst. Math. Applic.* 12, 273–286.
- A.K. Cline, C.B. Moler, G.W. Stewart, and J.H. Wilkinson (1979), “An Estimate for the Condition Number of a Matrix,” *SIAM J. Numer. Anal.* 16, 368–375.
- J.-P. Dedieu (1997), “Condition Operators, Condition Numbers, and Condition Number Theorem for the Generalized Eigenvalue Problem,” *Lin. Alg. Applic.* 263, 1–24.
- J.W. Demmel (1983), “The Condition Number of Equivalence Transformations that Block Diagonalize Matrix Pencils,” *SIAM J. Numer. Anal.* 20, 599–610.
- I.S. Dhillon (1998), “Reliable Computation of the Condition Number of a Tridiagonal Matrix in O(n) Time,” *SIAM J. Matrix Anal. Applic.* 19, 776–796.
- S. Gratton (1996), “On the Condition Number of Linear Least Squares Problems in a Weighted Frobenius Norm,” *BIT* 36, 523–530.
- C. Greif and J.M. Varah (2006), “Minimizing the Condition Number for Small Rank Modifications,” *SIAM J. Matrix Anal. Applic.* 29, 82–97.
- R.G. Grimes and J.G. Lewis (1981), “Condition Number Estimation for Sparse Matrices,” *SIAM J. Sci. Stat. Comput.* 2, 384–388.
- N.J. Higham (1986), “Efficient Algorithms for Computing the Condition Number of a Tridiagonal Matrix,” *SIAM J. Sci. Stat. Comput.* 7, 150–165.
- N.J. Higham (1987), “A Survey of Condition Number Estimation for Triangular Matrices,” *SIAM Review* 29, 575–596.
- J.M. Peña (2007), “Strict Diagonal Dominance and Optimal Bounds for the Skeel Condition Number,” *SIAM J. Numer. Anal.* 45, 1107–1108.

## Floating-point range, underflow, and roundoff

- N.N. Abdelmalek (1971), “Roundoff Error Analysis for Gram-Schmidt Method and Solution of Linear Least Squares Problems,” *BIT* 11, 345–368.
- J.W. Demmel (1984), “Underflow and the Reliability of Numerical Software,” *SIAM J. Sci. Stat. Comput.* 5, 887–919.
- J. Larson and A. Sameh (1978), “Efficient Calculation of the Effects of Roundoff Errors,” *ACM Trans.* (full citation in master bibliography).
- W. Miller (1975), “Computational Complexity and Numerical Stability,” *SIAM J. Comput.* 4, 97–107.
- W. Miller and D. Spooner (1978), “Software for Roundoff Analysis, II,” *ACM Trans. Math. Softw.* 4, 369–390.
- M. Wei and Q. Liu (2003), “Roundoff Error Estimates of the Modified Gram-Schmidt Algorithm with Column Pivoting,” *BIT* 43, 627–645.
- H. Wozniakowski (1978), “Roundoff-Error Analysis of Iterations for Large Linear Systems,” *Numer. Math.* 30, 301–314.
- H. Wozniakowski (1980), “Roundoff Error Analysis of a New Class of Conjugate Gradient Algorithms,” *Lin. Alg. Applic.* 29, 509–529.

## Mixed and extended precision

- M. Baboulin, A. Buttari, J. Dongarra, J. Kurzak, J. Langou, J. Langou, P. Luszczek, and S. Tomov (2009), “Accelerating Scientific Computations with Mixed Precision Algorithms,” *Comput. Phys. Commun.* 180, 2526–2533.
- X.S. Li, J.W. Demmel, D.H. Bailey, G. Henry, Y. Hida, J. Iskandar, W. Kahan, S.Y. Kang, A. Kapur, M.C. Martin, B.J. Thompson, T. Tung, and D.J. Yoo (2002), “Design, Implementation and Testing of Extended and Mixed Precision BLAS,” *ACM Trans. Math. Softw.* 28, 152–205.

## Iterative refinement

- R.D. Skeel (1980), “Iterative Refinement Implies Numerical Stability for Gaussian Elimination,” *Math. Comput.* 35, 817–832.
- M. Gulliksson (1994), “Iterative Refinement for Constrained and Weighted Linear Least Squares,” *BIT* 34, 239–253.
- F. Tisseur (2001), “Newton’s Method in Floating Point Arithmetic and Iterative Refinement of Generalized Eigenvalue Problems,” *SIAM J. Matrix Anal. Applic.* 22, 1038–1057.

## Immediate design lesson for `Holomorphic`

The bibliography itself shows why “just clamp the zoom” is numerically unsatisfactory. There are separate literatures for:

- scaling a representation;
- measuring conditioning;
- controlling backward error;
- choosing precision;
- handling underflow/roundoff;
- refining a result after a cheaper first computation.

A viewport bound should therefore be treated as a UI/design choice only after these numerical mechanisms have been exhausted, not as the mechanism that makes the computation numerically safe.

# Conditioning, scaling, and artificial range limits

A numerical program should not use a narrow input clamp as a substitute for understanding why its computation becomes unstable outside that interval.

## Four separate questions

When a computation fails as the scale changes, ask these in order:

1. **Is the mathematical problem ill-conditioned?**  Small input perturbations genuinely cause large output changes.
2. **Is the chosen representation badly scaled?**  The same problem is being expressed with unnecessarily huge or tiny intermediate quantities.
3. **Is the algorithm unstable?**  Rounding errors are amplified beyond what the conditioning of the problem requires.
4. **Is the numeric format inadequate?**  The algorithm is sound but the selected exponent range or precision is insufficient.

These are different defects and need different repairs.

## Range is not condition number

Large magnitude does not by itself mean high condition number. A value can be enormous and still be represented/evaluated stably if the computation uses appropriate scaling, logarithmic representations, normalization, or factored forms. Conversely, a moderate-looking expression can be catastrophically ill-conditioned near cancellation or a singularity.

A hard `min/max` range check hides this distinction.

## Prefer reformulation to clipping

Common repairs include:

- nondimensionalize coordinates;
- factor out a dominant scale before evaluation;
- keep products as sums of logarithms when only log-magnitude is needed;
- accumulate phase separately from magnitude;
- evaluate polynomials in a scaled coordinate or stable basis instead of raw monomials;
- use Horner/Clenshaw-like recurrences where appropriate;
- use orthogonal factorizations rather than normal equations when solving least squares;
- solve systems instead of explicitly forming inverses;
- normalize vectors during iterative methods;
- use preconditioning to change the numerical geometry of a solve;
- use mixed precision intentionally, promoting only sensitive paths;
- use iterative refinement when a low-precision factorization can still support a high-quality corrected solution.

## Backward-error target

A useful implementation contract is often:

> the computed result should be the exact result for a nearby input, with the size of the input perturbation controlled relative to machine precision and the chosen representation.

Forward error can then be interpreted through the condition number of the mathematical problem. This keeps blame assigned correctly: conditioning belongs to the problem; excess amplification belongs to the algorithm.

## Application to viewport zoom

For a renderer whose mathematical viewport changes scale, do not start with a zoom clamp. Treat viewport scale as part of the coordinate representation.

If a term such as

`q(z) = a1 z + a2 z^2 + ... + an z^n`

explodes merely because the viewport exposes large `|z|`, determine whether the desired mathematical model really demands raw monomial growth over that whole plane. If it does, evaluate the resulting phase/log-magnitude without forming unnecessary huge intermediates. If it does not, choose a scale-aware basis/model whose semantics match the intended deformation.

Remote poles/singularities that are specified relative to the viewport should likewise be represented in normalized viewport units and mapped to mathematical coordinates at evaluation time. Their distance from the visible boundary can then remain invariant under zoom without creating an arbitrary global coordinate cap.

## Sources in this repository

- Kincaid & Cheney, Chapter 2: floating point, loss of significance, stable/unstable computations, conditioning.
- Kincaid & Cheney, Chapters 4–5: norms, error analysis, refinement, orthogonal factorizations, SVD.
- Trefethen & Bau, Part III: conditioning and stability; Parts II/IV: orthogonal methods and stable linear solves.
- Golub & Van Loan, Chapters 2–6: finite precision, sensitivity, pivoting, refinement, orthogonalization, regularization.
- Acton: repeated practical treatment of scaling, cancellation, approximation choice, extrapolation instability, and strategy versus tactics.

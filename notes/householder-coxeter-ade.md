# Householder reflectors, Coxeter groups, and ADE

Cross-posted from the reflection/Dynkin work in `isomorphismes/coxeter`, especially:

- `notes/householder-reflectors.md`
- `notes/baez-ade-dynkin.md`

The numerical-linear-algebra entry point is Trefethen & Bau, Lecture 10.

## Householder as a reflection

For nonzero `v` in a real inner-product space,

`H(v) = I − 2 vvᵀ / (vᵀv)`.

Then:

- `H(v)² = I`;
- `H(v)v = −v`;
- every vector orthogonal to `v` is fixed.

So a Householder transformation is exactly reflection across the hyperplane `v⊥`.

Trefethen & Bau use these reflections constructively: choose the reflecting hyperplane so a column tail is sent to a multiple of `e₁`, introducing zeros while preserving the previously created zeros.

## From individual reflections to Coxeter groups

A generic sequence of Householder reflectors is not automatically a Coxeter system.

For two reflections with normals meeting at angle `θ`, their product is a rotation through `2θ`. If `θ = π/m`, then the product has order `m`, giving the Coxeter relation

`(sᵢ sⱼ)^m = 1`.

This is the direct bridge from metric geometry of reflecting hyperplanes to the abstract Coxeter presentation.

## Reflection groups, Weyl groups, ADE

Useful containment picture:

`Householder reflection`

`→ real reflection groups`

`→ finite Coxeter groups`

`→ crystallographic finite Coxeter groups = Weyl groups`

`→ simply-laced Weyl groups = ADE types`

The last steps add structure rather than merely renaming the same object. A Coxeter diagram records pairwise orders; Dynkin data also retains root-length information needed by Lie theory.

## Lacing

For crystallographic root systems:

- single edge: equal root lengths, `m = 3`;
- double edge: two root lengths, `m = 4`;
- triple edge: two root lengths, `m = 6`.

Simply-laced means only single edges, hence ADE.

## Where infinity enters in ordinary Euclidean reflection geometry

Two intersecting reflecting hyperplanes generate a rotation in their 2-dimensional normal plane. Two parallel reflecting hyperplanes generate a translation.

That gives the elementary affine mechanism: repeated products of the two parallel reflections give arbitrarily large translations, hence infinitely many group elements, even though everything still acts on ordinary Euclidean space.

This is the relevant infinite-Coxeter direction to compare first with Householder geometry. Hyperbolic Coxeter groups require a different ambient metric situation and are not needed to understand Householder QR.

## Numerical-linear-algebra caution

The fact that every Householder matrix is a reflection does not imply that the reflectors appearing in a QR factorization form a useful finite Coxeter group. Coxeter simplifications require extra information about the relative normals/root system.

That distinction is worth preserving:

`generic matrix → orthogonal map → reflection → Coxeter generator → Weyl/Dynkin data`

Each arrow requires additional certified structure.

## Sources

- Lloyd N. Trefethen and David Bau III, **Numerical Linear Algebra**. Philadelphia: SIAM, 1997. Lecture 10, “Householder Triangularization.” ISBN 978-0-89871-361-9.
- John C. Baez, **This Week's Finds in Mathematical Physics, Week 62** — finite reflection groups and Coxeter diagrams: https://math.ucr.edu/home/baez/week62.html
- John C. Baez, **Week 63** — crystallographic condition and root lengths: https://math.ucr.edu/home/baez/week63.html
- John C. Baez, **Week 64** — Dynkin diagrams and simple Lie algebras: https://math.ucr.edu/home/baez/week64.html
- John C. Baez, **Week 65** — ADE lattices and McKay correspondence: https://math.ucr.edu/home/baez/week65.html
- John C. Baez, **Week 182** — spherical / Euclidean / hyperbolic boundary in ADE-related classification: https://math.ucr.edu/home/baez/week182.html

## Provenance

This note is a mathematics-focused cross-post from `isomorphismes/coxeter` branch `research/reflections-dynkin`, trimmed of compiler-specific material and linked back to the Trefethen–Bau notes already in this repository.

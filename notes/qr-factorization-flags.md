# QR factorization, flags, and representation theory

## Trefethen–Bau Lecture 7 is literally a flag construction

In Lecture 7 of Lloyd N. Trefethen and David Bau III, *Numerical Linear Algebra*, let the columns of a full-column-rank matrix

`A = [a₁ a₂ ... aₙ]`

be given. Define the successive column spaces

`Vⱼ = span(a₁, ..., aⱼ)`.

Then

`V₁ ⊂ V₂ ⊂ ... ⊂ Vₙ`.

Because the columns are independent, `dim Vⱼ = j`. This is a flag of type `(1,2,...,n)` in the ambient space. If the ambient space is restricted to `col(A) = Vₙ`, it is a complete flag there.

QR constructs orthonormal vectors `q₁,...,qₙ` satisfying

`span(q₁,...,qⱼ) = span(a₁,...,aⱼ) = Vⱼ`

for every `j`. Thus QR can be read geometrically as constructing an orthonormal frame adapted to the flag determined by the ordered columns of `A`.

## Why `R` is upper triangular

The flag condition itself explains the triangular shape. Since

`aⱼ ∈ Vⱼ = span(q₁,...,qⱼ)`,

the coordinates of `aⱼ` along `qⱼ₊₁, qⱼ₊₂, ...` vanish. Therefore in

`A = QR`

all entries of `R` below the diagonal are zero.

This is more than a mnemonic: triangularity is the coordinate expression of respecting the successive subspaces in the flag.

## Square case: the direct bridge to flag varieties

For `A ∈ GL(n, ℂ)`, its ordered columns determine a complete flag in `ℂⁿ`. The full flag variety can be written

`GL(n, ℂ) / B`,

where `B` is the subgroup of invertible upper-triangular matrices. Right multiplication by an element of `B` changes the ordered basis while preserving every successive span, so all matrices in the same right coset determine the same flag.

QR writes

`A = QR`

with `Q` unitary and `R` upper triangular. Since `R ∈ B`, `A` and `Q` determine the same point of `GL(n, ℂ)/B`. In this sense QR chooses a unitary/orthonormal representative of the flag determined by `A` (up to the usual diagonal phase/sign convention).

Equivalently, endomorphisms preserving a fixed complete flag are represented by upper-triangular matrices in a basis adapted to that flag. This is the Borel subgroup/Borel subalgebra side of the same geometry.

For rectangular full-column-rank `A ∈ ℂ^{m×n}`, the same successive-span idea gives a partial/full flag inside the `n`-dimensional column space, while `Q` is an orthonormal `n`-frame in `ℂᵐ`.

## Do not collapse three different uses of “flag”

### 1. Flags / flag varieties / flag manifolds

This is the notion relevant to QR and representation theory: nested subspaces, together with spaces parameterizing such nested subspaces. Borel and parabolic subgroups enter because they are stabilizers of full and partial flags.

### 2. Razborov flag algebras

These belong to extremal combinatorics. They are not what is happening in Trefethen–Bau Lecture 7.

### 3. Ghrist’s flag complexes

Robert Ghrist’s *Elementary Applied Topology* has §2.4, **“Flag complexes and networks.”** Here “flag complex” means the simplicial/clique-complex construction associated with a graph. This is again a different use of the word “flag”; EAT does not appear to be introducing the representation-theoretic notion of a flag algebra there.

## The MIT representation-theory note the earlier recollection was pointing to

The remembered MIT link can be identified much more precisely as David Vogan’s course notes **“Geometry of Flag Manifolds and Representation Theory”** for MIT 18.758.

Vogan explicitly contrasts algebraic geometers with representation theorists. The remembered sides were reversed: he says that, for algebraic geometers, projective space is a large workshop in which one can **“fashion beautiful little pieces of art”**, whereas for representation theorists projective space is, roughly, almost the only algebraic variety needed.

He then passes from projective space to partial flag varieties, interprets them as homogeneous spaces for `GL(n)` modulo parabolic subgroups, extends the picture to reductive groups, and connects the geometry to representation theory.

This is the conceptual bridge that makes the Trefethen–Bau page look familiar: the numerical-linear-algebra chain of successive column spaces is exactly the elementary linear-algebra object that becomes a flag manifold/flag variety when one studies the space of all such chains.

## Sources

- Lloyd N. Trefethen and David Bau III, *Numerical Linear Algebra*, SIAM, 1997, Lecture 7 (“QR Factorization”), especially the successive-span condition leading to `A = QR`.
- David A. Vogan Jr., “Geometry of Flag Manifolds and Representation Theory,” MIT 18.758 course notes. https://math.mit.edu/~dav/flags.pdf
- MIT 18.758 course page identifying Vogan and the flag-variety notes: https://math.mit.edu/~dav/758.06.html
- Robert Ghrist, *Elementary Applied Topology*, 2014, Chapter 2, §2.4 “Flag complexes and networks.” Contents: https://www2.math.upenn.edu/~ghrist/EAT/EATcontents.pdf

## Cross-links to pursue

- Schubert cells and Bruhat order: the finer decomposition of flag varieties that appears throughout representation theory and algebraic geometry.
- Iwasawa/QR viewpoints: `GL(n, ℂ)` decompositions into unitary and triangular factors, and how these choices relate to homogeneous spaces.
- Numerical algorithms that preserve or update nested subspaces explicitly, including Hessenberg/Arnoldi/Krylov constructions.

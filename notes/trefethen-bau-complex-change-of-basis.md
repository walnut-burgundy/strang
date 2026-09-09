# Complex change of basis and realification

These notes record a point that can look odd when reading numerical linear algebra: a matrix whose entries are real may be studied using a complex change of basis. There is no contradiction. The real matrix is being regarded as a complex-linear map, and the complex basis may expose structure that no real eigenbasis can expose.

## Real matrices inside complex linear algebra

A real matrix

\[
A \in \mathbb R^{n\times n}
\]

can also be regarded as a complex matrix

\[
A \in \mathbb C^{n\times n}
\]

whose imaginary part happens to be zero. Once the ambient scalar field is \(\mathbb C\), basis vectors and change-of-basis matrices are allowed to have complex entries.

This is why a treatment may begin with matrices whose displayed entries are real but later use unitary matrices, conjugate transpose \((\cdot)^*\), and complex basis vectors. The notation is uniform over \(\mathbb C\). In the purely real case, unitary becomes orthogonal and conjugate transpose becomes ordinary transpose.

## Why a real matrix may need a complex basis

The standard example is a planar rotation

\[
R_\theta =
\begin{pmatrix}
\cos\theta & -\sin\theta\\
\sin\theta & \cos\theta
\end{pmatrix}.
\]

Unless \(\theta=0\) or \(\pi\), this matrix has no real eigenvectors. Its eigenvalues are

\[
e^{i\theta}, \qquad e^{-i\theta},
\]

with complex eigenvectors. Thus a diagonalizing change of basis is necessarily complex even though every entry of \(R_\theta\) is real.

Over \(\mathbb R\), the same structure remains as a \(2\times2\) rotation block. Over \(\mathbb C\), that block splits into two scalar eigenvalues. Complex numbers do not change the operator; they give a scalar field in which more of its structure can be separated.

This is the same general reason that complex Schur form is triangular while real Schur form must retain \(2\times2\) blocks for conjugate pairs of nonreal eigenvalues.

## Every complex matrix has a doubled real representation

Write

\[
A = B+iC,
\]

where

\[
B,C\in\mathbb R^{m\times n}.
\]

Identify a complex vector

\[
z=x+iy \in \mathbb C^n
\]

with the real vector

\[
\begin{pmatrix}x\\y\end{pmatrix}\in\mathbb R^{2n}.
\]

Then the complex linear map \(A:\mathbb C^n\to\mathbb C^m\) is represented as the real linear map

\[
\Phi(A)
=
\begin{pmatrix}
B & -C\\
C & B
\end{pmatrix}
:
\mathbb R^{2n}\to\mathbb R^{2m}.
\]

Indeed,

\[
(B+iC)(x+iy)
=
(Bx-Cy)+i(Cx+By),
\]

so

\[
\begin{pmatrix}
\operatorname{Re}(Az)\\
\operatorname{Im}(Az)
\end{pmatrix}
=
\begin{pmatrix}
B & -C\\
C & B
\end{pmatrix}
\begin{pmatrix}x\\y\end{pmatrix}.
\]

For a square \(n\times n\) complex matrix, this gives a real \(2n\times2n\) matrix.

## Algebra is preserved

The map \(\Phi\) preserves matrix addition and multiplication:

\[
\Phi(A+D)=\Phi(A)+\Phi(D),
\]

and

\[
\Phi(AD)=\Phi(A)\Phi(D).
\]

Consequently, for square matrices,

\[
A \text{ invertible}
\iff
\Phi(A) \text{ invertible},
\]

and

\[
\Phi(A^{-1})=\Phi(A)^{-1}.
\]

The determinant satisfies

\[
\det \Phi(A)=|\det A|^2.
\]

The eigenvalues of \(\Phi(A)\), when the real matrix is itself considered over \(\mathbb C\), consist of the eigenvalues of \(A\) together with their complex conjugates, with multiplicity.

## Complex change of basis becomes real change of basis in doubled dimension

Suppose

\[
A' = P^{-1}AP,
\qquad P\in GL_n(\mathbb C).
\]

Then realification gives

\[
\Phi(A')
=
\Phi(P)^{-1}\Phi(A)\Phi(P).
\]

Thus a complex change of basis in \(\mathbb C^n\) is exactly a real change of basis after passing to \(\mathbb R^{2n}\), provided the real change of basis respects the extra structure that represents multiplication by \(i\).

If the original matrix \(A\) is real, then

\[
\Phi(A)
=
\begin{pmatrix}
A&0\\
0&A
\end{pmatrix}.
\]

A complex basis can therefore be understood without treating complex numbers as mysterious new objects: it is a structured real basis change on two copies of the original real space.

## Which real matrices come from complex matrices?

Not every real \(2n\times2n\) matrix is the realification of a complex \(n\times n\) matrix.

Multiplication by \(i\) corresponds, in doubled real coordinates, to

\[
J=
\begin{pmatrix}
0&-I\\
I&0
\end{pmatrix}.
\]

A real matrix \(M\in\mathbb R^{2n\times2n}\) represents a complex-linear map exactly when

\[
MJ=JM.
\]

Equivalently, such matrices have the block form

\[
M=
\begin{pmatrix}
B&-C\\
C&B
\end{pmatrix}.
\]

The condition \(MJ=JM\) is the real-coordinate statement that the map respects scalar multiplication by \(i\).

## Practical interpretation

There are therefore two equivalent viewpoints:

1. work in \(\mathbb C^n\), where conjugate eigenpairs may become separate scalar directions and formulas are often shorter; or
2. work in \(\mathbb R^{2n}\), carrying an additional operator \(J\) that records the complex structure.

The first is usually preferable in numerical linear algebra because it avoids doubling every vector and matrix and because diagonalization, Schur theory, Fourier modes, and oscillatory phenomena are naturally expressed with complex scalars.

For real input data, many decompositions can still be chosen entirely real. QR and SVD of a real matrix can use real orthogonal factors. The place where complex bases become essential is when one asks for structure, such as an eigenbasis, that may not exist over \(\mathbb R\).

## Source

This note was prompted by the treatment of real and complex vector spaces, unitary changes of basis, and spectral decompositions in:

Lloyd N. Trefethen and David Bau III, *Numerical Linear Algebra*, Society for Industrial and Applied Mathematics (SIAM), 1997. ISBN 978-0-89871-361-9.

The equations and explanatory development above are working notes rather than page-by-page quotations from the book. Exact page citations should be added when the corresponding passages are checked directly against the text.

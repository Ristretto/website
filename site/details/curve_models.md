# Curve Models

Decaf and Ristretto make use of three curve shapes: Jacobi quartic,
twisted Edwards, and Montgomery, and isogenies between them.

## The Jacobi Quartic

The Jacobi quartic curve is parameterized by \\(e, A\\), and is of the
form $$ \mathcal J\_{e,A} : t\^2 = es\^4 + 2As\^2 + 1, $$ with
identity point \\((0,1)\\).  For more details on the Jacobi quartic,
see the [_Decaf_ paper][decaf_paper] or
[_Jacobi Quartic Curves Revisited_][hwcd_jacobi] by Hisil, Wong,
Carter, and Dawson).

When \\(e = a\^2\\) is a square, \\(\mathcal J\_{e,A}\\) has full
\\(2\\)-torsion (i.e., \\(\mathcal J[2] \cong \mathbb Z /2 \times
\mathbb Z/2\\)), and
we can write the \\(\mathcal J[2]\\)-coset of a point \\(P =
(s,t)\\) as
$$
P + \mathcal J[2] = \left\\{
(s,t),
(-s,-t),
(1/as, -t/as\^2),
(-1/as, t/as\^2)
\right\\}.
$$
Notice that replacing \\(a\\) by \\(-a\\) just swaps the last two
points, so this set does not depend on the choice of \\(a\\).  

## Twisted Edwards Curves

Twisted Edwards curves are parameterized by \\(a, d\\) and are of the form
$$
\mathcal E\_{a,d} : ax\^2 + y\^2 = 1 + dx\^2y\^2.
$$
These are usually represented by the [_Extended Twisted Edwards
Coordinates_][hwcd_edwards] of Hisil, Wong, Carter, and Dawson: points are
represented in projective coordinates as  \\((X:Y:Z:T)\\) with
$$
XY = ZT, \quad aX\^2 + Y\^2 = Z\^2 + dT\^2.
$$
(More details on Edwards curve models can be found in the
[`curve25519_dalek` `curve_models`][curve_models] documentation).  The
case \\(a = 1\\) is the _untwisted_ case; the case \\(a = -1\\)
provides the fastest formulas.  When not otherwise specified, we write
\\(\mathcal E\\) for \\(\mathcal E\_{a,d}\\).

When both \\(d\\) and \\(ad\\) are nonsquare (which forces \\(a\\) to be
square), the curve is *complete*.  In this case the four-torsion subgroup is
cyclic, and we can write it explicitly as
$$
\mathcal E\_{a,d}[4] = \\{ (0,1),\\; (1/\sqrt a, 0),\\; (0, -1),\\; (-1/\sqrt{a}, 0)\\}.
$$
These are the only points with \\(xy = 0\\); the points with
\\( y \neq 0 \\) are \\(2\\)-torsion.

## Montgomery Curves

Montgomery curves are parameterized by \\(B, A\\) with \\(B \neq 0\\)
and \\(A^2 \neq 4 \\) and are of the form
\\[
\mathcal M_{B,A} : Bv^2 = u(u^2 + Au + 1),
\\]
with the identity point at infinity.  More details can be found in the
[_Decaf_ paper][decaf_paper] or in [_Montgomery curves and their
arithmetic_][cs_monty] by Costello and Smith.


[hwcd_edwards]: https://eprint.iacr.org/2008/522.pdf
[curve_models]: https://doc-internal.dalek.rs/curve25519_dalek/curve_models/index.html
[decaf_paper]: https://eprint.iacr.org/2015/673.pdf
[hwcd_jacobi]: https://eprint.iacr.org/2009/312.pdf
[cs_monty]: https://arxiv.org/pdf/1703.01863v1.pdf
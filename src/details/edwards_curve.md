# The Edwards Curve

The primary internal model in `curve25519-dalek` for Curve25519 points
is the [_Extended Twisted Edwards Coordinates_][hwcd_edwards] of
Hisil, Wong, Carter, and Dawson.  These correspond to the affine model
$$
\mathcal E\_{a,d} : ax\^2 + y\^2 = 1 + dx\^2y\^2.
$$
In projective coordinates, we represent a point as \\((X:Y:Z:T)\\)
with
$$
XY = ZT, \quad aX\^2 + Y\^2 = Z\^2 + dT\^2.
$$
(For more details on this model, see the
[`curve_models`][curve_models] documentation). The case \\(a = 1\\) is
the _untwisted_ case; we only consider \\(a = \pm 1\\), and in
particular we focus on the twisted Edwards form of Curve25519, which
has \\(a = -1, d = -121665/121666\\).  When not otherwise specified,
we write \\(\mathcal E\\) for \\(\mathcal E\_{a,d}\\).

When both \\(d\\) and \\(ad\\) are nonsquare (which forces \\(a\\)
to be square), the curve is *complete*.  In this case the
four-torsion subgroup is cyclic, and we
can write it explicitly as
$$
\mathcal E\_{a,d}[4] = \\{ (0,1),\\; (1/\sqrt a, 0),\\; (0, -1),\\; (-1/\sqrt{a}, 0)\\}.
$$
These are the only points with \\(xy = 0\\); the points with
\\( y \neq 0 \\) are \\(2\\)-torsion.

[hwcd_edwards]: https://eprint.iacr.org/2008/522.pdf
[curve_models]: https://doc-internal.dalek.rs/curve25519_dalek/curve_models/index.html

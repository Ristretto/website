# Torquing Points

To bridge the gap between the cofactor \\(4\\) and cofactor \\(8\\)
cases, we need a way to canonically select a representative modulo
\\(\mathcal E[2] \\), given a representative modulo \\(\mathcal E[4] \\).

Using the description of \\(\mathcal E[4]\\) above, we can write the
\\(\mathcal E[4]\\)-coset of a point \\(P = (x,y)\\) as
$$
P + \mathcal E\_{a,d}[4] = \\{ (x,y),\\; (y/\sqrt a, -x\sqrt a),\\; (-x, -y),\\; (-y/\sqrt a, x\sqrt a)\\}.
$$
Notice that if \\(xy \neq 0 \\), then exactly two of these points have
\\( xy \\) non-negative, and they differ by the \\(2\\)-torsion point
\\( (0,-1) \\).

This means that we can select a representative modulo
\\(\mathcal E[2]\\) by requiring \\(xy\\) nonnegative and \\(y \neq
0\\), and we can ensure that this condition holds by conditionally
adding a \\(4\\)-torsion point \\(Q_4\\) if \\(xy\\) is negative or
\\(y = 0\\).

The points of exact order \\(4\\) are \\( (\pm 1/\sqrt{a}, 0 )\\);
convenient choices for \\( Q_4 \\) are \\((1,0)\\) when \\( a = 1 \\)
and \\( (i, 0) \\) when \\( a = -1 \\), although the choice of which
\\(4\\)-torsion point to use doesn't matter.

This procedure gives a canonical lift from \\(\mathcal E / \mathcal
E[4]\\) to \\(\mathcal E / \mathcal E[2]\\).  Since it involves a
conditional rotation, we refer to it as *torquing* the point.


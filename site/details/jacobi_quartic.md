# The Jacobi Quartic

The Jacobi quartic curve is parameterized by \\(e, A\\), and is of the
form $$ \mathcal J\_{e,A} : t\^2 = es\^4 + 2As\^2 + 1, $$ with
identity point \\((0,1)\\).  For more details on the Jacobi quartic,
see the [Decaf paper][decaf_paper] or
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

Ristretto requires \\(a = \pm 1\\).

## Encoding \\(\mathcal J / \mathcal J[2]\\)

To encode points on \\(\mathcal J\\) modulo \\(\mathcal J[2]\\),
we need to choose a canonical representative of the above coset.
To do this, it's sufficient to make two independent sign choices:
the Decaf paper suggests choosing \\((s,t)\\) with \\(s\\)
non-negative and finite, and \\(t/s\\) non-negative or infinite.

The encoding is then the (canonical byte encoding of the)
\\(s\\)-value of the canonical representative.

[decaf_paper]: https://eprint.iacr.org/2015/673.pdf
[hwcd_jacobi]: https://eprint.iacr.org/2009/312.pdf

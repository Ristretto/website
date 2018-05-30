# The Ristretto Group

We consider two cases:

* cofactor \\(4\\), where \\( \\# \mathcal E(\mathbb F_p) = 4\cdot \ell \\);
* cofactor \\(8\\) with cyclic \\(8\\)-torsion,
  where \\( \\# \mathcal E(\mathbb F_p) = 8 \cdot \ell \\)
  and \\( \mathcal E[8] \cong \mathbb Z / 8 \\).

In the cofactor \\(4\\) case, we have \\( \[2\](\mathcal E[4]) =
\mathcal E[2] \\), so that \\( \mathcal E[2] \subseteq \[2\](\mathcal
E) \\), and the group we will construct is
$$
\frac{\[2\](\mathcal E)}{\mathcal E[2]}
$$
which has prime order \\( (4\ell/2)/2 = \ell \\).

In the cofactor \\(8\\) case, since the \\(8\\)-torsion is cyclic, we
have \\( \[2\](\mathcal E[8]) = \mathcal E[4] \\), so that \\(\mathcal
E[4] \subseteq \[2\](\mathcal E)\\), and the group we will construct
is
$$
\frac{\[2\](\mathcal E)}{\mathcal E[4]}
$$
which has prime order \\( (8\ell/2)/4 = \ell \\).

In particular, Curve25519 has \\( \mathcal E(\mathbb
F\_p) \cong \mathbb Z / 8 \times \mathbb Z / \ell\\), where \\( \ell
= 2\^{252} + \cdots \\) is a large prime, so it meets the requirements
for the cofactor \\(8\\) case.

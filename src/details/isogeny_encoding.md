# Encoding with the Isogeny

The Decaf paper recalls that, for a group \\( G \\) with normal
subgroup \\(G' \leq G\\), a group homomorphism \\( \phi : G
\rightarrow H \\) induces a homomorphism
$$
\bar{\phi} : \frac G {G'} \longrightarrow \frac {\phi(G)}{\phi(G')} \leq \frac {H} {\phi(G')},
$$
and that the induced homomorphism \\(\bar{\phi}\\) is injective if
\\( \ker \phi \leq G' \\).  In our context, the kernel of
\\(\theta\\) is \\( \\{(0, \pm 1)\\} \leq \mathcal J[2] \\),
so \\(\theta\\) gives an isomorphism
$$
\frac {\mathcal J} {\mathcal J[2]}
\cong
\frac {\theta(\mathcal J)} {\theta(\mathcal J[2])}
\cong
\frac {\[2\](\mathcal E)} {\mathcal E[2]}.
$$

We can use the isomorphism to transfer the encoding of \\(\mathcal
J / \mathcal J[2] \\) defined above to \\(\[2\](\mathcal E)/\mathcal
E[2]\\), by encoding the Edwards point \\((x,y)\\) using the Jacobi
quartic encoding of \\(\theta\^{-1}(x,y)\\).

Since \\(\\# (\[2\](\mathcal E) / \mathcal E[2]) = (\\#\mathcal
E)/4\\), if \\(\mathcal E\\) has cofactor \\(4\\), we're done.
Otherwise, if \\(\mathcal E\\) has cofactor \\(8\\), as in the
Curve25519 case, we use the torquing procedure to lift \\(\mathcal E
/ \mathcal E[4]\\) to \\(\mathcal E / \mathcal E[2]\\), and then
apply the encoding for \\( \[2\](\mathcal E) / \mathcal E[2] \\).

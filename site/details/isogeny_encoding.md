# Encoding with Isogenies

The Decaf strategy is to define an encoding of
\\(\mathcal J / \mathcal J[2]\\),
the Jacobi quartic modulo its \\(2\\)-torsion, and then to transport
the encoding to an Edwards or Montgomery curve used to implement the
group operations.  Ristretto uses the same strategy,
but makes different choices for encoding 
\\(\mathcal J / \mathcal J[2]\\)
and explicitly supports the cofactor-\\(8\\) case.

## Encoding \\(\mathcal J / \mathcal J[2]\\)

As noted in the [Curve Models](./details/curve_models.html) section,
the \\(\mathcal J[2]\\)-coset of a point \\(P =
(s,t)\\) on \\(\mathcal J_{a^2, A}\\) is
\\[
P + \mathcal J[2] = \left\\{
(s,t),
(-s,-t),
(1/as, -t/as\^2),
(-1/as, t/as\^2)
\right\\}.
\\]

To encode points on \\(\mathcal J\\) modulo \\(\mathcal J[2]\\),
we need to choose a canonical representative of the above coset.
The encoding is then the (canonical byte encoding of the)
\\(s\\)-value of the canonical representative.

To do this, it's sufficient to make two independent sign choices;
Decaf chooses the \\((s,t)\\) with \\(s\\)
non-negative and finite, and \\(t/s\\) non-negative or infinite.
Ristretto makes different sign choices, discussed later.

## Transporting an encoding along an isogeny

The Decaf paper recalls that, for a group \\( G \\) with normal
subgroup \\(G' \leq G\\), a group homomorphism \\( \phi : G
\rightarrow H \\) induces a homomorphism
$$
\bar{\phi} : \frac G {G'} \longrightarrow \frac {\phi(G)}{\phi(G')} \leq \frac {H} {\phi(G')},
$$
and that the induced homomorphism \\(\bar{\phi}\\) is injective if
\\( \ker \phi \leq G' \\).

Both \\(\phi : \mathcal J \rightarrow \mathcal E_1 \\) and \\(\theta :
\mathcal J \rightarrow \mathcal E_2 \\) have kernels contained in
\\(\mathcal J[2]\\), and give isomorphisms
$$
\frac {\mathcal J} {\mathcal J[2]}
\cong
\frac {\phi(\mathcal J)} {\phi(\mathcal J[2])}
\cong
\frac {\[2\](\mathcal E_1)} {\mathcal E_1[2]},
\qquad
\frac {\mathcal J} {\mathcal J[2]}
\cong
\frac {\theta(\mathcal J)} {\theta(\mathcal J[2])}
\cong
\frac {\[2\](\mathcal E_2)} {\mathcal E_2[2]}.
$$

We can use these isomorphisms to transfer an encoding of \\(\mathcal
J / \mathcal J[2] \\) to 
\\(\[2\](\mathcal E)/\mathcal E[2]\\) for either choice of \\(\mathcal E = \mathcal E_1, \mathcal E_2\\).

In the cofactor \\(4\\) case, where
\\( \\# \mathcal E(\mathbb F_p) = 4\cdot \ell \\),
\\(\[2\](\mathcal E)/\mathcal E[2] \\)
has prime order \\( (4\ell/2)/2 = \ell \\) and we're done.

In the cofactor \\(8\\) case with cyclic \\(8\\)-torsion, we
have \\( \[2\](\mathcal E[8]) = \mathcal E[4] \\), so that \\(\mathcal
E[4] \subseteq \[2\](\mathcal E)\\).
The group
\\(\[2\](\mathcal E)/\mathcal E[4] \\)
has prime order \\( (8\ell/2)/4 = \ell \\), and to encode it
we 
use the torquing procedure described below to canonically lift
\\(\mathcal E / \mathcal E[4]\\) to \\(\mathcal E / \mathcal E[2] \\), and then
apply the encoding above.

## Torquing points to lift \\(\mathcal E / \mathcal E[4]\\) to \\(\mathcal E / \mathcal E[2] \\)

To bridge the gap between the cofactor \\(4\\) and cofactor \\(8\\)
cases, we need a way to canonically select a representative modulo
\\(\mathcal E[2] \\), given a representative modulo \\(\mathcal E[4] \\).

Using the description of \\(\mathcal E[4]\\)
in the [Curve Models](./details/curve_models.html) section,
we can write the
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

## Ristretto and Decaf

Decaf is oriented around use of the curve \\(\mathcal E_1 = \mathcal E_{a_1,d_1}\\), while Ristretto is oriented around use of the curve \\(\mathcal E_2 = \mathcal E_{a_2, d_2}\\).
The correspondence between the Ristretto parameters \\(a_2 , d_2\\)
and the Decaf parameters \\(a_1, d_1\\) is given by
\\[
\begin{aligned}
a_2 &= -a_1   & a_1 &= -a_2 \\\\
d_2 &= \frac {a_1 d_1} {a_1 - d_1} & d_1 &= \frac {a_2 d_2 }{a_2 - d_2} \\\\
\end{aligned}
\\]
When using the Edwards form of Curve25519 to implement the group, as
in `ristretto255`, this approach has the advantage that when \\(a_2,
d_2\\) are the Ed25519 parameters \\(-1, -121665/121666\\), \\(a_1,
d_1\\) are \\(1, -121666\\).  This allows a compatible implementation
to implement group operations on the curve \\(\mathcal E_1\\) or its
\\(-1\\) twist \\(\mathcal E_{-1, 121666}\\), which has smaller
constants and therefore slightly better performance.
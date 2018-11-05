# Encoding in Affine Coordinates

We can write the Ristretto encoding/decoding procedure in affine
coordinates, before describing optimized formulas to and from
projective coordinates.

## Encoding

On input \\( (x,y) \in \[2\](\mathcal E)\\), a representative for a
coset in \\( \[2\](\mathcal E) / \mathcal E[4] \\):

1. Check if \\( xy \\) is negative or \\( x = 0 \\); if so, torque
   the point by setting \\( (x,y) \gets (x,y) + Q_4 \\), where
   \\(Q_4\\) is a \\(4\\)-torsion point.

2. Check if \\(x\\) is negative or \\( y = -1 \\); if so, set
   \\( (x,y) \gets (x,y) + (0,-1) = (-x, -y) \\).

3. Compute $$ s = +\sqrt {(-a) \frac {1 - y} {1 + y} }, $$ choosing
   the positive square root.

The output is then the (canonical) byte-encoding of \\(s\\).

If \\(\mathcal E\\) has cofactor \\(4\\), we skip the first step,
since our input already represents a coset in
\\( \[2\](\mathcal E) / \mathcal E[2] \\).

## Interpreting the Encoding Procedure

How does this procedure correspond to the description involving
\\( \theta \\)?

The first step lifts from \\( \mathcal E / \mathcal E[4] \\) to
\\(\mathcal E / \mathcal E[2]\\).  To understand steps 2 and 3,
notice that the \\(y\\)-coordinate of \\(\theta(s,t)\\) is
$$
y = \frac {1 + as\^2}{1 - as\^2},
$$
so that the \\(s\\)-coordinate of \\(\theta\^{-1}(x,y)\\) has
$$
s\^2 = (-a)\frac {1-y}{1+y}.
$$
Since
$$
x = \frac 1 {\sqrt {ad - 1}} \frac {2s} {t},
$$
we also have
$$
\frac s t = x \frac {\sqrt {ad-1}} 2,
$$
so that the sign of \\(s/t\\) is determined by the sign of \\(x\\).

Recall that to choose a canonical representative of \\( (s,t) + \mathcal J[2]
\\), it's sufficient to make two sign choices: the sign of \\(s\\) and the sign
of \\(s/t\\).  Step 2 determines the sign of \\(s/t\\), while step 3 computes
\\(s\\) and determines its sign (by choosing the positive square root).
Finally, the check that \\(y \neq -1\\) prevents division-by-zero when encoding
the identity.  If the inverse square root function returns \\(0\\), this check
falls out of the optimized formulas for projective coordinates.

## Decoding

On input `s_bytes`, decoding proceeds as follows:

1. Decode `s_bytes` to \\(s\\); reject if `s_bytes` is not the
   canonical encoding of \\(s\\).

2. Check whether \\(s\\) is negative; if so, reject.

3. Compute
$$
y \gets \frac {1 + as\^2}{1 - as\^2}.
$$

4. Compute
$$
x \gets +\sqrt{ \frac{4s\^2} {ad(1+as\^2)\^2 - (1-as\^2)\^2}},
$$
choosing the positive square root, or reject if the square root does
not exist.

5. Check whether \\(xy\\) is negative or \\(y = 0\\); if so, reject.


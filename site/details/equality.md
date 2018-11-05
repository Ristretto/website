# Equality Testing

Testing equality of two Ristretto points means testing whether they
are equal in the quotient group, i.e., whether they lie in the same
coset of \\(\mathcal E[4] \\) (for the cofactor-\\(8\\) case) or
\\(\mathcal E[2] \\) (for the cofactor-\\(4\\) case).

Equality testing of points on the Edwards curve requires comparing to
affine coordinates, which requires an expensive inversion.  However,
testing whether two points lie in the same coset can be done in
projective coordinates, making it actually *easier* than equality
testing in the original non-quotient group.

The Decaf paper proves that in the cofactor-\\(4\\) case, to test equality of
\\(P_1 = (X_1:Y_1:Z_1:T_1)\\)
and 
\\(P_2 = (X_2:Y_2:Z_2:T_2)\\)
it's sufficient to test whether
\\[
X_1 Y_2 = Y_1 X_2.
\\]

In the cofactor-\\(8\\) case, we need to test equality not modulo
\\(\mathcal E[2]\\) but modulo \\(\mathcal E[4]\\).  This means we
need to check whether either \\( P_1 \equiv P_2 \pmod {\mathcal E[2]}
\\) or \\( P_1 + Q_4 \equiv P_2 \pmod {\mathcal E[2]} \\), where
\\(Q_4\\) is a \\(4\\)-torsion point.

Write \\(P_1 + Q_4 = (X_1' : Y_1' : Z_1' : T_1')\\).
In affine coordinates, \\(P + Q_4 = (y/\sqrt a, -x\sqrt a)\\),
so that \\(X_1' = Y_1 / \sqrt a\\), \\(Y_1' = -X_1 \sqrt a\\).
The equation
\\[ X_1' Y_2 = Y_1' X_2 \\]
then becomes
\\[
Y_1 Y_2 / \sqrt a = - \sqrt a X_1 X_2
\\]
or equivalently
\\[
Y_1 Y_2 = - a X_1 X_2.
\\]

So, to check equality modulo \\(\mathcal E[4]\\), it's sufficient to check whether
\\[
X_1 Y_2 = Y_1 X_2  \quad \mathrm{or} \quad Y_1 Y_2 = -a X_1 X_2.
\\]


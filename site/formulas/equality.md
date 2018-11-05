# Testing Equality

To test equality of two Edwards points
\\(P_1 = (X_1:Y_1:Z_1:T_1)\\)
and 
\\(P_2 = (X_2:Y_2:Z_2:T_2)\\)
which represent Ristretto points, it's sufficient to check whether
\\[
X_1 Y_2 = Y_1 X_2  \quad \mathrm{or} \quad Y_1 Y_2 = -a X_1 X_2.
\\]

Note that when \\(a = -1\\), this simplifies to
\\[
X_1 Y_2 = Y_1 X_2  \quad \mathrm{or} \quad Y_1 Y_2 = X_1 X_2.
\\]

Unlike testing equality on the Edwards curve, this does not require an inversion and can be done in projective coordinates.


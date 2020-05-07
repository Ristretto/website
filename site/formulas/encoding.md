# Encoding from Extended Coordinates

To encode a Ristretto point represented by the point \\((X:Y:Z:T)\\)
in extended coordinates, an implementation proceeds as follows.

1. \\(u\_1 \gets (Z\_0 + Y\_0)(Z\_0 - Y\_0) \\).
2. \\(u\_2 \gets X\_0 Y\_0 \\).
3. \\(I \gets \mathrm{invsqrt}(u\_1 u\_2\^2) \\).
4. \\(D\_1 \gets u\_1 I \\).
5. \\(D\_2 \gets u\_2 I \\).
6. \\(Z\_{inv} \gets D\_1 D\_2 T\_0 \\).
7. If \\( T\_0 Z\_{inv} \\) is negative:
    1. \\( (X, Y) \gets (Y\_0 (\pm 1/\sqrt{a}), X\_0 (\mp \sqrt{a})) \\).
    2. \\( D \gets D\_1 / \sqrt{a-d} \\).
8. Otherwise:
    1. \\( (X, Y) \gets (X\_0, Y\_0) \\).
    2. \\( D \gets D\_2 \\).
9. If \\( X Z\_{inv} \\) is negative, set \\( Y \gets - Y\\).
10. Compute \\( s \gets |\sqrt{-a} (Z - Y) D| \\), i.e., compute
    \\(\sqrt{-a} (Z - Y) D\\) and negate it if it is negative.
11. Return the canonical byte encoding of \\( s \\).

Since \\( a = \pm 1 \\), implementations can replace multiplications
by \\(a\\) with sign changes, as appropriate.  

When \\(a = -1\\), (10) becomes \\(s \gets |(Z-Y)D|\\), and choosing
\\( Q\_4 = (i, 0) \\)  is convenient since it simplifies (7.1)
to \\( (X,Y) \gets (iY_0, iX_0) \\).  

The inverse square root in step
3 always exists when \\((X:Y:Z:T)\\) is a valid representative.

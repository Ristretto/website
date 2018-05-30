# Encoding in Extended Coordinates

The formulas on the previous page are given in affine coordinates, but the
usual internal representation is extended twisted Edwards coordinates \\(
(X:Y:Z:T) \\) with \\( x = X/Z \\), \\(y = Y/Z\\), \\(xy = T/Z \\).

This presents a complication for the cofactor-\\(8\\) case: selecting the
distinguished representative of the coset requires the affine coordinates \\(
(x,y) \\), and computing \\( s \\) requires an inverse square root.  As
inversions are expensive, we'd like to be able to do this whole computation
with only one inverse square root, by batching together the inversion and the
inverse square root.

It is not obvious how to do this, since we need the inverse square
root of one of two values, depending on what the distinguished
representative is, but the choice of representative depends on the
affine coordinates.  However, an ingenious trick (due to Mike Hamburg)
allows recovering either of the inverse square roots we want.

## Batching the Inversion and Inverse Square Root

Write \\( (X\_0 : Y\_0 : Z\_0 : T\_0) \\)
for the coordinates of the initial representative, and write 
\\( (X:Y:Z:T) \\) for the coordinates of the distinguished
representative of the coset.

Since \\(y = Y/Z\\), in extended coordinates the formula for \\(s\\) becomes
$$
s
= \sqrt{ (-a) \frac{ 1 - Y/Z}{1+Y/Z}} = \sqrt{\frac{Z - Y}{Z+Y}} \sqrt{-a}
= \frac {Z - Y} {\sqrt{Z\^2 - Y\^2}} \sqrt{-a},
$$
so we need to compute \\( 1 / \sqrt{Z^2 - Y^2} \\).

The distinguished representative \\( (X:Y:Z:T) \\) is selected by the
torquing procedure in step 1, which conditionally adds a
\\(4\\)-torsion point \\(Q_4\\).  As noted in the torquing section
above, \\( Q_4 = (\pm 1/\sqrt{a}, 0) \\), so we obtain
$$
(X : Y : Z : T ) = 
\begin{cases}
(X\_0 : Y\_0 : Z\_0 : T\_0) \\\\
(\pm Y\_0 / \sqrt{a} : \mp X\_0 \sqrt{a} : Z\_0 : -T\_0) 
\end{cases}
.
$$
This means we want to compute either of
$$
\frac {1} {\sqrt{Z^2 - Y^2}}
=
\begin{cases}
1 / \sqrt{Z\_0^2 - Y\_0^2} \\\\
1 / \sqrt{Z\_0^2 - aX\_0^2}
\end{cases}
.
$$
To relate these quantities, recall from the curve equation that
$$
-dX\^2Y\^2 = Z\^4 - aZ\^2X\^2 - Z\^2Y\^2,
$$
so
$$
(a-d)X\^2Y\^2 = Z\^4 - aZ\^2X\^2 - Z\^2Y\^2 + aX\^2Y\^2.
$$
Factoring the right-hand side gives
$$
(a-d)X\^2Y\^2 = (Z\^2 - Y\^2)(Z\^2 - aX\^2),
$$
which relates the two quantities we want to compute:
$$
\frac 1 {Z^2 - aX^2} = \frac 1 {a - d} \frac {Z^2 - Y^2} {X^2 Y^2},
$$
so
$$
\frac 1 {\sqrt{Z^2 - aX^2}} = \frac 1 {\sqrt{a - d}} \sqrt{ \frac {Z^2 - Y^2} {X^2 Y^2} }.
$$

## Explicit Encoding Formulas

Using this trick, we can write the encoding procedure explicitly:

1. \\(u\_1 \gets (Z\_0 + Y\_0)(Z\_0 - Y\_0)
    \textcolor{gray}{= Z\_0\^2 - Y\_0\^2}
    \\)
2. \\(u\_2 \gets X\_0 Y\_0 \\)
3. \\(I \gets \mathrm{invsqrt}(u\_1 u\_2\^2)
    \textcolor{gray}{= 1/\sqrt{X\_0\^2 Y\_0\^2 (Z\_0\^2 - Y\_0\^2)}}
   \\)
4. \\(D\_1 \gets u\_1 I
    \textcolor{gray}{= \sqrt{(Z\_0\^2 - Y\_0\^2)/(X\_0\^2 Y\_0\^2)} }
   \\)
5. \\(D\_2 \gets u\_2 I
   \textcolor{gray}{= \pm \sqrt{1/(Z\_0\^2 - Y\_0\^2)} }
   \\)
6. \\(Z\_{inv} \gets D\_1 D\_2 T\_0
   \textcolor{gray}{= (u\_1 u\_2)/(u\_1 u\_2\^2) T\_0 = T\_0 / X\_0 Y\_0 = 1/Z\_0}
   \\)
7. If \\( T\_0 Z\_{inv} \textcolor{gray}{= x\_0 y\_0 }\\) is negative:
    1. \\( (X, Y) \gets (Y\_0 (\pm 1/\sqrt{a}), X\_0 (\mp \sqrt{a})) \\)
    2. \\( D \gets D\_1 / \sqrt{a-d}
           \textcolor{gray}{= 1/\sqrt{Z\_0\^2 - a X\_0\^2} = 1/\sqrt{Z^2 -Y^2} }
       \\)
8. Otherwise:
    1. \\( (X, Y) \gets (X\_0, Y\_0) \\)
    2. \\( D \gets D\_2
           \textcolor{gray}{= \pm \sqrt{1/(Z\_0\^2 - Y\_0\^2)} = \pm 1/\sqrt{Z^2 - Y^2}}
       \\)
9. If \\( X Z\_{inv} \textcolor{gray}{= x} \\) is negative, set \\( Y \gets - Y\\)
10. Compute \\( s \gets |\sqrt{-a} (Z - Y) D| \textcolor{gray}{= |\sqrt{-a} (Z - Y) / \sqrt{Z\^2 - Y\^2}| } \\)
11. Return the canonical byte encoding of \\( s \\).

The choice of \\( Q\_4 = (i, 0) \\) when \\( a = -1 \\) is convenient
since it simplifies 7.1 to \\( (X,Y) \gets (iY_0, iX_0) \\).

## Explicit Decoding Formulas

As with encoding, we want to batch operations to use only a single
inverse square root.  However, the procedure is much simpler since
there's no torquing.

On input `s_bytes`:

1. Check that `s_bytes` is the canonical byte-encoding of a field
element \\(s\\), otherwise reject.
2. Decode `s_bytes` to \\(s\\).
3. Check that \\( s \\) is nonnegative, otherwise reject.
4. \\( u_1 \gets 1 + as^2 \\)
5. \\( u_2 \gets 1 - as^2 \\)
6. \\( v \gets (ad)u_1^2 - u_2^2 \textcolor{gray}{= ad(1+as^2)^2 - (1-as^2)^2} \\)
7. \\( I \gets \mathrm{invsqrt}( v u_2^2 ) \textcolor{gray}{= 1/\sqrt{v u_2^2} } \\)
8. \\( D_x \gets Iu_2 \textcolor{gray}{= 1/\sqrt{v} } \\)
9. \\( D_y \gets ID_x v \textcolor{gray}{= I^2 u_2 v = (v u_2) / (v u_2^2) = 1/u_2 } \\)
10. \\( x \gets |2sD_x| \textcolor{gray}{= +\sqrt{ 4s^2 / (ad(1+as^2)^2 - (1-as^2)^2 )}}\\)
11. \\( y \gets u_1 D_y \textcolor{gray}{= (1+as^2)/(1-as^2) } \\)
12. \\( t \gets xy \\)
12. Check that \\(t \\) is nonnegative and that \\( y \neq 0 \\), otherwise reject.
13. Return \\( P = (x: y: 1: t) \\)

## TODO

Add explicit formulas for the cofactor 4 case.

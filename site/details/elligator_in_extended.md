# Elligator in Extended Coordinates

As noted on the previous page, the Elligator map to the Jacobi
quartic, written in terms of the Ristretto parameters, is given by
\\[ 
(s,t) = 
\left(
\+ \sqrt{
  \frac{
    a_2(r+1)(a_2 + d_2)(d_2 - a_2)
  }{
    (d_2 r - a_2)(a_2 r - d_2)
  }
  }
,
  \frac{
    +a_2(r-1)(a_2 + d_2)^2
  }{
    (d_2 r - a_2)(a_2 r - d_2)
  }
  \- 1
\right)
\\]
or
\\[
(s,t) = 
\left(
\- \sqrt{
  \frac{
    a_2r(r+1)(a_2 + d_2)(d_2 - a_2)
  }{
    (d_2 r - a_2)(a_2 r - d_2)
  }
  }
,
  \frac{
    -a_2r(r-1)(a_2 + d_2)^2
  }{
    (d_2 r - a_2)(a_2 r - d_2)
  }
  \- 1
\right)
\\]
depending on which square root exists, preferring the second when \\(r
= 0\\) and both are square.  (When using the `ristretto255`
parameters, the first is nonsquare when \\(r = 0\\)
and there is no ambiguity).

Applying the isogeny \\(\theta\\) to \\(s,t\\) gives a point on
\\(\mathcal E_2\\).  When the denominator is zero, the Elligator
result should be a 2-torsion point, but as noted in the Decaf paper it
does not matter which one, since we quotient out by \\(2\\)-torsion
anyways.

## Elligator for `ristretto255` in extended coordinates

This can be implemented for `ristretto255` with a single square root
computation, using the `sqrt_ratio_i` function [described in the
explicit formulas section](./formulas/invsqrt.html).  To make this
work, `ristretto255` chooses \\(n = i = +\sqrt{-1}\\) as the quadratic
nonresidue.

Write the numerator and denominator as
\\[
\begin{aligned}
N_s &= a(r+1)(a + d)(d - a) \\\\
D &= (d r - a)(a r - d).
\end{aligned}
\\]
The value of \\(s\\) is then either \\(+\sqrt{N_s/D}\\) or \\(-\sqrt{rN_s/D}\\).
Since \\(r = ir_0^2\\), we have
\\[
-\sqrt{\frac {rN_s} D}
=
-\sqrt{\frac {ir_0^2N_s} D}
=
-\left|
r_0 \sqrt{i \frac {N_s} D}
\right|.
\\]

Because we want to apply \\(\theta\\) to \\((s,t)\\), we can work
projectively and write \\(t = N_t/D\\). Then
\\[
N_t = c(r-1)(a+d)^2 - D,
\\]
where \\(c = a\\) in the first case where \\(N_s/D\\) is square
and \\(c = -ar\\) in the second case where \\(rN_s/D\\) is square.

Applying \\(\theta\\) gives
\\[
(x,y)
=
\left(
\frac { 2s } { t } \frac 1 { \sqrt{ad-1} }
, \quad
\frac { 1 +a s^2 } { 1 - as^2 }
\right)
=
\left(
\frac { 2s D } { N_t \sqrt{ad-1} }
, \quad
\frac { 1 + as^2 } { 1 - a s^2 }
\right).
\\]
In extended coordinates, this becomes
\\[
\begin{aligned}
X &= (2sD)(1-as^2) \\\\
Y &= (1+as^2)(N_t \sqrt{ad-1})\\\\
Z &= (N_t \sqrt{ad-1})(1-as^2)\\\\
T &= (2sD)(1+as^2).
\end{aligned}
\\]

In summary, we can compute the Elligator map directly to extended
coordinates as follows.

1. \\(r \gets ir_0^2\\).
2. \\(N_s \gets (r+1)(1-d^2) 
   \textcolor{gray}{= a(r+1)(a+d)(a-d)}
   \\).
3. \\(c \gets -1\\).
4. \\(D \gets (c - dr)(r+d) 
   \textcolor{gray}{= (dr -a)(ar-d)}
   \\).
5. `Ns_D_is_sq`, \\(s \gets \\) `sqrt_ratio_i(N_s, D)`.
6. \\(s' \gets -|s r_0|\\).
7. \\(s \gets s' \\) if not `Ns_D_is_sq`.
8. \\(c \gets r \\) if not `Ns_D_is_sq`.
9. \\(N_t \gets c(r-1)(d-1)^2 - D\\).
10. \\(W_0 \gets 2sD \\).
11. \\(W_1 \gets N_t\sqrt{ad-1} \\).
12. \\(W_2 \gets 1 - s^2 
   \textcolor{gray}{= 1 +as^2}
   \\).
13. \\(W_3 \gets 1 + s^2 
   \textcolor{gray}{= 1 -as^2}
   \\).
14. Return \\( (W_0 W_3 : W_2 W_1 : W_1 W_3 : W_0 W_2) \\).

When \\(D = 0\\), we need to obtain a \\(2\\)-torsion point, but since
`sqrt_ratio_i` returns `(0, 0)`, the resulting Edwards point will be
the identity \\((0,1)\\), so the exceptional case does not need to be
handled specially.

# Elligator

The Decaf paper explains how to formulate a map from field elements to
points suitable for hashing to group elements.  Rather than mapping
directly to the Edwards or Montgomery model, Decaf suggests using
Elligator 2 to the Jacobi quartic \\(\mathcal J\\), then applying an
isogeny to obtain a point on whichever curve is used for implementing
group operations.

This method also applies to Ristretto, with a suitable change of variables.

## Elligator for Decaf

The Decaf paper constructs the Elligator map as follows, using the
Decaf parameters \\(a_1, d_1\\).  First, fix a parameter \\(n \in
\mathbb F \\) which is nonsquare.  Then, on input \\(r_0 \in \mathbb
F\\), compute \\( r \gets n r_0^2 \\).  Since \\( n \\) is nonsquare,
\\(r\\) is nonsquare unless \\(r_0 = 0\\), in which case \\(r = 0\\).

The output of the Elligator map is either
\\[
(s,t) = 
\left(
\+ \sqrt{
  \frac{
    (r+1)(a_1 - 2d_1)
  }{
    (d_1 r + a_1 - d_1) (d_1 r - a_1 r - d_1)
  }
  }
,
  \frac{
    -(r-1)(a_1 - 2d_1)^2
  }{
    (d_1 r + a_1 - d_1) (d_1 r - a_1 r - d_1)
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
    r(r+1)(a_1 - 2d_1)
  }{
    (d_1 r + a_1 - d_1) (d_1 r - a_1 r - d_1)
  }
  }
,
  \frac{
    r(r-1)(a_1 - 2d_1)^2
  }{
    (d_1 r + a_1 - d_1) (d_1 r - a_1 r - d_1)
  }
  \- 1
\right)
\\]
depending on which square root exists, preferring the second case when
\\(r = 0\\) and both are square.

## Changing variables to Ristretto parameters

We can rewrite this in terms of the Ristretto parameters \\(a\_2,
d\_2\\) using the identities \\(a\_1 = -a\_2\\), \\(d\_1 = a\_2d\_2 /
(a\_2 - d\_2)\\).  The denominator is
\\[
\begin{aligned}
D &= (d_1 r + a_1 - d_1) (d_1 r - a_1 r - d_1) \\\\
  &= 
  \left(
	\frac {a_2 d_2}{ a_2 - d_2}
	r - a_2 - 
	\frac {a_2 d_2}{ a_2 - d_2}
  \right)
  \left(
	\frac {a_2 d_2}{ a_2 - d_2}
	r + a_2r  - 
	\frac {a_2 d_2}{ a_2 - d_2}
  \right)
  \\\\
  &=
  \frac {
  \left(
	a_2 d_2 r - a_2(a_2 - d_2) - a_2 d_2
  \right)
  \left(
    a_2 d_2 r + a_2(a_2 -d_2) r - a_2 d_2
  \right)
  }
  {(a_2 - d_2)^2}
  \\\\
  &= 
  \frac 
  {
  \left(
    a_2 d_2 r - 1
  \right)
  \left(
    r - a_2 d_2
  \right)
  }
  {(a_2 - d_2)^2}
  .
  \\\\
\end{aligned}
\\]
Since \\(a_2^2 = 1\\), this is
\\[
\begin{aligned}
D &=
  \frac 
  {
  a_2^2
  \left(
    a_2 d_2 r - 1
  \right)
  \left(
    r - a_2 d_2
  \right)
  }
  {(a_2 - d_2)^2}
  \\\\
  &=
  \frac 
  {
  \left(
    a_2^2 d_2 r - a_2
  \right)
  \left(
    a_2 r - a_2^2 d_2
  \right)
  }
  {(a_2 - d_2)^2}
  \\\\
  &=
  \frac 
  {
  \left(
    d_2 r - a_2
  \right)
  \left(
    a_2 r - d_2
  \right)
  }
  {(a_2 - d_2)^2}
  ,
  \\\\
\end{aligned}
\\]
so that
\\[
1/D = 
  \frac 
  {(a_2 - d_2)^2}
  {
  \left(
    d_2 r - a_2
  \right)
  \left(
    a_2 r - d_2
  \right)
  }
  .
\\]
The numerators of the terms in the Elligator map are
\\[
\begin{aligned}
(r+1)(a_1 - 2d_1) &\qquad & -(r+1)(a_1 - 2d_1)^2 \\\\
r(r+1)(a_1 - 2d_1) & \qquad & r(r+1)(a_1 - 2d_1)^2; \\\\
\end{aligned}
\\]
since 
\\[
a_1 - 2d_1 = - a_2 \left( \frac{a_2 + d_2}{a_2 - d_2} \right)^2,
\\]
these become
\\[
\begin{aligned}
-a_2 (r+1)
\left( \frac{a_2 + d_2}{a_2 - d_2} \right)
&\qquad &
+a_2 (r-1)
\left( \frac{a_2 + d_2}{a_2 - d_2} \right)^2
\\\\
-a_2 r (r+1)
\left( \frac{a_2 + d_2}{a_2 - d_2} \right)
& \qquad &
-a_2 r (r-1)
\left( \frac{a_2 + d_2}{a_2 - d_2} \right)^2.
\\\\
\end{aligned}
\\]
Multiplying the numerators by the denominator \\(1/D\\) gives
\\[
\begin{aligned}
\frac {
-a_2 (r+1) (a_2 + d_2)(a_2 - d_2)
}{
(d_2 r - a_2)(a_2r-d_2)
}
&\qquad &
\frac {
+a_2 (r-1) (a_2 + d_2)^2
}{
(d_2 r - a_2)(a_2r-d_2)
}
\\\\
\frac {
-a_2 r (r+1) (a_2 + d_2)(a_2 - d_2)
}{
(d_2 r - a_2)(a_2r-d_2)
}
& \qquad &
\frac {
-a_2 r (r-1) (a_2 + d_2)^2
}{
(d_2 r - a_2)(a_2r-d_2)
}
\\\\
\end{aligned}
\\]

## Elligator for Ristretto

The Elligator map, written in terms of the Ristretto parameters, is
therefore given by 
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

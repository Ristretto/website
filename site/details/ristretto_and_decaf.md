# Ristretto and Decaf

In what sense is Ristretto a variant of Decaf?

## Decaf: from Jacobi to Edwards and Montgomery via \\(2\\)-isogeny

The Decaf encoding is parameterized in terms of \\(a\_1, d\_1\\). In the
Decaf paper, these are written as \\(a, d\\), but they're relabeled
here to avoid confusion between Decaf and Ristretto parameters.

The Jacobi quartic \\(\mathcal J = \mathcal J\_{a\_1^2, a\_1 - 2d\_1} \\)
is \\(2\\)-isogenous to the Edwards curve \\(\mathcal E\_1 = \mathcal
E\_{a\_1,d\_1}\\) via the isogeny
\\[
\phi(s,t) = \left( \frac {2s} {1 + a\_1 s^2} , \frac {1 -as^2}{t} \right)
\\]
with dual
\\[
\hat \phi(s,t) = \left( \frac x y , \frac {2 - y^2 - a\_1 x^2} {y^2}\right).
\\]

Decaf uses sign choices to determine an encoding of \\(\mathcal J /
\mathcal J[2]\\), then transports this encoding to \\(\mathcal E\_1\\)
via \\(\phi\\).
However, \\(\mathcal J\\) is also \\(2\\)-isogenous to the Montgomery curve 
\\[
\mathcal M\_{B,A} : Bv^2 = u(u^2 + Au + 1)
\\]
where \\( B = a\_1 \\), \\( A = 2 - 4d\_1/a\_1\\), via the isogeny
\\[
\psi(s,t) = \left( \frac 1 {a_1 s^2} , \frac {-t} {a_1 s^3} \right),
\\]
with dual
\\[
\hat \psi(u,v) = \left (
\frac {1 - u^2} { 2 a_1 v } ,
\frac {a_1 (u + 1)^4 + 8 d_1 u(u^2 +1) } {4 a_1^2 v^2 }
\right).
\\]
This means it is possible to transport an encoding of \\( \mathcal J
/ \mathcal J[2] \\) along \\(\psi\\) to the Montgomery curve; §5 of
the Decaf paper explains how \\(\psi\\) can be used to implement Decaf
using the Montgomery ladder.

## From Montgomery to Edwards via isomorphism

When \\((A+2)/a_2B\\) is a square, the curve \\(\mathcal M_{B,A}\\) is
isomorphic (\\(1\\)-isogenous) to the curve
\\(\mathcal E_2 = \mathcal E_{a_2, d_2} \\)
with \\(a_2 = \pm 1, d_2 = a_2 (A-2)/(A+2)\\) via the maps
\\[
\eta (u,v) = \left( \frac u v \left( \pm \sqrt{ \frac {A+2} {a_2 B} }\right) , \frac {u - 1} {u + 1} \right)
\\]
with inverse (dual)
\\[
\hat \eta (x,y) = \left(
\frac {1+y}{1-y} ,
\frac {1+y}{1-y} \frac 1 x \left( \pm \sqrt {\frac {B a_2} {A+2}} \right)
\right).
\\]
Note that there are actually two maps, one for each choice of square
root.  This means that rather than transporting an encoding of
\\(\mathcal J / \mathcal J[2]\\) along \\(\phi\\) to \\(\mathcal
E_1\\), it's possible to travel along \\( \eta \circ \psi \\) to \\(
\mathcal E_2 \\).

## Ristretto and Decaf

Ristretto uses (different) sign choices to determine an encoding of
\\(\mathcal J / \mathcal J[2]\\), then transports this encoding to
\\(\mathcal E\_2\\) via \\(\theta = \eta \circ \psi \\).
The correspondence between the Ristretto parameters \\(a_2 , d_2\\)
and the Decaf parameters \\(a_1, d_1\\) is given by
\\[
\begin{aligned}
a_2 &= -a_1   & a_1 &= -a_2 \\\\
d_2 &= \frac {a_1 d_1} {a_1 - d_1} & d_1 &= \frac {a_2 d_2 }{a_2 - d_2} \\\\
\end{aligned}
\\]

XXX write up the description of why this is true / rework presentation of \\(\theta\\).




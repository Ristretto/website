# Isogenies

## From Jacobi to Edwards and Montgomery via \\(2\\)-isogeny

The isogenies used by Decaf are parameterized in terms of \\(a\_1, d\_1\\). In the
Decaf paper, these are written as \\(a, d\\), but they're relabeled
here to avoid confusion between Decaf and Ristretto parameters.

As noted in the Decaf paper, the Jacobi quartic
\\(\mathcal J = \mathcal J\_{a\_1^2, a\_1 - 2d\_1} \\)
is \\(2\\)-isogenous to the Edwards curve
\\(\mathcal E\_1 = \mathcal E\_{a\_1,d\_1}\\)
via the isogeny
\\[
\phi(s,t) = \left( \frac {2s} {1 + a\_1 s^2} ,\quad \frac {1 -a\_1 s^2}{t} \right)
\\]
with dual
\\[
\hat \phi(x,y) = \left( \frac x y ,\quad \frac {2 - y^2 - a\_1 x^2} {y^2}\right).
\\]
It is also \\(2\\)-isogenous to the Montgomery curve 
\\(
\mathcal M\_{B,A}
\\)
where \\( B = a\_1 \\), \\( A = 2 - 4d\_1/a\_1\\), via the isogeny
\\[
\psi(s,t) = \left( \frac 1 {a_1 s^2} ,\quad \frac {-t} {a_1 s^3} \right),
\\]
with dual
\\[
\hat \psi(u,v) = \left (
\frac {1 - u^2} { 2 a_1 v } ,\quad
\frac {a_1 (u + 1)^4 + 8 d_1 u(u^2 +1) } {4 a_1^2 v^2 }
\right).
\\]

## From Montgomery to Edwards via isomorphism

When \\((A+2)/a_2B\\) is a square, the curve \\(\mathcal M_{B,A}\\) is
isomorphic (\\(1\\)-isogenous) to the curve
\\(\mathcal E_2 = \mathcal E_{a_2, d_2} \\)
with
\\[
a_2 = \pm 1, \qquad d_2 = a_2 \frac{A-2}{A+2}
\\]
via the map
\\[
\eta (u,v) = \left( \frac u v \left( \pm \sqrt{ \frac {A+2} {a_2 B} }\right) ,\quad \frac {u - 1} {u + 1} \right)
\\]
with inverse (dual)
\\[
\hat \eta (x,y) = \left(
\frac {1+y}{1-y} ,\quad
\frac {1+y}{1-y} \frac 1 x \left( \pm \sqrt {\frac {B a_2} {A+2}} \right)
\right).
\\]
Note that there are actually two maps, one for each choice of square
root.
The parameters \\(a_1, d_1\\) and \\(a_2, d_2\\) are related by
\\[
\begin{aligned}
a_2 &= -a_1   & a_1 &= -a_2 \\\\
d_2 &= \frac {a_1 d_1} {a_1 - d_1} & d_1 &= \frac {a_2 d_2 }{a_2 - d_2}, \\\\
\end{aligned}
\\]
so that
\\[
\mathcal J\_{a\_1^2, a\_1 - 2d\_1}
=
\mathcal J
=
\mathcal J\_{a_2\^2, -a_2\frac{a_2+d_2}{a_2-d_2}}
\\]

## From Jacobi to Edwards via Montgomery

The composition \\( \theta = \eta \circ \psi \\) gives a
\\(2\\)-isogeny from \\(\mathcal J\\) to \\(\mathcal E_2 \\),
which can be written in terms of the \\(\mathcal E_2 \\)
parameters \\(a_2, d_2\\) as
$$
\theta\_{a_2,d_2}(s,t) = \left( \frac{1}{\sqrt{a_2d_2-1}} \cdot \frac{2s}{t},\quad \frac{1+a_2s\^2}{1-a_2s\^2} \right),
$$
with dual
$$
\hat{\theta}\_{a_2,d_2} : (x,y) \mapsto \left( \sqrt{a_2d_2-1} \cdot \frac{xy}{1-a_2x\^2}, \frac{y^2 + a_2x^2}{1-a_2x^2} \right).
$$

# The Isogeny

For \\(a = \pm 1\\), we have a \\(2\\)-isogeny
$$
\theta\_{a,d} : \mathcal J\_{a\^2, -a(a+d)/(a-d)} \longrightarrow \mathcal E\_{a,d}
$$
(or simply \\(\theta\\)) defined by
$$
\theta\_{a,d} : (s,t) \mapsto \left( \frac{1}{\sqrt{ad-1}} \cdot \frac{2s}{t},\quad \frac{1+as\^2}{1-as\^2} \right).
$$
Its dual is 
$$
\hat{\theta}\_{a,d} : \mathcal E\_{a,d} \longrightarrow \mathcal J\_{a\^2, -a(a+d)/(a-d)},
$$
defined by
$$
\hat{\theta}\_{a,d} : (x,y) \mapsto \left( \sqrt{ad-1} \cdot \frac{xy}{1-ax\^2}, \frac{y^2 + ax^2}{1-ax^2} \right)
$$

The kernel of the isogeny is \\( \{(0, \pm 1)\} \\).
The image of the isogeny is \\(\[2\](\mathcal E)\\).  To see this,
first note that because \\( \theta \circ \hat{\theta} = [2] \\), we
know that \\( \[2\](\mathcal E) \subseteq \theta(\mathcal J)\\); then, to see that
\\(\theta(\mathcal J)\\) is exactly \\(\[2\](\mathcal E)\\),
recall that isogenous elliptic curves over a finite field have the
same number of points (exercise 5.4 of Silverman), so that
$$
\\# \theta(\mathcal J) = \frac {\\# \mathcal J} {\\# \ker \theta}
= \frac {\\# \mathcal E}{2} = \\# \[2\](\mathcal E).
$$

To determine the image \\(\theta(\mathcal J[2])\\) of the
\\(2\\)-torsion, we consider the image of the coset
\\(\theta((s,t) + \mathcal J[2])\\).
Let \\((x,y) = \theta(s,t)\\); then
\\(\theta(-s,-t) = (x,y)\\) and
\\(\theta(1/as, -t/as\^2) = (-x, -y)\\),
so that \\(\theta(\mathcal J[2]) = \mathcal E[2]\\).

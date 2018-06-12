# What is Ristretto?

Ristretto is a construction of a prime-order group using a non-prime-order
Edwards curve.

The Decaf paper suggests using a non-prime-order curve \\(\\mathcal E\\) to
implement a prime-order group by constructing a quotient group.  Ristretto uses
the same idea, but with different formulas, in order to allow the use of
cofactor-\\(8\\) curves such as Curve25519.

Internally, a Ristretto point is represented by an Edwards point.  Two Edwards
points \\(P, Q\\) may represent the same Ristretto point, in the same way that
different projective \\( X, Y, Z \\) coordinates may represent the same Edwards
point.  Group operations on Ristretto points are carried out with no overhead
by performing the operations on the representative Edwards points.

To do this, Ristretto defines:

1. a new type for Ristretto points which contains the representative
   Edwards point;

2. equality on Ristretto points so that all equivalent
   representatives are considered equal;

3. an encoding function on Ristretto points so that all equivalent
   representatives are encoded as identical bitstrings;

4. a decoding function on bitstrings with built-in validation, so that only the
   canonical encodings of valid points are accepted;

5. a map from bitstrings to Ristretto points suitable for hash-to-point
   operations.

In other words, an existing Edwards curve implementation can implement the
correct abstraction for complex protocols just by adding a new type and three
or four functions.  Moreover, equality checking for the Ristretto group is
actually less expensive than equality checking for the underlying curve!

The Ristretto construction is described and justified in detail in the [next
chapter](./details/index.html), and explicit formulas aimed at implementors are
in the [Explicit Formulas](./formulas/index.html) chapter.

# Explicit Formulas

As mentioned in the [What is Ristretto](../what_is_ristretto.html) chapter,
Ristretto defines a new point type, together with formulas for encoding,
decoding, equality checking, and hash-to-point operations.

This chapter contains explicit formulas for these operations, aimed at
implementors.

## A note on type safety

Ristretto points are represented by curve points, but they are not curve
points.  Not every curve point is a representative of a Ristretto point.  The
Ristretto group is not a subgroup of the curve, and the Ristretto group is
logically distinct from the group of curve points.

For these reasons, Ristretto points should have a different type than curve
points, and it should be a type error to mix them.  In particular, it's
dangerous and unsafe to implement the Ristretto functions as operating on
arbitrary curve points, rather than only on the representatives contained in a
distinct Ristretto point type.

## Implementation

In terms of invasiveness to a pre-existing curve implementation,
implementing Ristretto requires changing only:

0. A distinct type for Ristretto points which forwards curve operations
   on points to their corresponding canonical representative in the Ristretto
   group (as mentioned in the previous section);
1. A function for equality checking (so that two representatives
   of the same coset are considered equal);
2. A function for encoding (so that two representatives of the
   same coset are encoded as identical bitstrings);
3. A function for decoding (so that only the canonical encoding of
   a coset is accepted);
4. A function for hashing to the Ristretto group.

Internally, each coset is represented by a curve point; two points
\\( P, Q \\) may represent the same coset in the same way that two
points with different \\(X, Y, Z\\) coordinates may represent the
same point.  The group operations are carried out with no overhead
using Edwards formulae.

Additionally, producing a Ristretto implementation will require a hash-to-point
function.  We will explicitly detail this and the above changes in the following
sections.

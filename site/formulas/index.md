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

Implementing Ristretto using an existing Edwards curve implementation requires the following:

0. A distinct type for Ristretto points which forwards curve operations
   on points to their corresponding canonical representative in the Ristretto
   group (as mentioned in the previous section);
1. [A function for decoding](formulas/decoding.html) (so that only the canonical encoding of
   a coset is accepted);
2. [A function for encoding](formulas/encoding.html) (so that two representatives of the
   same coset are encoded as identical bitstrings);
3. [A function for equality checking](formulas/equality.html) (so that two representatives
   of the same coset are considered equal);
4. [A function for hashing to the Ristretto group](formulas/elligator.html).

Implementation of these functions requires an inverse square root
function.  This is often inlined into a point decompression function,
so we also [give formulas for implementing an inverse square root for
`ristretto255`](formulas/invsqrt.html).

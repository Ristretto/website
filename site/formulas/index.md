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

## TODO

This section should have explicit formulas for Ristretto, aimed at
implementations.

It should also have a description of "how to implement Ristretto", i.e.,
what functionality is required: encoding, decoding, equality,
hash-to-point, type safety, etc.

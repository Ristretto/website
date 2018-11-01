# Ristretto in Detail

This section contains details and justification on how and why Ristretto
works, and a derivation of the formulas contained in the [Explicit
Formulas][explicit_formulas] chapter.

These notes are meant to be self-contained, but it may be also be useful
to consult the [Decaf paper][decaf_paper].  Decaf constructs a
prime-order group from a cofactor-\\(4\\) Edwards curve by defining an
encoding of a related Jacobi quartic, then transporting the encoding
from the Jacobi quartic to the Edwards curve by means of an isogeny.

Ristretto uses the same strategy, but with a different isogeny,
different sign choices, and is explicitly designed to cover the
cofactor-\\(8\\) case, making it easy to use with a cofactor-\\(8\\)
curve such as Curve25519.  This construction also allows
implementations to replace the Edwards form of Curve25519 with a
faster curve while maintaining interoperability.

[decaf_paper]: https://eprint.iacr.org/2015/673.pdf
[explicit_formulas]: ./formulas/index.html

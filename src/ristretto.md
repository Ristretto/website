<img style="float: left; width: 25%;" src="/theme/ristretto-sm.png">

**Ristretto** is a variant of Decaf compatible with cofactor-\\(8\\) curves,
such as Curve25519.

In particular, this allows an existing Curve25519 library to implement a
prime-order group with only a thin abstraction layer, and makes it possible
for systems using Ed25519 signatures to be safely extended with zero-knowledge
protocols, with **no additional cryptographic assumptions** and **minimal code
changes**.

## Organization

This site is organized into several chapters:

- [Why Ristretto?](./why_ristretto.html) describes the pitfalls of the cofactor abstraction mismatch.
- [What is Ristretto?](./what_is_ristretto.html) describes what Ristretto provides to protocol implementors.
- [Ristretto in Detail](./details/index.html) contains mathematical justification for why Ristretto works.
- [Explicit Formulas](./formulas/index.html) describes how to implement Ristretto.
- [Test Vectors](./test_vectors/index.html) contains test vectors for the Ristretto functions.
- [Ristretto Implementations](./implementations.html) contains a list of implementations of Ristretto.

## TODO

This is the landing page. It should link to the other pages, have the
coffee graphic, and have the author list.


# Why Ristretto?

## Background

Many cryptographic protocols require an implementation of a group of prime
order \\(q\\), usually an elliptic curve group.  However, modern elliptic curve
implementations with fast, simple formulas don't provide a prime-order group.
Instead, they provide a group of order \\(h \cdot q\\) for a small cofactor,
usually \\( h = 4 \\) or \\( h = 8\\).  

In many existing protocols, the complexity of managing this abstraction is
pushed up the stack via ad-hoc protocol modifications.  But these modifications
are a recurring source of vulnerabilities and subtle design complications, and
they usually prevent applying the security proofs of the abstract protocol.

On the other hand, prime-order curves provide the correct abstraction, but
their formulas are slower and more difficult to implement in constant time.  A
clean solution to this dilemma is Mike Hamburg's [Decaf proposal][decaf_paper],
which shows how to use a cofactor-\\(4\\) curve to provide a prime-order group
– with no additional cost.

This provides the best of both choices: the correct abstraction required to
implement complex protocols, and the simplicity, efficiency, and speed of a
non-prime-order curve.  However, many systems use Curve25519, which has
cofactor \\(8\\), not cofactor \\(4\\).  

**Ristretto** is a variant of Decaf designed for compatibility with
cofactor-\\(8\\) curves, such as Curve25519.  It is particularly well-suited
for extending systems using Ed25519 signatures with complex zero-knowledge
protocols.

## Pitfalls of a cofactor

## Why can't we just multiply by the cofactor?

## TODO

This page should have writing explaining the context for the problem
Ristretto solves.

[decaf_paper]: https://eprint.iacr.org/2015/673.pdf


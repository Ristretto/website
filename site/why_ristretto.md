# Why Ristretto?

## Background

Many cryptographic protocols require an implementation of a group of prime
order \\( \ell \\), usually an elliptic curve group.  However, modern elliptic curve
implementations with fast, simple formulas don't provide a prime-order group.
Instead, they provide a group of order \\(h \cdot \ell \\) for a small cofactor,
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

Curve cofactors have caused several vulnerabilities in higher-layer protocol
implementations.  The abstraction mismatch can also have subtle consequences for
programs using these cryptographic protocols, as design quirks in the protocol
bubble up—possibly even to the UX level.

The malleability in Ed25519 signatures
[caused a double-spend vulnerability][monero]—or, technically, octuple-spend as
\\( h = 8\\)—in the CryptoNote scheme used by the Monero cryptocurrency, where
the adversary could add a low-order point to an existing transaction, producing
a new, seemingly-valid transaction.

In Tor, Ed25519 public key malleability would mean that every v3 onion service
has eight different addresses, causing mismatches with user expectations and
potential gotchas for service operators.  Fixing this required
[expensive runtime checks][bug22006] in the v3 onion services protocol,
requiring a full scalar multiplication, point compression, and equality check.
This check [must be called in several places][hs_address_is_valid] to validate
that the onion service's key does not contain a small torsion component.

## Why can't we just multiply by the cofactor?

In some protocols, designers can specify appropriate places to multiply by the
cofactor \\(h\\) to "fix" the abstraction mismatches; in others, it's unfeasible
or impossible.  In any case, multiplying by the cofactor often means that
security proofs are not cleanly applicable.

As touched upon earlier, a curve point consists of an \\( h \\)-torsion
component and an \\( \ell \\)-torsion component.  Multiplying by the cofactor is
frequently referred to as "clearing" the low-order component, however doing so
affects the \\( \ell \\)-torsion component, effectively mangling the point.
While this works for some cases, it is not a validation method.
To validate that a point is in the prime-order subgroup, one can alternately
multiply by \\( \ell \\) and check that the result is
the identity.  But this is extremely expensive.

Another option is to mandate that all scalars have particular bit patterns, as
in X25519 and Ed25519.  However, this means that scalars are no longer
well-defined \\( \mathrm{mod} \ell \\), which makes HKD schemes
[much more complicated][hierarchical_keys].  Yet another approach is to
choose a [torsion-safe representative][torsion_safe]:
an integer which is \\( 0 \mathrm{mod} h \\) and with a particular value \\(
\mathrm{mod} \ell \\), so that scalar multiplications remove the low-order
component.  But these representatives are a few bits too large to be used with
existing implementations, and in any case aren't a comprehensive
solution.

## A comprehensive solution

Rather than bit-twiddling, point mangling, or otherwise kludged-in ad-hoc
fixes, Ristretto is a thin layer that provides protocol
implementors with the correct abstraction: a prime-order group.

[decaf_paper]: https://eprint.iacr.org/2015/673.pdf
[monero]: https://moderncrypto.org/mail-archive/curves/2017/000898.html
[hierarchical_keys]: https://moderncrypto.org/mail-archive/curves/2017/000858.html
[bug22006]: https://trac.torproject.org/projects/tor/ticket/22006#comment:13
[hs_address_is_valid]: https://github.com/torproject/tor/search?q=hs_address_is_valid&unscoped_q=hs_address_is_valid
[torsion_safe]: https://moderncrypto.org/mail-archive/curves/2017/000866.html

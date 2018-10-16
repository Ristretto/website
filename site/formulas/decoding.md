# Decoding to Extended Coordinates

Given 32 octets of data, `s_bytes`, representing a compressed
Ristretto point, the implementation must:

1. Check that `s_bytes` is in fact 32 octets.  (In typed languages the
   implementor should guarantee this through the type system, otherwise
   there should be an explicit check.)
2. Check that the encoding `s_bytes` is canonical\\( \dag \\) and
   then decode `s_bytes` into a field element, `s`.
3. Check that `s` is non-negative.

If any of the above conditions are not met, the decoding routine must abort.

Otherwise, the decoding routine is as follows:

4. Compute two field elements, `u_1` as \\( 1 - a s^{2} \\),
   and `u_2` as \\( 1 + a s^{2} \\),
   where in the case of Ristretto255 \\( a = -1 \\).
5. Compute `v` as \\( ad(1 - a s^{2}) - (1 + a s^{2}) \\), where
   the Edwards curve constants \\( a = -1 \\) and
   \\( d = \frac{-121665}{121666} \mod p\\) for the
   case of Ristretto255.
6. Compute `I` as \\( invsqrt( v u_1^{2} ) \\).
7. Compute `D_x` as \\( I u_2 \\).
8. Compute `D_y` as \\( I D_x v \\).
9. Compute the field element `x` as \\( 2 s Dx \\).
10. If `x` is negative, then negate it.
11. Compute `y` as \\( u_2 D_y \\).
12. Compute `t` as \\( x y \\).
13. If the inverse square root in step #6 was not correct\\( \ddag \\),
    or `t` is negative, or `y` is
    zero, abort.  Otherwise, return the Ristretto point
    \\(( x, y, 1, t \\)).

\\( \dag \\) One way to check that the encoding is canonical is to
decode `s_bytes` into `s`, then re-encode `s` into `s_bytes_check`,
and then check that `s_bytes` and `s_bytes_check` are equal.  This is
done to ensure that the field element `s` satisfies
\\( s < p : p \in (2^{255}, 2^{255}-19] \\) (for the case of Ristretto255).
However, for this to work, the implementation of field element
encoding MUST be canonical and the decoding MUST reject the high bit
(for the case of Ristretto255).

\\( \ddag \\) 
Using the same trick as in ed25519 decoding, we merge the
inversion, the square root, and the square test as follows.

To compute sqrt(α), we can compute β = α^((p+3)/8).
Then β^2 = ±α, so multiplying β by sqrt(-1) if necessary
gives sqrt(α).

To compute 1/sqrt(α), we observe that
   1/β = α^(p-1 - (p+3)/8) = α^((7p-11)/8)
                           = α^3 * (α^7)^((p-5)/8).

We can therefore compute sqrt(u/v) = sqrt(u)/sqrt(v)
by first computing
   r = u^((p+3)/8) v^(p-1-(p+3)/8)
     = u u^((p-5)/8) v^3 (v^7)^((p-5)/8)
     = (uv^3) (uv^7)^((p-5)/8).

If v is nonzero and u/v is square, then r^2 = ±u/v,
                                    so vr^2 = ±u.
If vr^2 =  u, then sqrt(u/v) = r.
If vr^2 = -u, then sqrt(u/v) = r*sqrt(-1).

If v is zero, r is also zero.

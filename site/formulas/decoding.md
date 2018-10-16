# Decoding to Extended Coordinates

To decode a byte-string `s_bytes` representing a compressed Ristretto
point into extended coordinates, an implementation proceeds as
follows.

1. Check that `s_bytes` is the canonical encoding of a field element, or else abort.
2. Decode `s_bytes`, a byte-string, into \\(s\\), a field element.
3. Check that \\(s\\) is non-negative, or else abort.
4. Compute \\(u_1 \gets 1 - a s^{2} \\) and \\(u_2 \gets 1 + a s^{2} \\).
5. Compute \\(v \gets adu_1^2 - u_2^2 \\).
6. Compute \\(I \gets \mathrm{invsqrt}( v u_1^{2} ) \\), or abort if the square root does not exist.
7. Compute \\(D_x \gets I u_2 \\).
8. Compute \\(D_y \gets I D_x v \\).
9. Compute \\(x \gets |2 s D_x| \\), i.e., compute \\(2sD_x\\) and negate it if it is negative.
11. Compute \\(y \gets u_2 D_y \\).
12. Compute \\(t \gets x y \\).
13. If \\( t \\) is negative or \\( y = 0 \\), abort.
    Otherwise, return the Ristretto point represented by
    \\(( x: y: 1: t \\)).

Since \\( a = \pm 1 \\), implementations can replace multiplications
by \\(a\\) with sign changes, as appropriate.

If the implementation's field element encoding function produces
canonical outputs, one way to check that `s_bytes` is a canonical
encoding (in step 1) is to decode `s_bytes` into \\(s\\), then
re-encode \\(s\\) into `s_bytes_check`, and ensure that `s_bytes ==
s_bytes_check`.

The `ristretto255` test vectors include checks for non-canonical decoding.
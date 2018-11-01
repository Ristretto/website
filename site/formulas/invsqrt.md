# Extracting an Inverse Square Root

Implementing Ristretto requires computing (inverse) square roots.
Ed25519 implementations compute square roots as part of the Ed25519
decompression functionality, but this is often inlined into the
decompression formulas.

This page explains how to extract an inverse square root function
suitable for use with `ristretto255` from an implementation of Ed25519.

## Computing \\(\sqrt{u/v}\\) or \\(\sqrt{iu/v}\\) simultaneously

Let \\(p = 2^{255} -19\\).  On input field elements \\(u, v\\), set
\\[
r = (uv^3)(uv^7)^{(p-5)/8}.
\\]

There are four possibilities when \\(u,v\\) are nonzero:

* If \\(vr^2 =  u\\), then \\( s \gets r \\) is a square root of \\(u/v\\).
* If \\(vr^2 = -u\\), then \\( s \gets ir \\) is a square root of \\(u/v\\).
* If \\(vr^2 =  iu\\), then \\( s \gets r \\) is a square root of \\(iu/v\\).
* If \\(vr^2 = -iu\\), then \\( s \gets ir \\) is a square root of \\(iu/v\\).

When \\(u = 0\\), \\(r = 0\\) and the first condition \\(vr^2 = u\\)
is satisfied.  When \\(v = 0\\) and \\(u \neq 0\\), \\(r = 0\\) and
none of the conditions are satisfied.
This allows computing the nonnegative square root of either 
\\( \sqrt{u/v} \\) or \\(\sqrt{iu/v}\\) as follows:

1. \\(r \gets (uv^3)(uv^7)^{(p-5)/8}\\).
2. \\(c \gets vr^2\\).
3. `correct_sign_sqrt` \\(\gets (c = u)\\).
4. `flipped_sign_sqrt` \\(\gets (c = -u)\\).
5. `flipped_sign_sqrt_i` \\(\gets (c = -iu)\\).
6. \\( r \gets ir \\) if `flipped_sign_sqrt | flipped_sign_sqrt_i`.
7. \\( r \gets -r \\) if \\(r\\) is negative.
8. Return `(correct_sign_sqrt | flipped_sign_sqrt, r)`.

The first part of the return value signals whether \\(u/v\\) was
square, and the second part contains a square root.  Specifically,
it returns

- `( true,   +sqrt(u/v))` if \\(v\\) is nonzero and \\(u/v\\) is square;
- `( true,         zero)` if \\(u\\) is zero;
- `(false,         zero)` if \\(v\\) is zero and \\(u\\) is nonzero;
- `(false, +sqrt(i*u/v))` if \\(u/v\\) is nonsquare (so \\(iu/v\\) is square).

The computation of \\(x^{(p-5)/8}\\) is already required for Ed25519
decoding, so an Ed25519 implementation can obtain a `sqrt_ratio_i`
function by refactoring the computation of \\(r\\) out of the decoding
function and using `sqrt_ratio_i` to perform decoding.

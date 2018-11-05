# Hash-to-Group with Elligator

The hash-to-group operation applies Elligator twice and adds the results.

## Hash-to-`ristretto255`

Given `bytes`, 64 bytes of hash output, construct field elements \\(r_0, r_1\\) as

* \\(r_0\\) is the low 255 bits of `bytes[ 0..32]`, taken mod \\(p\\).
* \\(r_1\\) is the low 255 bits of `bytes[32..64]`, taken mod \\(p\\).

Apply the Elligator map below to the inputs \\(r_0, r_1\\) to obtain points \\(P_1, P_2\\), 
and return \\(P_1 + P_2\\).

1. \\(r \gets ir_0^2\\).
2. \\(N_s \gets (r+1)(1-d^2) 
   \textcolor{gray}{= a(r+1)(a+d)(a-d)}
   \\).
3. \\(c \gets -1\\).
4. \\(D \gets (c - dr)(r+d) 
   \textcolor{gray}{= (dr -a)(ar-d)}
   \\).
5. `Ns_D_is_sq`, \\(s \gets \\) `sqrt_ratio_i(N_s, D)`.
6. \\(s' \gets -|s r_0|\\).
7. \\(s \gets s' \\) if not `Ns_D_is_sq`.
8. \\(c \gets r \\) if not `Ns_D_is_sq`.
9. \\(N_t \gets c(r-1)(d-1)^2 - D\\).
10. \\(W_0 \gets 2sD \\).
11. \\(W_1 \gets N_t\sqrt{ad-1} \\).
12. \\(W_2 \gets 1 - s^2 
   \textcolor{gray}{= 1 +as^2}
   \\).
13. \\(W_3 \gets 1 + s^2 
   \textcolor{gray}{= 1 -as^2}
   \\).
14. Return \\( (W_0 W_3 : W_2 W_1 : W_1 W_3 : W_0 W_2) \\).

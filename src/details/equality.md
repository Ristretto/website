# Equality Testing

Testing equality of two Ristretto points means testing whether they
are equal in the quotient group, i.e., whether they lie in the same
coset of \\(\mathcal E[4] \\) (for the cofactor-\\(8\\) case) or
\\(\mathcal E[2] \\) (for the cofactor-\\(4\\) case).

Equality testing of points on the Edwards curve requires comparing to
affine coordinates, which requires an expensive inversion.  However,
testing whether two points lie in the same coset can be done in
projective coordinates, making it actually *easier* than equality
testing in the original non-quotient group.

XXX write up details

# Batched Double-and-Encode

The encoding is not batchable, since it requires an inverse square
root.  However, since \\( \theta \circ \hat \theta = [2] P \\), it's
possible to compute the encoding of \\( [2]P \\) by using \\( \hat
\theta \\) instead of \\( \theta^{-1} \\).  Since \\( \hat \theta \\) only
requires inversions, given \\( P\_1, \ldots, P\_n \\), it's possible
to compute the encodings of \\( [2]P\_1, \ldots, [2]P\_n \\) in a
batch.

XXX write up details

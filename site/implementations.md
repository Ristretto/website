# `ristretto255` Implementations

An [internet-draft][id] for ristretto255 is now available, written by
Henry de Valence, Jack Grigg, George Tankersley, Filippo Valsorda, and
Isis Lovecruft.

* Rust: [`curve25519-dalek`][dalek], by Isis Lovecruft and Henry de Valence;
* Go: `ristretto255` (forthcoming), by George Tankersley and Filippo Valsorda;
* C: `ristretto-donna` (forthcoming), by Isis Lovecruft, based on Andrew Moon's `ed25519-donna`;
* C: [`libsodium`][libsodium] (currently in git master branch), by Frank Denis;
* AssemblyScript: [`wasm-crypto`][wasm-crypto] (forthcoming), by Frank Denis;

[id]: https://datatracker.ietf.org/doc/draft-hdevalence-cfrg-ristretto/
[dalek]: https://doc.dalek.rs/curve25519_dalek/
[libsodium]: https://libsodium.org
[wasm-crypto]: https://github.com/jedisct1/wasm-crypto

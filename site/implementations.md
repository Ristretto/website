# `ristretto255` Implementations

An [internet-draft][id] for ristretto255 is now available, written by
Henry de Valence, Jack Grigg, George Tankersley, Filippo Valsorda, and
Isis Lovecruft.

* Rust: [`curve25519-dalek`][dalek], by Isis Lovecruft and Henry de Valence;
* Go: [`ristretto255`][go_ristretto255], by George Tankersley, Filippo Valsorda,
  and Henry de Valence;
* C: [`libsodium`][libsodium], by Frank Denis.
* AssemblyScript: [`wasm-crypto`][wasm-crypto], by Frank Denis.
* Javascript: [`ristretto255-js`][ristretto255-js], by Valeria Nikolaenko and Kevin Lewi.

[id]: https://datatracker.ietf.org/doc/draft-hdevalence-cfrg-ristretto/
[dalek]: https://doc.dalek.rs/curve25519_dalek/
[libsodium]: https://download.libsodium.org/doc/advanced/point-arithmetic/ristretto
[go_ristretto255]: https://github.com/gtank/ristretto255
[wasm-crypto]: https://github.com/jedisct1/wasm-crypto
[ristretto255-js]: https://github.com/calibra/ristretto255-js

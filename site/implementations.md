# `ristretto255` Implementations

An [internet-draft][id] for ristretto255 is now available, written by
Henry de Valence, Jack Grigg, George Tankersley, Filippo Valsorda, and
Isis Lovecruft.

* Rust: [`curve25519-dalek`][dalek], by Isis Lovecruft and Henry de Valence;
* Go: [`ristretto255`][go_ristretto255], by George Tankersley, Filippo Valsorda,
  and Henry de Valence;
* C: [`libsodium`][libsodium], by Frank Denis.
* AssemblyScript: [`wasm-crypto`][wasm-crypto], by Frank Denis.
* Javascript with Typescript: [`noble-ed25519`][noble], by Paul Miller
* Javascript: [`ristretto255-js`][ristretto255-js], by Valeria Nikolaenko and Kevin Lewi.
* Zig: the [`zig`][zig] programming language includes ristretto255 in its standard library, implemented by Frank Denis.

[id]: https://ietf.org/id/draft-irtf-cfrg-ristretto255-00.html
[dalek]: https://doc.dalek.rs/curve25519_dalek/
[libsodium]: https://download.libsodium.org/doc/advanced/point-arithmetic/ristretto
[go_ristretto255]: https://github.com/gtank/ristretto255
[noble]: https://github.com/paulmillr/noble-ed25519
[wasm-crypto]: https://github.com/jedisct1/wasm-crypto
[ristretto255-js]: https://github.com/calibra/ristretto255-js
[zig]: https://ziglang.org/

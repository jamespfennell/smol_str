# smol_str

[![CI](https://github.com/rust-analyzer/smol_str/workflows/CI/badge.svg)](https://github.com/rust-analyzer/smol_str/actions?query=branch%3Amaster+workflow%3ACI)
[![Crates.io](https://img.shields.io/crates/v/smol_str.svg)](https://crates.io/crates/smol_str)
[![API reference](https://docs.rs/smol_str/badge.svg)](https://docs.rs/smol_str/)


A `SmolStr` is a string type that has the following properties:

* `size_of::<SmolStr>() == size_of::<String>()`
* `Clone` is `O(1)`
* Strings are stack-allocated if they are:
    * Up to 22 bytes long
    * Longer than 22 bytes, but substrings of `WS` (see `src/lib.rs`). Such strings consist
    solely of consecutive newlines, followed by consecutive spaces
* If a string does not satisfy the aforementioned conditions, it is heap-allocated

Unlike `String`, however, `SmolStr` is immutable. The primary use case for
`SmolStr` is a good enough default storage for tokens of typical programming
languages. Strings consisting of a series of newlines, followed by a series of
whitespace are a typical pattern in computer programs because of indentation.
Note that a specialized interner might be a better solution for some use cases.

## Cargo features

- `std` (on by default): use the Rust standard library.
    Disable for `no_std` mode.

- `derive`: implement Serde traits for `SmolStr`.

- `nosync`: use `Rc` internally instead of `Arc`. This makes the `SmolStr` type 
    more performant in single-threaded contexts, but it will not be `Send` or `Sync`.

## MSRV Policy

Minimal Supported Rust Version: latest stable.

Bumping MSRV is not considered a semver-breaking change.

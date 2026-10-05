# AGENTS.md

Motoko tooling (lexer, parser, and partial interpreter) implemented as a Rust library.

## Workspace layout

A Cargo workspace with two crates:

- `crates/motoko` — the main library (crate name `motoko`) and the `mo-rs` binary.
- `crates/motoko_proc_macro` — procedural macros used by the library.

Other top-level directories:

- `node/` — a Node.js script that regenerates bundled Motoko package data (see Generated files).
- `submodules/` — git submodules (`dfinity/motoko`, `dfinity/motoko-base`) consumed by the generator; initialize with `git submodule update --init`.
- `docs/` — reference material (e.g. `grammar.txt`).

## Build, test, format, lint

Run from the repository root:

```
cargo build
cargo test
cargo fmt
cargo clippy
```

CI (`.github/workflows/rust.yml`) builds and tests on the **nightly** toolchain
with the `rustfmt` and `clippy` components installed. Use nightly to match CI.

The `mo-rs` binary and its REPL require the `exe` feature:

```
cargo run --features exe --bin mo-rs
```

Default features are `parser` and `to-motoko`. Other features: `exe`,
`serde-paths`, `reflection`, `value-reflection`, `core-reflection`.

## Benchmarks

Benchmarks use Criterion (harness disabled) at `crates/motoko/benches/benchmark.rs`:

```
cargo bench
```

CI (`.github/workflows/bench.yml`) runs benchmarks on PRs comparing against the
base branch; it uses the stable toolchain.

## License check

`.github/workflows/license.yml` runs `cargo-deny check bans licenses sources`.
The job is named `license-check:required`, so new dependencies must satisfy the
`cargo-deny` configuration or this required check fails.

## Generated files (do not hand-edit)

`node/generate.js` regenerates these files; edit the generator or sources, not
the outputs:

- `crates/motoko/src/packages/prim.mo`
- `crates/motoko/src/packages/base.json`
- `crates/motoko/src/packages/base-test.json`
- `crates/motoko/src/packages/matchers.json`

To regenerate (requires initialized submodules):

```
cd node && npm install && npm run generate
```

## Conventions

- `Cargo.lock` is git-ignored (this is a library); do not commit it.
- The parser is generated at build time by LALRPOP via `crates/motoko/build.rs`.

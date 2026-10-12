# AGENTS.md

Motoko tools (lexer, parser, interpreter) implemented as a Rust library.

## Layout

This is a Cargo workspace with two crates:

- `crates/motoko` — the main library (crate `motoko`) and the `mo-rs` binary.
- `crates/motoko_proc_macro` — procedural macros used by the library.
- `submodules/` — git submodules (`motoko`, `motoko-base`) used only to generate package data; initialize with `git submodule update --init`.
- `node/` — a Node.js script that regenerates bundled Motoko package data (see Generated files).
- `docs/` — reference material (`grammar.txt`).

## Build, test, format, lint

Run from the repository root.

- Build: `cargo build`
- Test: `cargo test`
- Format: `cargo fmt`
- Lint: `cargo clippy`

CI builds and tests on the Rust **nightly** toolchain with the `rustfmt` and `clippy` components installed.

## Generated files — do not hand-edit

- `crates/motoko/src/lib/parser.rs` is generated from `crates/motoko/src/lib/parser.lalrpop` by the LALRPOP build script (`crates/motoko/build.rs`). It is gitignored; edit the `.lalrpop` grammar instead.
- `crates/motoko/src/packages/{base,base-test,matchers}.json` and `crates/motoko/src/packages/prim.mo` are produced by `node/generate.js`. Regenerate with `npm install` then `npm run generate` in `node/` (requires the submodules checked out).

## CI checks

- `cargo-deny check bans licenses sources` must pass (License Check workflow). New dependencies must satisfy its license and source policy.
- A benchmark comparison runs on pull requests. Benchmarks live in `crates/motoko/benches/benchmark.rs`; the `crates/motoko/_benches/` directory is not wired into the Cargo bench harness.

## Notes

- `Cargo.lock` is gitignored (library crate).

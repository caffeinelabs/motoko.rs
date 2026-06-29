# AGENTS.md

Motoko language tooling for Rust: a lexer, parser, and partially-implemented interpreter exposed as the `motoko` library crate (binary `mo-rs`).

## Workspace layout

This is a Cargo workspace with two crates:

- `crates/motoko` — the main library, binary (`mo-rs`), tests, and benchmarks.
- `crates/motoko_proc_macro` — proc-macro support crate.

Other top-level directories:

- `node/` — Node.js script that regenerates the bundled Motoko packages (see below).
- `submodules/` — git submodules (`motoko`, `motoko-base`) used as input to package generation.
- `docs/` — reference notes (e.g. `grammar.txt`).

## Build, test, format

Run from the repository root unless noted.

- Build: `cargo build`
- Test: `cargo test`
- Format: `cargo fmt`
- Lint: `cargo clippy`
- Benchmarks: `cargo bench` (defined in `crates/motoko`)

CI (`.github/workflows/rust.yml`) builds and tests on the **nightly** toolchain with `rustfmt` and `clippy` components installed.

To see logs from failing tests: `RUST_LOG=trace cargo test -- --nocapture`. Tests that rely on this use `test_log::test`.

## Generated files — do not hand-edit

`crates/motoko/src/packages/` is generated. `node/generate.js` (run via `npm run generate` in `node/`) writes `prim.mo`, `base.json`, `base-test.json`, and `matchers.json` there from the `submodules/`. Regenerate instead of editing by hand.

The parser is generated from `crates/motoko/src/lib/parser.lalrpop` by `build.rs` using LALRPOP at build time.

## Conventions and gotchas

- `Cargo.lock` is gitignored (this is a library), so builds resolve fresh.
- The `mo-rs` binary requires the `exe` feature: `cargo run --features exe`.
- License/dependency policy is enforced in CI by `cargo-deny` (`.github/workflows/license.yml`), which runs `check bans licenses sources`; the crate is `Apache-2.0`.
- After cloning, initialize submodules (`git submodule update --init`) before running the package generator.

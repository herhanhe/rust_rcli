# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Build & Test Commands

```bash
# Format code
cargo fmt

# Type check
cargo check --all

# Lint (must pass CI)
cargo clippy --all-targets --all-features --tests --benches -- -D warnings

# Run tests (uses nextest)
cargo nextest run --all-features

# Run tests with warnings if no tests found
cargo nextest run --all-features --no-tests warn

# Run a single test
cargo nextest run --all-features <test_name>

# Dependency security/license audit
cargo deny check -d

# Spell check
typos

# Generate changelog
git-cliff -o CHANGELOG.md

# Run all pre-commit checks
pre-commit run --all-files
```

## Architecture

Minimal Rust CLI template project (from `tyr-rust-bootcamp/template`). Currently a single binary crate with no dependencies.

- **Entry point**: [src/main.rs](src/main.rs) — prints "Hello, Herhan!"
- **Dependencies**: `Cargo.toml` [dependencies] is empty; add new deps here as needed
- **New modules**: add `.rs` files under `src/` with `mod` declarations in `main.rs`

## CI Pipeline

GitHub Actions (see [.github/workflows/build.yml](.github/workflows/build.yml)) runs on push/PR to `main`:
1. `cargo fmt -- --check`
2. `cargo check --all`
3. `cargo clippy` with `-D warnings` (warnings are errors)
4. `cargo nextest run --all-features --no-tests warn`
5. On `v*` tag: generates changelog via git-cliff, creates GitHub Release

## Pre-commit Hooks

Configured in [.pre-commit-config.yaml](.pre-commit-config.yaml). Runs on every commit: cargo fmt, cargo deny, typos, cargo check, cargo clippy, cargo nextest, plus general hooks (EOF fixes, trailing whitespace, YAML lint, merge-conflict check, etc.).

## Key Tooling

- **cargo-nextest**: test runner (faster than `cargo test`)
- **cargo-deny**: dependency security/licensing/bans audit (config: `deny.toml`)
- **typos-cli**: spell checker (config: `_typos.toml`, excludes CHANGELOG.md and notebooks/)
- **git-cliff**: changelog generation (config: `cliff.toml`, repo URL: `https://github.com/herhanhen/rust_rcli`)
- **pre-commit**: gate before every commit; all checks must pass

## Assets

- [assets/juventus.csv](assets/juventus.csv): football dataset for course exercises

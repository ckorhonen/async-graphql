# async-graphql

## Architecture and setup

This Cargo workspace is a Rust GraphQL library, not a standalone service. Rust 1.89+ is the manifest minimum. `src/` owns schema registration, validation, and execution; `parser/` and `value/` own syntax/value representations; `derive/` owns procedural macros; `integrations/` adapts HTTP frameworks. Read `ARCHITECTURE.md` for changes crossing these boundaries. Preserve schema, coercion, validation, subscription, and public API compatibility with focused cases in `tests/` or the affected crate.

Cargo resolves dependencies from the manifests. `examples/` is an external Git submodule, so a normal clone does not supply runnable examples; initialize it only if that example is needed. There is no root dev-server command.

## Checks

Use `cargo test -p async-graphql` or the affected workspace package while iterating, then `cargo build --workspace --verbose`, `cargo test --workspace`, and `cargo clippy --all` as relevant. Formatting uses `cargo +nightly-2025-04-09 fmt --all -- --check`, matching the CI formatter and requiring that toolchain.

Feature-sensitive changes need `.github/workflows/ci.yml`'s feature matrix: it builds all features and separately computes the set excluding `boxed-trait` before testing the workspace. A default-feature pass is not the same coverage. Documentation examples use mdBook plus compiled dependencies (`mdbook test -L target/debug/deps ./docs/en` or `./docs/zh-CN`). Framework adapters need their package's tests; don't assume root library tests exercise the HTTP boundary.

## Finishing work

Follow the nearby implementation and keep changes within the requested scope. Carry authorized changes through the relevant checks, fixing failures caused by the change. For a bug, reproduce the affected behavior and add a focused regression check when useful. Make routine reversible choices without another approval; ask only when missing information materially changes correctness, scope, or authorization, and name the exact source of any blocking rule. Report what changed, checks actually run, and concrete unverified behavior; repeat checks when new edits or evidence warrant it.

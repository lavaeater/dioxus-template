# {{project-name}}

A Dioxus 0.8 app generated from `lavaeater/dioxus-template`. Dioxus API guide (written
for 0.7, still mostly accurate): @AGENTS.md

## Building, linting, testing

Run exactly these commands — they match the bacon jobs in `bacon.toml`, so builds share
artifacts instead of rebuilding each other. A different `--features` combination compiles
a different dependency graph.

| Purpose | Command |
|---|---|
| Lint (default platform) | `cargo clippy` |
{%- if is_fullstack %}
| Lint server side | `cargo clippy --no-default-features --features server` |
{%- endif %}
| Tests | `cargo nextest run --hide-progress-bar --failure-output final` |
| Run the app | `dx serve` (the user runs this; don't start it unless asked) |

- Never add `RUSTFLAGS`, `RUSTC_WRAPPER`, `CARGO_PROFILE_*`, `+toolchain` or `-Z` flags to a
  command. The compiler cache and toolchain come from the user's global config; build
  settings live in the `Cargo.toml` profiles. One-off overrides invalidate the cache and
  force rebuilds of everything.
- No cranelift. Dev profiles favour compile speed; runtime performance is not a goal.
- Fix clippy warnings in code you touch. The lint set is pedantic with a curated allow-list
  in `Cargo.toml`; prefer fixing over adding `#[allow]`.
- If cargo blocks on the build-directory lock, the user's bacon is building — just wait.

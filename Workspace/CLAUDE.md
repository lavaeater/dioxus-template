# {{project-name}}

A Dioxus 0.8 workspace generated from `lavaeater/dioxus-template`: shared components in
`packages/ui`, platform entry points in `packages/web`, `packages/desktop` and
`packages/mobile`{% if is_fullstack %}, server functions in `packages/api`{% endif %}.
Dioxus API guide (written for 0.7, still mostly accurate): @AGENTS.md

## Building, linting, testing

Run exactly these commands — they match the bacon jobs in `bacon.toml`, so builds share
artifacts instead of rebuilding each other. Feature sets matter: a different
`-p`/`--features` combination (e.g. `cargo check --workspace`) compiles a different
dependency graph.

| Purpose | Command |
|---|---|
{%- if is_fullstack %}
| Lint server side (default check) | `cargo clippy -p web --features server` |
{%- endif %}
| Lint web client | `cargo clippy -p web --features web` |
| Lint desktop | `cargo clippy -p desktop --features desktop` |
| Lint mobile | `cargo clippy -p mobile --features mobile` |
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

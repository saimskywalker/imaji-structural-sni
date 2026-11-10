# AGENTS.md

Rules for AI coding agents working in this repository. People should read
[CONTRIBUTING.md](CONTRIBUTING.md); everything here applies to them too. The
`PR rules` check enforces the rules marked (checked); a pull request that
breaks one cannot be merged.

## What imaji-structural-sni is

A Rust library of Indonesian structural design checks: steel members to
SNI 1729 (`src/steel/`), reinforced concrete flexure to SNI 2847
(`src/concrete/`), deflection (`src/deflection.rs`), LRFD load combinations to
SNI 1727 (`src/loads.rs`) and seismic base shear to SNI 1726
(`src/seismic.rs`). Every quantity is an SI newtype (`src/units.rs`); every
error is an `SniError` (`src/error.rs`).

Read before you touch:

- any check: "Things to be careful with" in CONTRIBUTING.md, and "Design
  principles" and "Scope and known limitations" in README.md.
- units: "Unit convention" in README.md and `src/units.rs`.
- tests: `tests/worked_examples.rs` (hand calculations written out in the
  comments) and `tests/boundary_cases.rs`. Unit tests sit in a
  `#[cfg(test)] mod tests` at the end of each module in `src/`.
- the profile catalogue: `src/steel/profiles.rs` and "Relationship to the SNI
  documents" in README.md.
- editions: issues #5 and #33.

## Commands

Use the stable Rust toolchain with rustfmt and clippy. Every one of these must
pass before you open or update a pull request. CI runs the same ones:

```bash
cargo fmt --all -- --check
cargo clippy --locked --all-targets -- -D warnings
cargo test --locked
cargo doc --no-deps
```

Run one test file while iterating: `cargo test --locked --test worked_examples`,
or one test by name: `cargo test --locked example1_plastic_moment`. The
examples print full reports:
`cargo run -p imaji-structural-sni --example steel_beam_check`.

## Stop and ask the human

Some things must come from the real world. An agent must never invent them.
Stop and ask the person you work for to provide them:

- **The clause and edition.** Every check names the SNI standard, edition and
  clause in its doc comment, read from the standard itself. Never write a
  clause number or edition from memory.
- **Coefficients and limits.** Resistance factors, slenderness limits, bounds
  and ratios come from the clause. Table values (site coefficients, `Sds`,
  `Sd1`, `R`, `Ie`, `Ct`, `x`, `Cu`, `kv` and similar) are caller inputs; never
  embed them. If a value cannot be confirmed, make it an input and say so in
  the pull request.
- **Worked examples.** A test for a new check reproduces a published example
  (the standard's commentary, or a well-known textbook for the equivalent ACI,
  AISC or ASCE provision) with document, edition and page in a comment, or a
  hand calculation written out step by step as in `tests/worked_examples.rs`.
  Never invent a source, a page number or an expected value.
- **Profile data.** New or changed rows in `src/steel/profiles.rs` come from a
  manufacturer catalogue the human names.
- **An accepted issue.** If the change has no issue labelled `accepted`, stop
  and ask the human to open one and wait for a maintainer.

## Code

- **English only** for code, identifiers, comments, commit messages and docs.
  Answer the person you work for in their language.
- Target Rust 1.75 (`rust-version` in `Cargo.toml`), edition 2021. CI builds on
  stable, so a newer feature passes CI and still breaks 1.75; do not use one.
- `src/lib.rs` denies `missing_docs` and `unsafe_code`, and doc examples
  compile with `deny(warnings)`. Every public item has a doc comment.
- No panics in library code: return `SniError`. `unwrap` and `expect` belong
  in tests, examples and doc examples only.
- Comments are sparse: only where the code is not self-evident. Do not add
  comments or doc comments to code you did not change.

## Tests (checked)

- Every change to `src/` comes with a test: a new `#[test]` in the module's
  `#[cfg(test)] mod tests` or a change under `tests/`. The check looks for an
  added `#[test]` line or a changed `tests/` file. A bug fix has a test that
  fails without the fix; check that it does before you open the pull request.
- A new check has a worked-example test with its source (see "Stop and ask
  the human").

## Never

- Never silence rustc, clippy or rustfmt (checked): no `#[allow(`,
  `#![allow(`, `#[expect(`, `#[rustfmt::skip]`, no lint set to `"allow"` in
  `Cargo.toml`, no `clippy.toml` or `rustfmt.toml`. Fix the finding instead.
- Never delete, skip (`#[ignore]`), loosen or rewrite an existing test or
  expected value to make CI pass. If a test looks wrong, say so in the issue.
- Never answer for a case outside the implemented clauses: return an
  `SniError` such as `UnsupportedSection`.
- Never rename or remove a public item (modules, functions, input and result
  structs and their fields, `SniError` variants, unit constructors and
  accessors such as `Force::kn` or `to_kn_m`). Callers depend on them.
- Never add a function that takes a bare `f64` for a physical quantity.
- Never copy text or tables out of an SNI standard; they are copyrighted by
  BSN.
- Never bump the version in `Cargo.toml`, publish, tag or release.
- Never add a dependency (the crate uses only the standard library) or change
  CI workflows unless the issue asks for it.
- Never add unrelated "improvements": no drive-by refactors, renames,
  reformatting of untouched code or dead code.
- Never open pull requests, issues or comments on your own. The human reviews
  your work and posts it.

## Pull requests (checked)

- First an issue, labelled `accepted` by a maintainer. The pull request says
  `Closes #N`.
- One issue per pull request, at most 400 changed lines.
- At most 2 open pull requests per person.
- Fill in the pull request template, including `### AI use` with the tool and
  model.
- The person submitting has read, run and understood every line, and writes
  the description in their own words. Pull requests that are raw, unreviewed
  agent output will be closed.

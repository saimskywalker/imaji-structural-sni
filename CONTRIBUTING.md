# Contributing

Contributions are welcome. A few things worth knowing before you spend time.

## How changes land

Every change reaches `main` through a pull request that the maintainer reviews
and merges. That includes changes from forks, which is the normal path: you
cannot push to this repository directly, and nothing merges without a review.
Fork the repository, branch from the latest `main`, and open the PR against
`main`. Keep one topic per PR. If a PR sits without a response, a nudge is
fine.

Commit messages follow the short conventional style already in the log:
`fix: ...`, `docs: ...`, `chore: ...`, with the PR number added on merge.

## Before opening a PR

```bash
cargo fmt --all -- --check
cargo clippy --all-targets -- -D warnings
cargo test
cargo doc --no-deps
```

CI runs exactly these four on `ubuntu-latest` with the stable toolchain.
Warnings are denied, and that includes `missing_docs`: a new public item
without a doc comment fails the build.

## Pull requests

Every CI check must pass before a pull request can be merged. The `PR rules`
check enforces these:

- **Issue first.** Open an issue and wait until a maintainer labels it
  `accepted`, then write `Closes #N` in the pull request.
- **Tests.** Every change to `src/` needs a test: a new `#[test]` or a
  change under `tests/`. A bug fix needs a test that fails without the fix.
- **Small.** At most 400 changed lines (`Cargo.lock` not counted), one issue
  per pull request.
- **No suppressions.** No `#[allow(`, `#[expect(` or `#[rustfmt::skip]`, no
  lint set to `"allow"` in `Cargo.toml`, no `clippy.toml` or `rustfmt.toml`.
  Fix the finding.
- **At most 2 open pull requests** per person.

If a rule really does not fit your change, say why in the description. A
maintainer can waive it with a label (`no-tests-needed`, `large-change` or
`suppression-ok`).

## Legal notice

By submitting a contribution you confirm that you wrote it, that you have the
right to submit it, and that it may be distributed under the project licence
(see [LICENSE-MIT](LICENSE-MIT)).

## AI-assisted contributions

Using AI tools is fine; please just be open about it.

- Say in the PR description which AI tools you used, if any: none, some or most, plus the tool or agent and the model (for example "Claude Code, Opus 5.5" or "Copilot, GPT-5").
- Please write the PR description in your own words; short and plain is fine. It shows us what you understood about the change. Please don't paste generated text.
- Read, run and test every line you submit, and be ready to explain it in review. If you don't understand what a line does, don't submit it.
- Keep the change focused. Agents like to add unrelated "improvements", long comments and dead code; remove them before you open the pull request.
- Pull requests that are raw, unreviewed agent output will be closed.
- If you use an agent, point it at [AGENTS.md](AGENTS.md).
- If the disclosure is missing, a check adds a reminder to the PR. It does not fail the PR, but a maintainer will ask about it before merging.

## What a good change looks like

A test built on a worked example with a stated source. Every check here
implements an SNI clause, and the test for it should reproduce a published
example, from the standard's own commentary or from a well-known textbook for
the equivalent ACI, AISC or ASCE provision, with a comment naming the
document, edition and page. A test that only checks the code against itself
does not show the formula is right.

For a bug fix, the test fails without the fix.

## Things to be careful with

**Cite the clause.** Every new check says in its doc comment which SNI
standard, edition and clause it implements. If you cannot confirm a
coefficient, do not guess one: leave it as an input and say so in the PR.

**Do not reproduce the standard.** SNI documents are copyrighted by BSN. The
crate implements equations and procedures and cites clause numbers. It does
not copy text and does not embed tables: a value an engineer reads from a
table (site coefficients, `R`, `Ie`, `kv` and similar) is an input.

**Refuse rather than guess.** A case outside the implemented clauses returns
`Err(SniError)`, for example `UnsupportedSection`, rather than an answer from
a clause that does not apply.

**No bare booleans.** A check returns the demand, the capacity, the ratio,
the governing limit state, and the intermediate values needed to repeat the
calculation by hand.

**Units.** Every quantity is a newtype and conversion happens only in named
constructors and accessors. Do not add a function that takes a bare `f64`
for a physical quantity.

**No panics.** `unwrap` and `expect` belong in tests and examples only.

## Scope

Member and load checks from SNI 1726, 1727, 1729 and 2847. Irrigation
hydraulics belongs in
[imaji-tirta](https://github.com/saimskywalker/imaji-tirta).
The crate has no dependencies beyond the standard library, and a PR that adds
one needs a strong reason.

## Stay in touch

Don't forget to follow [@saimskywalker](https://github.com/saimskywalker) on
GitHub to see new releases and other open-source projects as they come out.

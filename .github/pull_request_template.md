## What this changes

<!-- One or two sentences. What is different after this merges? -->

## Why

<!-- The problem it solves. Link the issue: Closes #<n> -->

## Checklist

- [ ] `cargo fmt --all -- --check` leaves no diff
- [ ] `cargo clippy --locked --all-targets -- -D warnings` is clean
- [ ] `cargo test --locked` passes
- [ ] `cargo doc --no-deps` builds, and new public items have doc comments
- [ ] Closes an issue labelled `accepted`; at most 400 changed lines; no `#[allow(` (checked)
- [ ] Changes to `src/` come with a test (checked)
- [ ] If this fixes a bug: there is a test that fails without the fix
- [ ] A new check cites its SNI clause and has a test built on a worked example with its source
- [ ] No text or table is copied out of an SNI standard
- [ ] README updated if this changes what the crate covers

### AI use

<!-- Using AI is welcome. Please tick one, naming the tool and model if you used AI; a reminder appears if it is missing. See CONTRIBUTING.md. -->

- [ ] None
- [ ] Some (tool or agent, and model: ...)
- [ ] Most (tool or agent, and model: ...)
- [ ] I have read and tested every line.

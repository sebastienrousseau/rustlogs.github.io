---
name: "rlg"
short_name: "rlg"
title: "Contributing to rlg | rlg"
description: "How to contribute to rlg: the development set-up, the checks every pull request passes, tests, signed commits, commit conventions and how contributions are licensed."
keywords: "contribute to rlg, rlg pull request, rlg contributing guide"
author: "Sebastien Rousseau"
date: "2026-10-26"
news_publication_date: "2026-10-26"
layout: "page"
language: "en-GB"
schema: "page"
changefreq: "monthly"
eyebrow: "Contributing"
headline: "Contributing to rlg"
lead: "A summary of the project's contributing guide; the guide in the repository is the reference."
---
## Setting up

```bash
git clone https://github.com/sebastienrousseau/rlg.git
cd rlg
cargo check --all-features
```

rlg targets Rust 1.88.0 and the 2024 edition. On macOS, run the integration
tests with `RLG_FALLBACK_STDOUT=1` to skip the `os_log` call.

## What every pull request passes

```bash
cargo fmt --check
cargo clippy --all-features --tests --benches -- -D warnings
cargo test --all-features
```

`make verify` runs everything CI runs on a pull request.

- A change in behaviour comes with tests in the same pull request, and a bug
  fix with a test that fails without it. Line coverage stays at 95% or more.
- Changes to the engine also run under Miri and Loom; untrusted-input
  parsers have fuzz targets; a new dependency must pass `cargo vet`.
- Production functions stay within the complexity ceilings: cyclomatic 10,
  cognitive 15, Halstead difficulty 30, 60 lines, and 500 lines per file.

## Signed commits and conventions

Every commit in a pull request must be cryptographically signed; unsigned
commits are rejected. Commit messages follow
[Conventional Commits](https://www.conventionalcommits.org/), with types such
as `feat`, `fix`, `perf`, `docs`, `test` and `ci`. Branch from `main`, for
example `feat/short-name`, keep one concern per commit, and explain why in the
pull request.

Security problems go through the private channel on the
[security page](/security/), never a public issue. The full guide, with the
Miri, Loom, Kani and fuzzing commands, is
[CONTRIBUTING.md](https://github.com/sebastienrousseau/rlg/blob/main/CONTRIBUTING.md).

## Licence of contributions

By contributing you agree that your work is dual-licensed under Apache-2.0 or
MIT, matching the project.

## Improving this website

The source of rustlogs.com is in the
[rustlogs.github.io repository](https://github.com/sebastienrousseau/rustlogs.github.io).
Report a mistake on the site there, or through the [contact page](/contact/).

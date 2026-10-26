---
name: "rlg"
short_name: "rlg"
title: "Security: reporting, memory safety, checks and verifying a release | rlg"
description: "How to report a vulnerability in rlg, where unsafe code is allowed, the checks that run on the engine and its inputs, the supply-chain controls, and how to verify a release."
keywords: "rlg security, rlg vulnerability report, Rust logging unsafe, rlg sbom, cosign verify rlg"
author: "Sebastien Rousseau"
date: "2026-10-26"
news_publication_date: "2026-10-26"
layout: "page"
language: "en-GB"
schema: "page"
changefreq: "monthly"
eyebrow: "Security"
headline: "Security model and how to check it"
lead: "What rlg guarantees, what it leaves to you, and the evidence for each claim."
nav_security: "true"
---
## How do I report a vulnerability?

Privately, through
[GitHub's advisory form](https://github.com/sebastienrousseau/rlg/security/advisories/new)
or by the email address in rlg's
[security policy](https://github.com/sebastienrousseau/rlg/security/policy).
Please do not open a public issue. Include a minimal reproducer, the affected
version and your assessment of the impact. The policy aims to acknowledge a
report within 5 business days. Only the latest published 0.0.x release
receives security fixes.

## Where is unsafe code allowed?

`unsafe_code` is denied in every crate of the workspace. The exceptions are
calls across a C boundary, each in a marked module with a safety comment:
`syslog(3)` for the macOS sink in `rlg`, and the thread and user ID calls in
`rlg-ebpf`.

## What limits does it put on resources?

- The ring buffer is bounded at 65,536 records; when it is full, the oldest
  are dropped and counted, so a burst cannot grow memory without limit.
- `rlg-otlp` caps a collector's response headers at 16 KiB, times out every
  request and refuses `https://` endpoints, since it has no TLS stack of its
  own.
- Records still in the buffer when the process is killed, before the
  `FlushGuard` is dropped, are lost.

## What is checked, and how often?

| Check | What it covers | When |
| :--- | :--- | :--- |
| Miri | Undefined behaviour in the engine and the `syslog` call | Every push to `main`, and pull requests touching the engine |
| Loom | The shutdown handshake and the session counter under every thread interleaving | Pull requests touching the engine |
| Kani | The level conversion and the session counter, model-checked | Merges to `main` and weekly |
| cargo-fuzz | Four targets: record parsing, format names, config loading, redaction | 30 seconds each on every pull request |
| proptest | Format output, the single-line NDJSON rule, filters | Every test run |
| cargo-semver-checks | Unannounced breaking changes | Pull requests touching the crates |

The test suite holds line coverage at 95% or more.

## How is the supply chain controlled?

- `cargo deny` checks licences, advisories and sources, over all features.
- `cargo vet` requires every dependency to be audited, imported from a
  trusted audit set, or exempted with a reason.
- Crates are published from CI through crates.io Trusted Publishing.
- Every release carries SPDX and CycloneDX bills of materials, signed
  keyless with Sigstore.

## Verifying a release

For release 0.0.15 and later, each bill of materials has a Sigstore bundle
beside it. With cosign 3 or later:

```bash
TAG=v0.0.15
BASE="https://github.com/sebastienrousseau/rlg/releases/download/${TAG}"
for f in sbom.spdx.json sbom.cyclonedx.json; do
  curl -sLO "${BASE}/${f}"
  curl -sLO "${BASE}/${f}.sigstore.json"
  cosign verify-blob \
    --bundle "${f}.sigstore.json" \
    --certificate-identity-regexp \
      'https://github.com/sebastienrousseau/rlg/.github/workflows/release.yml@refs/tags/v[0-9]+.*' \
    --certificate-oidc-issuer https://token.actions.githubusercontent.com \
    "$f"
done
```

Each file should print `Verified OK`. Releases up to 0.0.14 carry a `.sig`
and a `.crt` per file instead; the
[verification runbook](https://github.com/sebastienrousseau/rlg/blob/main/pkg/VERIFY.md)
covers both, and how to check a release tag's SSH signature against the
keys in `KEYS.asc`. The prebuilt binaries are listed with their SHA-256 in
each release's `SHA256SUMS` file.

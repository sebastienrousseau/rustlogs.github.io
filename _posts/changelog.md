---
name: "rlg"
short_name: "rlg"
title: "Changelog: rlg releases up to 0.0.15 | rlg"
description: "What changed in rlg's recent releases, from the smaller dependency tree in 0.0.13 to 0.0.15, with links to the full changelog."
keywords: "rlg changelog, rlg releases, rlg 0.0.15, rlg 0.0.14"
author: "Sebastien Rousseau"
date: "2026-10-26"
news_publication_date: "2026-10-26"
layout: "page"
language: "en-GB"
schema: "page"
changefreq: "monthly"
eyebrow: "Changelog"
headline: "rlg releases up to 26 October 2026"
lead: "The recent releases, summarised from the project's CHANGELOG.md, dated by their GitHub releases."
---
## 0.0.15, 11 October 2026

- Release bills of materials are signed with cosign 3, as one Sigstore bundle
  per file in place of separate `.sig` and `.crt` files.
- `make verify-release` now stops at the first bill of materials that fails
  verification.
- Dependency and CI updates.

## 0.0.14, 1 October 2026

- **Memory safety, macOS:** the `os_log` sink declared the variadic
  `syslog(3)` with a fixed argument list, so on Apple Silicon it could read a
  stale stack word as the message pointer. Present since 0.0.9; fixed.
- `fire()` costs about half as much per thread with eight threads logging at
  once, and no longer builds the `caller` attribute on the calling thread.
  **Breaking** for code that builds a `LogEvent` literal.
- **Breaking** (`rlg-otlp`): the blocking exporter uses the crate's own
  HTTP/1.1 client and no longer depends on `ureq`.
- The count of dropped events is now exact when the buffer is full.

## 0.0.13, 1 October 2026

- `rlg-mcp` moves to the official MCP SDK and serves stdio, streamable HTTP
  and HTTP+SSE.
- `rlg` and `rlg-report` print shell completions and manual pages, and
  `make install` installs them.
- `fire()` is about 40% cheaper on the calling thread, with byte-identical
  output.
- **Breaking** (`rlg-otlp`): it sends plain OTLP/HTTP to a local
  OpenTelemetry Collector and rejects `https://` endpoints; the `grpc`
  feature, a scaffold, is removed.
- 82 crates leave the dependency tree, including `reqwest`, `tonic` and
  `notify`; configuration hot reload now polls the file.
- `rlg` compiles on Windows again, and CI checks it there.

## 0.0.12, 26 August 2026

- Builds on targets without 64-bit atomics.
- Crates are published through crates.io Trusted Publishing.
- `rlg-mcp` gains its prompt, its resources and the `tail_logs_glob` tool.
- Patches RUSTSEC-2026-0204.

## Where is the full history?

Every change is in
[CHANGELOG.md](https://github.com/sebastienrousseau/rlg/blob/main/CHANGELOG.md),
and the releases are on
[GitHub](https://github.com/sebastienrousseau/rlg/releases) and
[crates.io](https://crates.io/crates/rlg/versions).

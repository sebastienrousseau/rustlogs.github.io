---
name: "rlg"
short_name: "rlg"
title: "About rlg and its author, Sebastien Rousseau | rlg"
description: "Who builds rlg, and the facts about the project: its crates, licence, minimum Rust version, latest release, versioning policy and where its source and security policy live."
keywords: "rlg author, sebastien rousseau, RustLogs, rlg licence, rlg versions"
author: "Sebastien Rousseau"
date: "2026-10-26"
news_publication_date: "2026-10-26"
layout: "page"
language: "en-GB"
schema: "page"
changefreq: "monthly"
eyebrow: "About"
headline: "About rlg"
lead: "rlg is an open-source project by Sebastien Rousseau, published as RustLogs."
---
## Who builds rlg?

rlg is written and maintained by
[Sebastien Rousseau](https://sebastienrousseau.com/), on
[GitHub](https://github.com/sebastienrousseau) as sebastienrousseau. The
project is also known as RustLogs, the name of this site. Version 0.0.1 was
published on crates.io in February 2023.

## Project facts

| | |
| :--- | :--- |
| **Crates** | `rlg` and nine satellites, all on crates.io |
| **Language** | Rust, 2024 edition |
| **Minimum Rust version** | 1.88.0 |
| **Licence** | Apache-2.0 or MIT, at your option |
| **Latest release** | [0.0.15](https://crates.io/crates/rlg/0.0.15), 11 October 2026 |
| **Source** | [github.com/sebastienrousseau/rlg](https://github.com/sebastienrousseau/rlg) |
| **User manual** | [doc.rustlogs.com/manual](https://doc.rustlogs.com/manual/) |
| **API reference** | [docs.rs/rlg](https://docs.rs/rlg) |
| **Security policy** | [SECURITY.md](https://github.com/sebastienrousseau/rlg/security/policy) |

## How is it versioned?

rlg follows Semantic Versioning and is at 0.0.x, so under Cargo's rules every
release may be breaking. Breaking changes are marked **Breaking** in the
CHANGELOG, and that includes a change to the bytes a format produces for the
same record. All ten crates are released together at the same version. A rise
in the minimum Rust version gets its own CHANGELOG entry with the reason.

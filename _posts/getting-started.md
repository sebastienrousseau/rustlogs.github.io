---
name: "rlg"
short_name: "rlg"
title: "Getting started with rlg: add the crate and write a first record | rlg"
description: "Add rlg to a Rust project, initialise it, fire a structured record, choose a format and a level, and see where the record goes on macOS, Linux and other systems."
keywords: "rlg getting started, cargo add rlg, Rust structured logging example, rlg init"
author: "Sebastien Rousseau"
date: "2026-10-26"
news_publication_date: "2026-10-26"
layout: "page"
language: "en-GB"
schema: "page"
changefreq: "monthly"
eyebrow: "Getting started"
headline: "Your first structured record"
lead: "From an empty Rust project to a JSON record in the system log, in one file."
nav_docs: "true"
---
## What do I need?

Rust 1.88.0 or newer: rlg's declared minimum supported Rust version, which CI
builds every library and binary on. rlg uses the 2024 edition. The native
sinks need macOS or Linux; elsewhere, including Windows, rlg writes to
standard output.

## How do I add it?

```bash
cargo add rlg
```

or in `Cargo.toml`:

```toml
[dependencies]
rlg = "0.0.15"
```

No Cargo features are on by default.

## How do I write a record?

```rust
use rlg::log::Log;
use rlg::log_format::LogFormat;

fn main() {
    // Keep the guard for the life of the program: dropping it flushes
    // pending records and stops the background thread.
    let _guard = rlg::init().unwrap();

    Log::info("User authenticated")
        .component("auth-service")
        .with("user_id", 42)
        .format(LogFormat::JSON)
        .fire();
}
```

`Log::info` starts a record; `Log::trace`, `debug`, `verbose`, `warn`,
`error`, `fatal` and `critical` start one at another level. `.with` adds an
attribute of any type that serialises to JSON, and `.fire()` hands the record
to the background thread.

## Where does the record go?

To the platform's log. On macOS, read it with `log show --last 1m`; on Linux,
with `journalctl -t rlg`. To see records on standard output while you work,
set `RLG_FALLBACK_STDOUT=1`:

```bash
RLG_FALLBACK_STDOUT=1 cargo run
```

```json
{"Attributes":{"caller":"src/main.rs:13","user_id":42},"Component":"auth-service","Description":"User authenticated","Format":"JSON","Level":"INFO","SessionID":1,"Timestamp":"2026-10-26T14:48:05.812932000Z"}
```

The `caller` attribute records the file and line that called `fire()`.

## How do I set the level and the default format?

Use the builder instead of `rlg::init()`:

```rust
use rlg::log_format::LogFormat;
use rlg::log_level::LogLevel;

let _guard = rlg::builder()
    .level(LogLevel::WARN)
    .format(LogFormat::Logfmt)
    .init()
    .expect("rlg is already initialised");
```

The builder's format applies to records from the `log` facade; without
one, they are logfmt on a terminal and JSON in a pipe or a file. A record you build yourself keeps the format you give it, and is
written as `MCP` if you give none. `RUST_LOG=debug` sets the level without a
code change.

## Do my log and tracing calls still work?

Yes. `rlg::init()` installs rlg behind the `log` facade and as the `tracing`
subscriber, so existing `log::warn!` and `tracing::info!` calls are written
through rlg too.

## Where next?

- The [API guide](/documentation/) for levels, formats, configuration and
  Cargo features.
- The [migration guide](/migration/) if you are coming from `log`, `slog` or
  `tracing`.
- The [user manual](https://doc.rustlogs.com/manual/) and the API reference
  on [docs.rs](https://docs.rs/rlg).

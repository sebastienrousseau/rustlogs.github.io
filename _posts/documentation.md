---
name: "rlg"
short_name: "rlg"
title: "API guide: records, levels, formats, configuration and Cargo features | rlg"
description: "A guide to rlg's API: building and firing a record, the levels, the 14 formats, the builder and environment variables, TOML configuration, Cargo features and the crates in the workspace."
keywords: "rlg api, rlg Log, rlg LogLevel, rlg LogFormat, rlg config toml, rlg cargo features"
author: "Sebastien Rousseau"
date: "2026-10-26"
news_publication_date: "2026-10-26"
layout: "page"
language: "en-GB"
schema: "page"
changefreq: "monthly"
eyebrow: "API guide"
headline: "How the rlg API fits together"
lead: "The types and options you choose between, with the full reference on docs.rs."
nav_docs: "true"
---
## Building a record

A record is a `rlg::log::Log`. Start one with a level constructor, add to it,
and fire it:

| Call | What it does |
| :--- | :--- |
| `Log::info(msg)`, `Log::warn(msg)`, ... | Starts a record at that level with a message |
| `.component(name)` | Names the part of the program it came from |
| `.with(key, value)` | Adds an attribute; the value can be any `Serialize` type |
| `.format(LogFormat::...)` | Chooses how this record is written |
| `.fire()` | Checks the level and pushes the record into the ring buffer |

## Levels

The severities, from lowest to highest, are `TRACE`, `DEBUG`, `VERBOSE`,
`INFO`, `WARN`, `ERROR`, `FATAL` and `CRITICAL`. `ALL`, `NONE` and `DISABLED`
complete `LogLevel` as filter settings. A record below the configured level is
dropped in `fire()`, before it reaches the buffer.

## Formats

`LogFormat` has 14 variants: `JSON`, `NDJSON`, `ECS`, `GELF`, `Logstash`,
`OTLP`, `MCP`, `Logfmt`, `CLF`, `CEF`, `ELF`, `W3C`, `ApacheAccessLog` and
`Log4jXML`. The [features page](/features/#fourteen-output-formats) groups
them by use.

## Initialisation

| Call | Effect |
| :--- | :--- |
| `rlg::init()` | Starts the engine with the defaults and returns a `FlushGuard` |
| `rlg::builder().level(..)` | Sets the minimum level |
| `.format(..)` | Sets the format of records from the `log` facade |
| `.without_log()` | Leaves the `log` facade alone |
| `.without_tracing()` | Leaves the global `tracing` subscriber alone |
| `.init()` | Starts the engine; fails if rlg or another logger is already installed |

## Environment variables

| Variable | Effect |
| :--- | :--- |
| `RUST_LOG` | Sets the level, as `RUST_LOG=debug` or `RUST_LOG=my_crate=debug` |
| `RLG_ENV=production` | Makes JSON the format of records from the `log` facade |
| `RLG_FALLBACK_STDOUT` | Writes to standard output instead of the native sink |
| `RLG_TUI=1` | Starts a live terminal dashboard (with the `tui` feature) |

## Configuration file

`rlg::config::Config` reads TOML: `version`, `profile`, `log_file_path`,
`log_level`, `log_format`, `logging_destinations` (`File`, `Stdout` or
`Network`), `log_rotation` and `env_vars`. `Config::load(Some("rlg.toml"))`
loads and validates it; with the `tokio` feature, `Config::load_async` and
`Config::hot_reload_async` load it asynchronously and follow edits. In
0.0.15 the engine still writes to the native sink whatever the destinations
say; `rlg::engine::ENGINE.apply_config(&config)` applies the level. The
[crate README](https://github.com/sebastienrousseau/rlg/blob/main/crates/rlg/README.md#configuration)
lists every field.

## Cargo features

Everything optional is off by default.

| Feature | Crate | Adds |
| :--- | :--- | :--- |
| `tokio` | `rlg` | `Config::load_async` and polling hot reload |
| `tui` | `rlg` | A live terminal dashboard, started with `RLG_TUI=1` |
| `tracing-layer` | `rlg` | `RlgLayer`, a `tracing_subscriber::Layer` |
| `fast-queue` | `rlg` | An eight-shard ring buffer for many concurrent producers |
| `uring` | `rlg` | The `io_uring` file sink on Linux, a scaffold |
| `debug_enabled` | `rlg` | Verbose diagnostics from the engine itself |
| `async` | `rlg-otlp` | `AsyncOtlpExporter` on Tokio |

## Crates in the workspace

All ten are on crates.io and released together at the same version.

| Crate | What it is |
| :--- | :--- |
| [`rlg`](https://crates.io/crates/rlg) | The engine: ring buffer, background thread, 14 formats, native sinks |
| [`rlg-cli`](https://crates.io/crates/rlg-cli) | The `rlg` binary: filter and convert log files |
| [`rlg-report`](https://crates.io/crates/rlg-report) | The `rlg-report` binary and library: log summaries |
| [`rlg-mcp`](https://crates.io/crates/rlg-mcp) | An MCP server over log files |
| [`rlg-otlp`](https://crates.io/crates/rlg-otlp) | OTLP/HTTP export to an OpenTelemetry Collector |
| [`rlg-redact`](https://crates.io/crates/rlg-redact) | Redaction of secrets and personal data |
| [`rlg-tower`](https://crates.io/crates/rlg-tower) | Access logs for `tower` services |
| [`rlg-test`](https://crates.io/crates/rlg-test) | Capturing and asserting on records in tests |
| [`rlg-wasm`](https://crates.io/crates/rlg-wasm) | WebAssembly bindings |
| [`rlg-ebpf`](https://crates.io/crates/rlg-ebpf) | Process enrichment; the eBPF enricher is a scaffold |

## Where is the full reference?

The rendered API reference is on [docs.rs/rlg](https://docs.rs/rlg), one page
per crate. The [user manual](https://doc.rustlogs.com/manual/) has the
tutorials, how-to guides, the architecture and the design decisions.

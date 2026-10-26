---
name: "rlg"
short_name: "rlg"
title: "Features: off-thread logging, 14 formats, native sinks and satellites | rlg"
description: "What rlg does: a ring buffer on the calling thread, 14 output formats, os_log and journald sinks, log and tracing bridges, redaction, OpenTelemetry export, an MCP server, command-line tools and WebAssembly."
keywords: "rlg features, Rust ring buffer logging, log formats, journald, os_log, rlg-redact, rlg-otlp, rlg-mcp, rlg-cli"
author: "Sebastien Rousseau"
date: "2026-10-26"
news_publication_date: "2026-10-26"
layout: "page"
language: "en-GB"
schema: "page"
changefreq: "monthly"
eyebrow: "Features"
headline: "What rlg does, and where it stops"
lead: "Each section says what the feature is, which crate provides it and what it does not do yet."
nav_features: "true"
---
## Logging off the calling thread

**`fire()` does three things: it checks the level, records where it was
called from, and pushes the record into a ring buffer.** The buffer holds
65,536 records. A background thread drains it in batches of up to 64, formats
each record and writes it to the sink, so your thread never waits on
formatting or I/O.

```rust
use rlg::log::Log;
use rlg::log_format::LogFormat;

let _guard = rlg::init().unwrap();

Log::info("User authenticated")
    .component("auth-service")
    .with("user_id", 42)
    .format(LogFormat::JSON)
    .fire();
```

`rlg::init()` returns a `FlushGuard`. Keep it for the life of the program:
dropping it writes the records still in the buffer and stops the background
thread. When the buffer is full, the oldest records are dropped to make room
and counted. With the `fast-queue` feature, the buffer is split into eight
shards to reduce contention between many producing threads.

## Fourteen output formats

**Each record carries its format, so one program can write several.**

| Family | Formats |
| :--- | :--- |
| JSON | `JSON`, `NDJSON`, `Logstash`, `ECS` (Elastic Common Schema), `GELF` |
| Telemetry and agents | `OTLP`, `MCP` (JSON-RPC notifications) |
| Text | `Logfmt`, `CLF`, `ApacheAccessLog`, `W3C`, `ELF` |
| Security and enterprise | `CEF`, `Log4jXML` |

A record built with `Log::info(...)` and its siblings is written as `MCP`
unless you call `.format(...)`. Records from the `log` facade use the format
`rlg::init()` picks: logfmt when standard output is a terminal, JSON when it
is a pipe or a file, and JSON whenever `RLG_ENV=production` is set. `tracing`
events are written as `MCP`, or in the format given to `RlgLayer::with_format`
when you use the layer.

## Native sinks

**rlg writes to the platform's own log where there is one.**

| Platform | Sink |
| :--- | :--- |
| macOS | `os_log`, through `syslog(3)` |
| Linux | `journald`, through its socket at `/run/systemd/journal/socket` |
| Linux without a running journald | Standard output |
| Everywhere else, including Windows | Standard output |

Setting `RLG_FALLBACK_STDOUT=1` sends records to standard output instead,
and rlg does the same on GitHub Actions. An `io_uring` file sink for Linux is
in the crate behind the `uring` feature, as a scaffold. There is no native
Windows Event Log sink.

## Bridges for log and tracing

**`rlg::init()` registers rlg as the backend for the `log` facade and as the
global `tracing` subscriber**, so `log::info!` and `tracing::info!` calls in
your code and its dependencies go through the same ring buffer. The builder
turns either bridge off and sets a level and a default format:

```rust
use rlg::log_format::LogFormat;
use rlg::log_level::LogLevel;

let _guard = rlg::builder()
    .level(LogLevel::WARN)
    .format(LogFormat::Logfmt)
    .init()
    .expect("rlg is already initialised");
```

`RUST_LOG` sets the level too: rlg reads `RUST_LOG=debug` and
`RUST_LOG=my_crate=debug`, and keeps the most permissive level it finds. With
the `tracing-layer` feature, `RlgLayer` adds rlg to an existing
`tracing_subscriber` stack instead.

## Configuration and hot reload

**`Config` loads a TOML file, validates it and creates the log paths it
needs.**

```toml
version              = "1.0"
profile              = "production"
log_file_path        = "/var/log/rlg.log"
log_level            = "INFO"
logging_destinations = [
    { type = "File", value = "/var/log/rlg.log" },
    { type = "Stdout" },
]
log_rotation         = { Size = 10485760 }  # 10 MiB
```

Load it with `Config::load(Some("rlg.toml"))`. With the `tokio` feature,
`Config::load_async` loads it without blocking and `Config::hot_reload_async`
polls the file every 250 ms and reloads it, including after an editor's save
that renames a new file over the old one.

In 0.0.15 the engine does not read its destination from the configuration:
records go to the native sink described above, whatever
`logging_destinations` and `log_file_path` say. The configured level applies
when you pass the loaded configuration to `rlg::engine::ENGINE.apply_config`.

## Redacting secrets and personal data

**[rlg-redact](https://crates.io/crates/rlg-redact) scrubs a record before it
is written.** `Redactor::with_defaults()` loads six patterns: card numbers,
JWTs, `Authorization: Bearer` tokens, email addresses, IPv4 addresses and AWS
access key IDs.

```rust
use rlg::log::Log;
use rlg_redact::Redactor;

let redactor = Redactor::with_defaults();
let record = Log::info("card 4111-1111-1111-1111 declined")
    .with("email", "user@example.com")
    .with("client_ip", "10.0.1.42");

redactor.scrub(record).fire();
// description "card [REDACTED] declined", email and client_ip "[REDACTED]"
```

It scrubs the message and every string attribute, including strings nested in
JSON arrays and objects. Numbers and booleans pass through unchanged. Add your
own patterns with `.with_pattern(...)` and change the marker with
`.marker(...)`.

## Export to OpenTelemetry

**[rlg-otlp](https://crates.io/crates/rlg-otlp) sends records over OTLP/HTTP,
JSON-encoded, to an OpenTelemetry Collector on the same host or in the same
pod.** The Collector then handles TLS, credentials, batching and retries
towards Honeycomb, Datadog, Grafana or any other backend.

```rust
use rlg_otlp::OtlpExporter;

// Defaults to http://localhost:4318/v1/logs
let exporter = OtlpExporter::builder().timeout_secs(10).build();
exporter.export_one(&record)?;
```

The crate carries no TLS stack: it rejects an `https://` endpoint rather than
send records in the clear, caps a collector's response headers at 16 KiB and
times out every request. The `async` feature adds `AsyncOtlpExporter` on Tokio.

## Logs for AI agents over MCP

**[rlg-mcp](https://crates.io/crates/rlg-mcp) is a Model Context Protocol
server that gives an AI agent read access to log files.** It offers four
tools:

| Tool | Returns |
| :--- | :--- |
| `tail_log` | The last records of a file |
| `tail_logs_glob` | The last records across every file matching a glob |
| `filter_log` | Records at or above a level, or from one component, in any format |
| `summarize_errors` | A count of ERROR-and-above records per component |

It also offers a `triage_error_spike` prompt, the level ladder as a resource
(`rlg://log-levels`) and a resource template for tailing a file. It serves
stdio by default, or streamable HTTP and the older HTTP+SSE transport with
`--transport`. Install it with `cargo install rlg-mcp`, or run the container
image `ghcr.io/sebastienrousseau/rlg-mcp`; it is listed in the MCP registry as
`io.github.sebastienrousseau/rlg-mcp`. Point your MCP client at the `rlg-mcp`
command:

```json
{ "mcpServers": { "rlg": { "command": "rlg-mcp" } } }
```

## Command-line tools

**Two binaries work on log files from the shell.** `rlg` (from the
`rlg-cli` crate) filters records and converts them to any of the 14 formats;
`rlg-report` summarises a file.

```bash
rlg --min-level error --format ecs app.log   # errors and worse, as ECS
rlg --component db --attr user_id=42 app.log # one component, one attribute
rlg-report app.log                           # counts, top messages, latency
```

`rlg-report` counts records by level and component, ranks the most frequent
messages and, when records carry a `latency_ms` or `http.latency_ms`
attribute, prints p50, p95, p99 and maximum latency. `--format json` prints
the same digest as JSON.

Both read one record per line in the JSON shape of rlg's `Log` type: the
fields `session_id`, `time`, `level`, `component`, `description`, `format`
and `attributes`. In 0.0.15 that is not the shape rlg's `JSON` and `NDJSON`
formats write, so they cannot read those formats' output back yet.

Both binaries print their own manual page (`--manpage`) and shell
completions (`--completions`), and every GitHub release has them prebuilt,
with `rlg-mcp`, for macOS, Linux and Windows.

## Access logs for tower services

**[rlg-tower](https://crates.io/crates/rlg-tower) is a `tower::Layer` that
writes one record per response** for axum, hyper and other `tower` services,
with the method, path and status in the message, and can copy a request
header such as `traceparent` into the record.

## Testing log output

**[rlg-test](https://crates.io/crates/rlg-test) captures records in a test**
and asserts on their level, message, component, attributes and count:

```rust
use rlg::log::Log;
use rlg::log_level::LogLevel;
use rlg_test::{assert_logged, capture, LogExt};

let capture = capture();
Log::info("user authenticated")
    .component("auth")
    .with("user_id", 42_u64)
    .log_to(&capture);

assert_logged!(capture, level == LogLevel::INFO);
assert_logged!(capture, attribute "user_id" => 42_u64);
```

## WebAssembly

**[rlg-wasm](https://crates.io/crates/rlg-wasm) wraps rlg with
`wasm-bindgen`** for browsers, Deno, Bun and Cloudflare Workers. Build it with
`wasm-pack build crates/rlg-wasm --target web --release`. A WASI 0.2
component interface is in the crate as a scaffold. It is published on
crates.io; it is not on npm.

## Process enrichment

**[rlg-ebpf](https://crates.io/crates/rlg-ebpf) adds host context to a
record.** `ProcessEnricher` attaches the process ID, and on Unix the thread
and user IDs. The eBPF enricher that would add kernel context on Linux is a
scaffold.

## Signed releases

**Each release is built and published by CI.** The crates go to crates.io
through Trusted Publishing; the GitHub release carries prebuilt binaries for
five targets with a `SHA256SUMS` file, and SPDX and CycloneDX bills of
materials, each signed keyless with Sigstore. Release tags are signed with
the maintainer's SSH key. The [security page](/security/#verifying-a-release)
shows how to check them.

---
name: "rlg"
short_name: "rlg"
title: "rlg: near-lock-free structured logging for Rust"
description: "rlg (RustLogs) is a structured logging library for Rust. fire() pushes a record into a 65,536-slot ring buffer and returns; a background thread formats it in one of 14 formats and writes it to os_log on macOS, journald on Linux or standard output."
keywords: "rlg, RustLogs, Rust logging, structured logging, ring buffer, journald, os_log, OTLP, MCP, log redaction"
author: "Sebastien Rousseau"
date: "2026-10-26"
news_publication_date: "2026-10-26"
layout: "index"
language: "en-GB"
schema: "page"
changefreq: "weekly"
eyebrow: "Open-source Rust library"
headline: "Structured logging for Rust that keeps formatting off your thread"
lead: "rlg pushes each record into a ring buffer and returns. A background thread formats it in one of 14 formats and writes it to os_log on macOS, journald on Linux or standard output."
---
## What is rlg?

rlg, the RustLogs project, is a free, open-source structured logging library
for Rust. You build a record with a level, a message, a component and typed
attributes, then call `fire()`. `fire()` checks the level and pushes the
record into a 65,536-slot ring buffer; a background thread formats it and
writes it to the platform's log: `os_log` on macOS, `journald` on Linux, and
standard output elsewhere. It is dual-licensed under Apache-2.0 or MIT.

## What else is in the box?

Nine satellite crates, released at the same version as the engine: command-line
tools to filter, convert and summarise logs, an MCP server that gives AI
agents access to log files, an OpenTelemetry exporter, a redactor for secrets
and personal data, access logs for `tower` services, test helpers and
WebAssembly bindings. The [features page](/features/) covers each one.

## When is it the wrong choice?

When you log a few records a second, a background thread and a large buffer
buy nothing a synchronous logger does not already give you. When spans are
what you analyse, use `tracing`. When every record must survive a crash,
remember that records wait in memory until the background thread writes them.
The [FAQ](/faqs/) lists the other limits.

## Who makes it?

rlg is written by [Sebastien Rousseau](https://sebastienrousseau.com/). The
source, the issue tracker and every release are on
[GitHub](https://github.com/sebastienrousseau/rlg); the crates are on
[crates.io](https://crates.io/crates/rlg). Read more on the
[about page](/about/).

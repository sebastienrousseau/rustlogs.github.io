---
name: "rlg"
short_name: "rlg"
title: "Migrate to rlg from log, slog or tracing | rlg"
description: "How to move a Rust program to rlg from the log facade, slog or tracing: what to change at start-up, how levels map, and when to keep tracing and bridge it instead."
keywords: "rlg migration, migrate from log, migrate from tracing, migrate from slog, env_logger alternative"
author: "Sebastien Rousseau"
date: "2026-10-26"
news_publication_date: "2026-10-26"
layout: "page"
language: "en-GB"
schema: "page"
changefreq: "monthly"
eyebrow: "Migration"
headline: "Moving a Rust program to rlg"
lead: "What changes when you switch from log, slog or tracing, and when not to switch."
nav_migration: "true"
---
## From log and env_logger

**You can keep every call site.** Replace the logger set-up and rlg becomes
the backend behind the `log` facade:

```diff
-env_logger::init();
+let _guard = rlg::init().unwrap();
```

Existing `log::info!` calls are then written through rlg. Their message text
is kept as it is; to get typed attributes instead of values formatted into
the message, rewrite a call to the fluent API:

```diff
-info!("user_id={user_id} authenticated");
+rlg::log::Log::info("authenticated").with("user_id", user_id).fire();
```

## From tracing

**Events map one to one; spans do not.** rlg records flat events with
attributes:

```diff
-tracing::info!(user_id = 42, region = "eu-west-1", "authenticated");
+rlg::log::Log::info("authenticated")
+    .with("user_id", 42_u64)
+    .with("region", "eu-west-1")
+    .fire();
```

If you rely on spans, `#[instrument]` or propagating span context between
services, keep `tracing`. `rlg::init()` installs rlg as the `tracing`
subscriber, and with the `tracing-layer` feature `RlgLayer` adds rlg to an
existing subscriber stack, so tracing's events still reach rlg's formats,
sinks and redaction.

## From slog

**There is no inherited context.** slog's `o!(...)` key-value context has no
direct equivalent: each rlg record is self-contained. Wrap the shared part in
a function:

```rust
use rlg::log::Log;

fn service_log(msg: &str) -> Log {
    Log::info(msg).component("api")
}

service_log("authenticated").with("user_id", user_id).fire();
```

`slog-async`'s channel is not needed: rlg's engine already moves records off
the calling thread.

## Level mapping

| log, tracing, slog | rlg |
| :--- | :--- |
| `trace!` | `Log::trace` |
| `debug!` | `Log::debug` |
| `info!` | `Log::info` |
| `warn!` | `Log::warn` |
| `error!` | `Log::error` |
| slog's `crit!` | `Log::critical` |

rlg adds `Log::verbose` and `Log::fatal`.

## From an older rlg

rlg is at 0.0.x, and breaking changes are marked **Breaking** in its
[CHANGELOG](https://github.com/sebastienrousseau/rlg/blob/main/CHANGELOG.md).
Every crate in the workspace is released at the same version, so upgrade
them together. Code written for 0.0.3, the version the old version of this
site described, used `Log::new(...)` with positional arguments and an async
`log()` call; today's API is the builder shown on the
[getting started page](/getting-started/).

The full guides are in the
[user manual](https://github.com/sebastienrousseau/rlg/tree/main/docs/migration).

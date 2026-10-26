---
name: "rlg"
short_name: "rlg"
title: "Frequently asked questions about rlg | rlg"
description: "Answers about rlg: what it is, its cost against tracing, keeping log and tracing calls, where records go, crashes, Windows, spans, AI agents, API stability and the licence."
keywords: "rlg faq, rlg vs tracing, rlg windows, rlg spans, rlg crash, rlg licence"
author: "Sebastien Rousseau"
date: "2026-10-26"
news_publication_date: "2026-10-26"
layout: "page"
language: "en-GB"
schema: "page"
changefreq: "monthly"
eyebrow: "FAQ"
headline: "Frequently asked questions"
lead: "Short answers first; each links to the page with the detail where there is one."
nav_faqs: "true"
---
## What is rlg?

rlg, the RustLogs project, is a free, open-source structured logging library for Rust. `fire()` pushes a record into a ring buffer and returns; a background thread formats it in one of 14 formats and writes it to os_log on macOS, journald on Linux or standard output. See the [features](/features/).

## Is it faster than tracing?

On the calling thread it costs about the same: 598 ns against 591 ns for one record in a published CI run, and 947 ns against 1,117 ns with three attributes. The difference is that rlg has not formatted the record or written it yet, so your thread never waits on I/O. The [performance page](/benchmarks/) has the full table.

## Can I keep my log and tracing calls?

Yes. `rlg::init()` installs rlg behind the log facade and as the tracing subscriber, so existing `log::info!` and `tracing::info!` calls are written through rlg. See the [migration guide](/migration/).

## Where do my records go?

To os_log on macOS and journald on Linux, and to standard output elsewhere, including Windows, or on Linux when journald is not running. Set `RLG_FALLBACK_STDOUT=1` to send them to standard output. See [native sinks](/features/#native-sinks).

## Can a crash lose records?

Yes. Records wait in memory until the background thread writes them, so records still in the buffer when the process is killed are lost. Dropping the `FlushGuard` returned by `rlg::init()` writes them out.

## Does it work on Windows?

rlg builds on Windows, which CI checks, and writes to standard output there. There is no native Windows Event Log sink. Prebuilt rlg, rlg-report and `rlg-mcp` binaries are published for Windows.

## Does it support spans?

No. rlg records events with attributes. If you analyse hierarchical spans, keep tracing; rlg's tracing support bridges events, not span context.

## Can an AI agent read my logs?

Yes, through the `rlg-mcp` server, which gives MCP clients tools to tail, filter and summarise log files, over stdio or HTTP. See [logs for AI agents](/features/#logs-for-ai-agents-over-mcp).

## Is the API stable?

No. rlg is at 0.0.x, so any release may break your build, and it has one maintainer. Breaking changes are marked in the CHANGELOG, and cargo-semver-checks runs on pull requests that change the crates.

## What does it cost, and under which licence?

Nothing. rlg is free software under the Apache License 2.0 or the MIT licence, at your option.

<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {
      "@type": "Question",
      "name": "What is rlg?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "rlg, the RustLogs project, is a free, open-source structured logging library for Rust. fire() pushes a record into a ring buffer and returns; a background thread formats it in one of 14 formats and writes it to os_log on macOS, journald on Linux or standard output."
      }
    },
    {
      "@type": "Question",
      "name": "Is it faster than tracing?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "On the calling thread it costs about the same: 598 ns against 591 ns for one record in a published CI run, and 947 ns against 1,117 ns with three attributes. The difference is that rlg has not formatted the record or written it yet, so your thread never waits on I/O."
      }
    },
    {
      "@type": "Question",
      "name": "Can I keep my log and tracing calls?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Yes. rlg::init() installs rlg behind the log facade and as the tracing subscriber, so existing log::info! and tracing::info! calls are written through rlg."
      }
    },
    {
      "@type": "Question",
      "name": "Where do my records go?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "To os_log on macOS and journald on Linux, and to standard output elsewhere, including Windows, or on Linux when journald is not running. Set RLG_FALLBACK_STDOUT=1 to send them to standard output."
      }
    },
    {
      "@type": "Question",
      "name": "Can a crash lose records?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Yes. Records wait in memory until the background thread writes them, so records still in the buffer when the process is killed are lost. Dropping the FlushGuard returned by rlg::init() writes them out."
      }
    },
    {
      "@type": "Question",
      "name": "Does it work on Windows?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "rlg builds on Windows, which CI checks, and writes to standard output there. There is no native Windows Event Log sink. Prebuilt rlg, rlg-report and rlg-mcp binaries are published for Windows."
      }
    },
    {
      "@type": "Question",
      "name": "Does it support spans?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "No. rlg records events with attributes. If you analyse hierarchical spans, keep tracing; rlg's tracing support bridges events, not span context."
      }
    },
    {
      "@type": "Question",
      "name": "Can an AI agent read my logs?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Yes, through the rlg-mcp server, which gives MCP clients tools to tail, filter and summarise log files, over stdio or HTTP."
      }
    },
    {
      "@type": "Question",
      "name": "Is the API stable?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "No. rlg is at 0.0.x, so any release may break your build, and it has one maintainer. Breaking changes are marked in the CHANGELOG, and cargo-semver-checks runs on pull requests that change the crates."
      }
    },
    {
      "@type": "Question",
      "name": "What does it cost, and under which licence?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Nothing. rlg is free software under the Apache License 2.0 or the MIT licence, at your option."
      }
    }
  ]
}
</script>

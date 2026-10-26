---
name: "rlg"
short_name: "rlg"
title: "Performance: what fire() costs on the calling thread | rlg"
description: "Measured cost of rlg's fire() against tracing and log on a GitHub-hosted runner, what each contender does in the timed path, the contention figures, and how to reproduce them."
keywords: "rlg benchmark, Rust logging performance, rlg vs tracing, fire() latency"
author: "Sebastien Rousseau"
date: "2026-10-26"
news_publication_date: "2026-10-26"
layout: "page"
language: "en-GB"
schema: "page"
changefreq: "monthly"
eyebrow: "Performance"
headline: "What a record costs your thread"
lead: "Published measurements from the rlg repository, with the machine they came from and what they do not show."
nav_docs: "true"
---
## What is measured?

The `competitive_bench` suite times the cost to the **calling thread** of
emitting one record. The three contenders do different work:

- **rlg `fire()`** checks the level and pushes the record into the ring
  buffer. Formatting and I/O happen later on the background thread and are not
  in the timed path.
- **`tracing::info!`** goes through a `tracing_subscriber::fmt` subscriber that
  formats the event on the calling thread and writes it to a discarding
  writer.
- **`log::info!`** goes to a logger that does nothing: the cost of the facade
  alone.

## The numbers

From the repository's
[docs/BENCHMARKS.md](https://github.com/sebastienrousseau/rlg/blob/main/docs/BENCHMARKS.md):
one run on a GitHub-hosted `ubuntu-latest` runner, stable Rust, release
profile, on the 0.0.13 release branch
([run 36802323103](https://github.com/sebastienrousseau/rlg/actions/runs/36802323103)).
Typical time per iteration, with Criterion's 95% confidence interval.

| Scenario | rlg `fire()` | `tracing::info!` | `log::info!` (no-op) |
| :--- | ---: | ---: | ---: |
| One record | 598 ns (582 to 615) | 591 ns (589 to 593) | 2.7 ns |
| One record, 3 attributes | 947 ns (923 to 969) | 1,117 ns (1,115 to 1,120) | 3.0 ns |
| Burst of 10,000 records | 6.86 ms (6.57 to 7.20) | 6.97 ms (6.95 to 7.00) | 0.03 ms |
| Latency distribution | 649 ns (639 to 659) | 587 ns (585 to 590) | 2.7 ns |

## How should I read them?

On the calling thread, rlg costs about what `tracing` costs, and less with
attributes, while `tracing` has already formatted the event and rlg has not.
What rlg buys is that the caller never waits on the sink's I/O, which this
suite's discarding writer does not exercise. Runner speed varies between runs:
compare within one run, not across runs.

## What about many threads?

Version 0.0.14 cut contention between producers. On the maintainer's
development machine, with eight threads logging at once, `fire()` went from
3.9 to 1.8 µs per call, and from 3.0 to 0.95 µs with the `fast-queue`
feature. These come from the
[CHANGELOG](https://github.com/sebastienrousseau/rlg/blob/main/CHANGELOG.md),
not from a published run.

## How do I reproduce them?

From a clone of the repository:

```bash
cargo bench -p rlg --bench competitive_bench
```

Each release tag also runs every workspace benchmark on a GitHub-hosted
runner and keeps the Criterion report as a workflow artifact for 90 days.

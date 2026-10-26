---
name: "rlg"
short_name: "rlg"
title: "Privacy: what rustlogs.com collects | rlg"
description: "What happens to your data on rustlogs.com: hosting on GitHub Pages, the contact form, and the theme preference stored in your browser."
keywords: "rlg privacy, rustlogs.com privacy policy"
author: "Sebastien Rousseau"
date: "2026-10-26"
news_publication_date: "2026-10-26"
layout: "page"
language: "en-GB"
schema: "page"
changefreq: "monthly"
eyebrow: "Privacy"
headline: "What this site collects"
lead: "Very little, and nothing that identifies you unless you send a message."
---
## What does the site itself store?

Only your choice of light, dark or system theme, saved in your browser's
local storage so the next page uses it. The site sets no cookies and runs no
analytics.

## Who hosts it?

The site is served by GitHub Pages, which processes the technical data any
web request carries, such as your IP address, under GitHub's own privacy
policy.

## What happens to a message I send?

The contact form posts to Formspree, which delivers it by email. Your name,
email address and message are used only to reply to you.

## Does rlg send data anywhere?

Only where you tell it to. The `rlg` library writes to your system log or
standard output; `rlg-otlp` sends records to the OpenTelemetry Collector you
configure, and `rlg-mcp` serves log files to the MCP clients that connect to
it. None of the crates sends data to the project.

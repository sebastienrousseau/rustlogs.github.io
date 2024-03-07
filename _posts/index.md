---

# Front Matter (YAML)

author: "The RustLogs (RLG) Team"
banner_alt: "Banner of the RustLogs (RLG) Official Website"
banner_height: "100vh"
banner_width: "100vw"
banner: "https://kura.pro/rlg/images/github/github-rlg.webp"
cdn: "https://kura.pro"
changefreq: "weekly"
charset: "utf-8"
cname: "rustlogs.com"
copyright: "© 2024 RustLogs (RLG). All rights reserved."
date: "Mar 08, 2024"
description: "Discover RustLogs (RLG), the flexible logging library for Rust with structured log formats, asynchronous logging, and extensive customization options."
download_url: ""
download_title: ""
format-detection: "telephone=no"
hero_description: "Discover RustLogs (RLG), the flexible logging library for Rust with structured log formats, asynchronous logging, and extensive customization options."
hreflang: "en"
icon: "https://kura.pro/rlg/images/favicon.ico"
id: "https://rustlogs.com/index.html"
image_alt: "Logo of the RustLogs (RLG) Library"
image_height: "100vh"
image_width: "100vw"
image: "https://kura.pro/rlg/images/logos/rlg.webp"
keywords: "Rust logging library, asynchronous Rust logging, structured log formats, Rust application debugging, customizable logging Rust, Rust development tools, RustLogs RLG features, efficient Rust logging, RustLogs integration, RustLogs documentation"
language: "en-GB"
layout: "index"
locale: "en_GB"
logo_alt: "Logo of the RustLogs (RLG) Library"
logo_height: "44"
logo_width: "44"
logo: "https://kura.pro/rlg/images/logos/rlg.webp"
menu: "active"
measurementID: "G-6BQMFZ772M"
name: "RustLogs (RLG)"
permalink: "https://rustlogs.com"
rating: "general"
referrer: "no-referrer"
revisit-after: "7 days"
robots: "index, follow"
short_name: "rustlogs"
subtitle: "Simplifying Application-Level Logging"
tags: "Rust, Logging, Asynchronous, Structured, Customizable, Debugging, Development, RustLogs, Integration, Documentation"
theme-color: "167, 42, 0"
title: "RustLogs (RLG): Advanced Logging Library for Rust"
url: "https://rustlogs.com/index.html"
viewport: "width=device-width, initial-scale=1, shrink-to-fit=no"

# RSS - The RSS feed front matter (YAML).

atom_link: "https://rustlogs.com/rss.xml"
category: "Logging, Macros, Configuration, Output Formats, Asynchronous, Performance, Error Handling, Filtering, Customization, Integration"
docs: "https://validator.w3.org/feed/docs/rss2.html"
generator: "Shokunin SSG (version 0.0.26)"
item_description: "Discover RustLogs (RLG), the flexible logging library for Rust with structured log formats, asynchronous logging, and extensive customization options."
item_guid: "https://rustlogs.com/rss.xml"
item_link: "https://rustlogs.com/rss.xml"
item_pub_date: "Fri, 08 Mar 2024 08:08:08 +0100"
item_title: "Shokunin - RSS Feed"
last_build_date: "Fri, 08 Mar 2024 08:08:08 +0100"
managing_editor: "contact@rustlogs.com"
pub_date: "Fri, 08 Mar 2024 08:08:08 +0100"
ttl: "60"
type: "website"
webmaster: "contact@rustlogs.com"

# Apple - The Apple front matter (YAML).

apple_mobile_web_app_orientations: "portrait"
apple_touch_icon_sizes: "192x192"
apple-mobile-web-app-capable: "yes"
apple-mobile-web-app-status-bar-inset: "black"
apple-mobile-web-app-status-bar-style: "black-translucent"
apple-mobile-web-app-title: "RustLogs (RLG): Advanced Logging Library for Rust"
apple-touch-fullscreen: "yes"

# MS Application - The MS Application front matter (YAML).

msapplication-config: "https://rustlogs.com/browserconfig.xml"
msapplication-tap-highlight: "no"
msapplication-TileColor: "167, 42, 0"
msapplication_tile_image: "https://kura.pro/rlg/images/logos/rlg.webp"

# Twitter Card - The Twitter Card front matter (YAML).

twitter_card: "summary"
twitter_creator: "@wwdseb"
twitter_description: "Discover RustLogs (RLG), the flexible logging library for Rust with structured log formats, asynchronous logging, and extensive customization options."
twitter_image: "https://kura.pro/rlg/images/logos/rlg.webp"
twitter_image_alt: "Logo of the RustLogs (RLG) Library"
twitter_site: "@wwdseb"
twitter_title: "RustLogs (RLG): Advanced Logging Library for Rust"
twitter_url: "https://rustlogs.com/"

# Humans.txt - The Humans.txt front matter (YAML).

author_website: "https://rustlogs.com/index.html"
author_twitter: "@wwdseb"
author_location: "London, UK"
thanks: "Thanks for reading!"
site_last_updated: "2024-03-08"
site_standards: "HTML5, CSS3, RSS, Atom, JSON, XML, YAML, Markdown, TOML"
site_components: "Shokunin SSG, Shokunin CLI, Shokunin Templates, Kaishi Templates, Kaishi Themes"
site_software: "Shokunin, Rust"

---

RustLogs (RLG) is a powerful and flexible logging library for Rust that provides application-level logging with a simple and readable output format. It offers a wide range of features and customization options to streamline your logging workflow and enhance your Rust applications.

## Overview

RustLogs (RLG) simplifies the process of adding logging functionality to your Rust applications. With its intuitive APIs and helpful macros, you can easily integrate logging into your codebase and gain valuable insights into your application's behavior.

![divider][divider].class=\"m-10 w-100\"

## Features

- **Multiple Log Levels**: RustLogs (RLG) supports a comprehensive set of log levels, including `ALL`, `DEBUG`, `DISABLED`, `ERROR`, `FATAL`, `INFO`, `NONE`, `TRACE`, `VERBOSE`, and `WARNING`, allowing you to control the granularity of your logs.

- **Structured Log Formats**: The library provides structured log formats that are easy to parse and filter, enabling efficient analysis and troubleshooting of your application's logs.

- **Extensive Output Format Compatibility**: RustLogs (RLG) supports a wide range of output formats, including 
  - **Common Event Format (CEF)** - A standardized log format developed by ArcSight for the exchange of event information between security devices and applications.
  - **Extended Log Format (ELF)** - A log format that extends the Common Log Format (CLF) with additional fields to provide more detailed information about web server requests.
  - **Graylog Extended Log Format (GELF)** - A log format designed for use with the Graylog log management system, which allows for the transmission of log messages over a network.
  - **JavaScript Object Notation (JSON)** - A lightweight data interchange format that is easy for humans to read and write and easy for machines to parse and generate.
  - **NCSA Common Log Format (CLF)** - A standardized log format used by web servers to log client requests, including information such as the client IP address, request method, and response status code.
  - **W3C Extended Log File Format (W3C)** - A log format developed by the World Wide Web Consortium (W3C) that extends the Common Log Format (CLF) with additional fields for more detailed logging of web server requests.
  - **Syslog Format** - A standardized log format used by various network devices and applications to send event messages to a centralized logging server.
  - **Apache Access Log Format** - A log format used by the Apache web server to log client requests, including information such as the client IP address, request method, and response status code.
  - **Logstash Format** - A log format used by the Logstash data processing pipeline, which allows for the collection, parsing, and forwarding of log data from various sources.
  - **Log4j XML Format** - A log format used by the Log4j logging framework, which allows for the structured logging of events in an XML format.
  - **NDJSON (Newline Delimited JSON)** - A log format where each log message is represented as a JSON object on a separate line, making it easy to parse and process log data in a streaming fashion.

- **Flexible Configuration**: Customize the behavior of RustLogs (RLG) to suit your application's needs. You can easily configure the log file path, log levels, and output formats to match your requirements.

- **Asynchronous Logging**: RustLogs (RLG) supports asynchronous logging, allowing you to log messages without blocking your application's execution. This ensures optimal performance and responsiveness.

- **Helpful Macros**: The library provides a set of convenient macros that simplify common logging tasks, such as creating log entries, printing logs to stdout, logging to files, and conditionally logging based on feature flags or predicates.

![divider][divider].class="m-10 w-100"

## Get Started with RustLogs (RLG) Today

Ready to take your Rust application logging to the next level? Get started with RustLogs (RLG) today and experience the power and flexibility of advanced logging!

### 🚀 Easy Integration

Integrating RustLogs (RLG) into your Rust project is a breeze. With just a few lines of code, you can start logging messages and gaining valuable insights into your application's behaviour.

### 📚 Comprehensive Documentation

Dive into our comprehensive documentation to learn more about RustLogs (RLG)'s features, configuration options, and usage examples. Our docs will guide you every step of the way.

### 💬 Community Support

Join our vibrant community of Rus t developers and logging enthusiasts. Get help, share your experiences, and contribute to the future of RustLogs (RLG). Together, let's make logging better!

[**Get Started Now →**](/getting-started/index.html)

![divider][divider].class=\"m-10 w-100\"

[divider]: https://kura.pro/common/images/elements/divider.svg "Divider"

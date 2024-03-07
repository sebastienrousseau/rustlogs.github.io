---

# Front Matter (YAML)

author: "The RustLogs (RLG) Team"
banner_alt: "MacBook Pro on white surface"
banner_height: "100vh"
banner_width: "100vw"
banner: "https://kura.pro/stock/images/banners/tianyi-ma-WiONHd_zYI4.webp"
cdn: "https://kura.pro"
changefreq: "weekly"
charset: "utf-8"
cname: ""
copyright: "© 2024 RustLogs (RLG). All rights reserved."
date: "Mar 08, 2024"
description: "Learn how to set up RustLogs (RLG) in your Rust projects for effective logging. Installation, configuration, and usage guide included."
download: ""
format-detection: "telephone=no"
hreflang: "en"
icon: "https://kura.pro/shokunin/images/favicon.ico"
id: "https://rustlogs.com/about/index.html"
image_alt: "Image of RustLogs (RLG)"
image_height: "100vh"
image_width: "100vw"
image: "https://kura.pro/rlg/images/logos/rlg.webp"
keywords: "RustLogs guide, Rust logging, RLG installation, Rust project configuration, asynchronous logging Rust, Rust error handling, logging macros Rust, RustLog setup, Rust documentation, RustLogs API"
language: "en-GB"
layout: "getting-started"
locale: "en_GB"
logo_alt: "Logo of RustLogs (RLG)"
logo_height: "44"
logo_width: "44"
logo: "https://kura.pro/rlg/images/logos/rlg.webp"
menu: "active"
measurementID: "G-6BQMFZ772M"
name: "RustLogs (RLG)"
permalink: "https://rustlogs.com/about/index.html"
rating: "general"
referrer: "no-referrer"
revisit-after: "7 days"
robots: "index, follow"
short_name: "rustlogs"
subtitle: "Streamline Your Rust Logging Workflow with RustLogs (RLG)"
tags: "RustLogs, Rust, Logging, Configuration, Asynchronous, ErrorHandling, Macros, Installation, Documentation, API"
theme-color: "167, 42, 0"
title: "Getting Started with RustLogs: Your Guide to Rust Logging"
url: "https://rustlogs.com/about/index.html"
viewport: "width=device-width, initial-scale=1, shrink-to-fit=no"

# RSS - The RSS feed front matter (YAML).

atom_link: "https://rustlogs.com/about/rss.xml"
category: "Software, Static Site Generator, Rust"
docs: "https://validator.w3.org/feed/docs/rss2.html"
generator: "Shokunin SSG (version 0.0.26)"
item_description: "Learn how to set up RustLogs (RLG) in your Rust projects for effective logging. Installation, configuration, and usage guide included."
item_guid: "https://rustlogs.com/about/rss.xml"
item_link: "https://rustlogs.com/about/rss.xml"
item_pub_date: "Fri, 08 Mar 2024 08:08:08 +0100"
item_title: "Getting Started with RustLogs: Your Guide to Rust Logging"
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
apple-mobile-web-app-title: "Getting Started with RustLogs: Your Guide to Rust Logging"
apple-touch-fullscreen: "yes"

# MS Application - The MS Application front matter (YAML).

msapplication-config: "https://rustlogs.com/browserconfig.xml"
msapplication-tap-highlight: "no"
msapplication-TileColor: "167, 42, 0"
msapplication_tile_image: "https://kura.pro/rlg/images/logos/rlg.webp"

# Twitter Card - The Twitter Card front matter (YAML).

twitter_card: "summary"
twitter_creator: "@wwdseb"
twitter_description: "Learn how to set up RustLogs (RLG) in your Rust projects for effective logging. Installation, configuration, and usage guide included."
twitter_image: "https://kura.pro/rlg/images/logos/rlg.webp"
twitter_image_alt: "Image of RustLogs (RLG)"
twitter_site: "@wwdseb"
twitter_title: "Getting Started with RustLogs: Your Guide to Rust Logging"
twitter_url: "https://rustlogs.com/about/index.html"

# Humans.txt - The Humans.txt front matter (YAML).

author_website: "https://rustlogs.com/about/index.html"
author_twitter: "@wwdseb"
author_location: "London, UK"
thanks: "Thanks for reading!"
site_last_updated: "2023-12-18"
site_standards: "HTML5, CSS3, RSS, Atom, JSON, XML, YAML, Markdown, TOML"
site_components: "Shokunin SSG, Shokunin CLI, Shokunin Templates, Shokunin Themes, Kaishi SSG, Kaishi CLI, Kaishi Templates, Kaishi Themes"
site_software: "Shokunin, Rust"

---

In the evolving landscape of software development, logging remains a critical component for monitoring, debugging, and ensuring the robustness of applications. Rust, known for its safety and performance, offers various logging libraries tailored to its ecosystem. Among these, RustLogs (RLG) stands out as a flexible and efficient logging framework.

Dive into RustLogs (RLG) for efficient logging in your Rust projects. This guide covers installation, configuration, and best practices to get you started with RLG, enhancing your Rust applications with powerful logging capabilities.

## Installation

To start using RustLogs (RLG) in your Rust project, add the following line to your `Cargo.toml` file:

```toml
[dependencies]
rlg = "0.0.3"
```

RustLogs (RLG) requires Rust 1.67.0 or later.

![divider][divider].class="m-10 w-100"

## Basic Configuration

By default, RustLogs (RLG) logs to a file named `RLG.log` in the current directory. You can customize the log file path by setting the `LOG_FILE_PATH` environment variable:

```rust
std::env::set_var("LOG_FILE_PATH", "/path/to/log/file.log");
```

RustLogs (RLG) provides a `Config` struct that allows you to load the configuration from environment variables or use default values:

```rust
use rlg::config::Config;

let config = Config::load();
```

![divider][divider].class="m-10 w-100"

## Creating Log Entries

To create a new log entry, you can use the `Log::new()` function:

```rust
use rlg::log::Log;
use rlg::log_format::LogFormat;
use rlg::log_level::LogLevel;

let log_entry = Log::new(
    "12345",
    "2023-01-01T12:00:00Z",
    &LogLevel::INFO,
    "MyComponent",
    "This is a sample log message",
    &LogFormat::JSON,
);
```

The `Log::new()` function takes the following parameters:

- `session_id`: A unique identifier for the session.
- `time`: The timestamp in ISO 8601 format.
- `level`: The logging level.
- `component`: The component generating the log.
- `description`: The log message.
- `format`: The format for the log message.

![divider][divider].class="m-10 w-100"

## Logging Messages

To log a message, you can use the `log()` method on a log entry:

```rust
tokio::runtime::Runtime::new().unwrap().block_on(async {
    log_entry.log().await.unwrap();
});
```

RustLogs (RLG) supports asynchronous logging, allowing you to log messages without blocking your application's execution. The `log()` method returns a `Result<(), io::Error>` that indicates the outcome of the logging operation.

![divider][divider].class="m-10 w-100"

## Using Macros

RustLogs (RLG) provides a set of helpful macros to simplify common logging tasks. Here are a few examples:

- `macro_log!`: Creates a new log entry with specified parameters.

```rust
let log = macro_log!(session_id, time, level, component, description, format);
```

- `macro_info_log!`: Creates an info log with default session ID and format.

```rust
let log = macro_info_log!(time, component, description);
```

- `macro_print_log!`: Prints a log to stdout.

```rust
macro_print_log!(log);
```

- `macro_log_to_file!`: Asynchronously logs a message to a file.

```rust
let result = macro_log_to_file!(log);
```

For a complete list of available macros and their usage, refer to the [RustLogs (RLG) Documentation](https://docs.rs/rlg).

![divider][divider].class="m-10 w-100"

## Error Handling

Errors can occur during logging operations, such as file I/O errors or formatting errors. The `log()` method returns a `Result<(), io::Error>` that indicates the outcome of the logging operation. You should handle potential errors appropriately in your code:

```rust
match log_entry.log().await {
    Ok(_) => println!("Log entry successfully written"),
    Err(err) => eprintln!("Error logging entry: {}", err),
}
```

![divider][divider].class="m-10 w-100"

## Next Steps

Congratulations! You're now ready to start using RustLogs (RLG) in your Rust projects. Here are some next steps you can take:

- Customize the log format and output destinations to suit your application's needs.
- Integrate RustLogs (RLG) into your existing Rust projects and start logging messages effectively.
- Contribute to the [RustLogs (RLG) Repository](https://github.com/sebastienrousseau/rlg) by reporting issues, suggesting improvements, or submitting pull requests.

If you have any questions or need further assistance, check our [**FAQs**](/faqs/index.html) and feel free to reach out to the RustLogs (RLG) community or open an issue on the [GitHub repository](https://github.com/sebastienrousseau/rlg).

Happy logging with RustLogs (RLG)!

![divider][divider].class="m-10 w-100"

[divider]: https://kura.pro/common/images/elements/divider.svg "Divider"

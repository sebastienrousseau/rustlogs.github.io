<!-- markdownlint-disable MD033 MD041 -->

<img
  align="right"
  alt="Logo of RustLogs (RLG)"
  height="261"
  src="https://kura.pro/rlg/images/logos/rlg.webp"
  width="261"
  />

<!-- markdownlint-enable MD033 MD041 -->

# RustLogs.com - Official Website 🌏

Welcome to the repository for [rustlogs.com][00], the digital presence of
RustLogs (RLG).

## Quick Start Guide

Setting up and running the website locally is easy and quick with the
[Shokunin Static Site Generator (SSG)][00].

### Prerequisites

Ensure you have the **Rust toolchain** installed. If not, follow the guide on
the [Rust website][01] to set it up.

### Installation & Usage

1. Install Shokunin SSG:

```shell
cargo install ssg
```

2. Clone the repository

```shell
git clone https://github.com/sebastienrousseau/rustlogs.github.io.git
```

3. Change into the repository directory:

```shell
cd rustlogs.github.io
```

4. Generate the static site for rustlogs.com:

```shell
ssg -n=docs -c=_posts -t=_layouts -o=output -s=public
```

[00]: https://rustlogs.com "RustLogs (RLG) Official Website"
[01]: https://www.rust-lang.org/learn/get-started "Rust Getting started guide"

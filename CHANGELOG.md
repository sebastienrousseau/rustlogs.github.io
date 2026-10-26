<!-- SPDX-License-Identifier: Apache-2.0 OR MIT -->

# Changelog

All notable changes to rustlogs.com are documented here.

## [0.0.2] - Unreleased

### Changed

- Rebuilt the site with Static Site Generator 0.0.69 and the Prism theme,
  coloured from the rlg logo, replacing the 2024 Shokunin site.
- Rewrote every page from the rlg repository at version 0.0.15: the site
  now describes the ring-buffer engine, its 14 formats, the native sinks
  and the nine satellite crates, instead of the 0.0.3 API, and drops
  text copied from a finance project.
- `make build` checks colour contrast (WCAG AAA), requires ssg's quality
  gate at 10/10, verifies every `#fragment` and SRI hash, renders one social
  image per page and runs `ssg audit`; the Pages workflow builds and deploys
  the result instead of uploading committed output.
- Removed Google Analytics, reCAPTCHA and the third-party scripts the old
  pages loaded; the site sets no cookies.

## [0.0.1] - 2026-10-26 (merged, not tagged)

### Changed

- Licensed the site's source under Apache-2.0 OR MIT.
- Deployed GitHub Pages with GitHub Actions instead of from a branch.

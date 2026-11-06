# Linux Disk Space Sentinel

Sample Linux filesystem capacity and show recent usage with threshold alerts.

[![Build and verify](https://github.com/netcodermike/linux-disk-space-sentinel/actions/workflows/verify.yml/badge.svg)](https://github.com/netcodermike/linux-disk-space-sentinel/actions/workflows/verify.yml)

**Language:** Rust · **Platform:** Linux · **App type:** Linux disk capacity trend monitor

First-release learning implementation, with explicit scope below. This is one independently runnable app, not a placeholder in a combined repository.

## Features

- GNU df snapshots
- Configurable path/threshold
- Five-second watch mode
- Twenty in-memory samples

## Requirements

Rust stable, Linux, and the documented system data sources. No external crates.

## Quick start

```sh
git clone https://github.com/netcodermike/linux-disk-space-sentinel.git
cd linux-disk-space-sentinel
cargo run --release
```

Commands run from the repository root. Lines marked for a particular operating system apply only there. Language tools are prerequisites, not installed by the app. Close desktop windows normally; terminal/server apps stop with Ctrl+C unless their usage notes say otherwise.

## Example workflow

Run cargo run --release -- --path / --threshold 80, then add --watch and observe a test filesystem. Stop with Ctrl+C.

## Architecture, configuration, and data

src/main.rs executes GNU df -Pk with a literal --path argument after --, parses POSIX rows, and prints available KiB/capacity. --watch samples every five seconds; a 20-sample queue shows recent usage. --threshold is 0–100 and defaults to 90. LC_ALL=C stabilizes parsing. Parser tests use synthetic df records. No files are changed.

## Verification

Build with the quick-start command where needed, then run:

```sh
cargo test
```

Manual acceptance: Run cargo run --release -- --path / --threshold 80, then add --watch and observe a test filesystem. Stop with Ctrl+C. Also test malformed inputs and an unwritable output destination.

See [validation record](docs/VALIDATION.md) for observed checks and their limits. CI results verify only what the workflow runs; they do not establish native GUI correctness or production readiness.

## Limitations and next steps

Linux/GNU df only. No predictive forecasting, persisted history, notifications, inode accounting, or disk writes. Capacity comes from df and may reflect reserved blocks. Mount names are displayed with escaped control characters; unusual wrapped/multiline df output may not parse.

## Troubleshooting

Use the exact platform/toolchain above. Preserve originals and use synthetic fixtures first. Errors are displayed in the UI or stderr. A headless machine can run the model tests but cannot verify native GUI/audio behavior. Output paths are chosen explicitly; keep backups of important data.

## Source map

- `Cargo.toml`
- `src/main.rs`

## Development and license

Read [contributing](CONTRIBUTING.md) before changing the app, and [security/privacy](SECURITY.md) before supplying private data. Use synthetic fixtures for tests. No telemetry, paid API key, or account is required by the app itself. Dependencies, when present, are stated in Requirements.

[MIT license](LICENSE), copyright 2026 netcodermike.

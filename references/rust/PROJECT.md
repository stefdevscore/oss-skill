# Rust Project Reference

## Overview

This reference covers Rust-specific project setup, configuration, and patterns for open-source libraries (crates) and CLI tools. Rust's toolchain is unified — `cargo` handles building, testing, managing dependencies, and publishing.

## Project Structure

### Library Crate

```
project-root/
├── src/
│   ├── lib.rs                # crate root
│   └── utils.rs              # modules
├── tests/                    # integration tests
│   └── integration_test.rs
├── benches/                  # benchmarks
│   └── my_bench.rs
├── examples/                 # runnable examples
│   └── basic.rs
├── Cargo.toml
├── Cargo.lock                # committed for binaries, optional for libraries
├── LICENSE
├── README.md
└── CHANGELOG.md
```

### Binary Crate (CLI Tool)

```
project-root/
├── src/
│   ├── main.rs               # binary entry point
│   ├── lib.rs                # shared logic (testable)
│   └── cli.rs                # CLI argument parsing
├── tests/
├── Cargo.toml
├── Cargo.lock                # always committed for binaries
├── LICENSE
└── README.md
```

### Workspace (Monorepo)

```
workspace-root/
├── Cargo.toml                # [workspace] definition
├── crates/
│   ├── core/
│   │   ├── src/lib.rs
│   │   └── Cargo.toml
│   ├── cli/
│   │   ├── src/main.rs
│   │   └── Cargo.toml
│   └── utils/
│       ├── src/lib.rs
│       └── Cargo.toml
└── Cargo.lock
```

```toml
# workspace Cargo.toml
[workspace]
members = ["crates/*"]
resolver = "2"

[workspace.package]
edition = "2024"
license = "MIT"
repository = "https://github.com/user/project"

[workspace.dependencies]
serde = { version = "1", features = ["derive"] }
tokio = { version = "1", features = ["full"] }
```

## Cargo.toml

### Complete Library Example

```toml
[package]
name = "my-crate"
version = "1.0.0"
edition = "2024"
rust-version = "1.85"
description = "A clear, one-line description"
license = "MIT"
repository = "https://github.com/user/my-crate"
homepage = "https://github.com/user/my-crate"
documentation = "https://docs.rs/my-crate"
readme = "README.md"
keywords = ["keyword1", "keyword2"]
categories = ["command-line-utilities"]

[dependencies]
serde = { version = "1", features = ["derive"] }

[dev-dependencies]
tokio = { version = "1", features = ["test-util", "macros"] }
```

### Key Fields

| Field | Purpose | Required for crates.io |
|---|---|---|
| `name` | Crate name | Yes |
| `version` | SemVer version | Yes |
| `edition` | Rust edition (2021, 2024) | Recommended |
| `rust-version` | Minimum supported Rust version (MSRV) | Recommended |
| `description` | Short description (<= 255 chars) | Yes |
| `license` | SPDX identifier | Yes |
| `repository` | Source code URL | Recommended |
| `documentation` | Docs URL (defaults to docs.rs) | Optional |
| `keywords` | Up to 5 search keywords | Recommended |
| `categories` | Up to 5 from [crates.io categories](https://crates.io/categories) | Recommended |

## Edition and MSRV

### Editions

| Edition | Features |
|---|---|
| **2015** | Original Rust |
| **2018** | Module system changes, `async`/`await` |
| **2021** | `IntoIterator` for arrays, new closure captures |
| **2024** | Latest — `unsafe_op_in_unsafe_fn`, `gen` blocks |

Always use the latest edition for new projects.

### Minimum Supported Rust Version (MSRV)

```toml
rust-version = "1.85"
```

- Set this to the oldest Rust version you test against.
- CI should test on both the MSRV and stable.
- Use `cargo msrv` to find the minimum version automatically.

## Features

Feature flags allow conditional compilation:

```toml
[features]
default = ["json"]
json = ["dep:serde_json"]
async = ["dep:tokio"]
full = ["json", "async"]

[dependencies]
serde_json = { version = "1", optional = true }
tokio = { version = "1", optional = true, features = ["full"] }
```

```rust
#[cfg(feature = "json")]
pub fn parse_json(input: &str) -> Result<Value, Error> {
    serde_json::from_str(input)
}
```

### Best Practices

- Keep `default` features minimal — users can opt in.
- Use `dep:` syntax (Rust 1.60+) to avoid implicit feature names.
- Document all features in README.

## Testing

Rust has built-in testing — no additional framework needed.

### Unit Tests (in source files)

```rust
pub fn add(a: i32, b: i32) -> i32 {
    a + b
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_add() {
        assert_eq!(add(1, 2), 3);
    }

    #[test]
    #[should_panic(expected = "overflow")]
    fn test_overflow() {
        add(i32::MAX, 1);
    }
}
```

### Integration Tests (in `tests/` directory)

```rust
// tests/integration_test.rs
use my_crate::add;

#[test]
fn test_add_externally() {
    assert_eq!(add(1, 2), 3);
}
```

### Running Tests

```bash
cargo test                      # all tests
cargo test --lib                # unit tests only
cargo test --test integration   # specific integration test
cargo test -- --nocapture       # show println! output
```

### Additional Test Tools

| Tool | Purpose |
|---|---|
| **cargo-nextest** | Faster test runner, better output |
| **proptest** | Property-based testing |
| **criterion** | Benchmarking |
| **insta** | Snapshot testing |

## Linting and Formatting

### Clippy (Linter)

```bash
cargo clippy                            # default lints
cargo clippy -- -W clippy::pedantic     # stricter lints
cargo clippy -- -D warnings             # treat warnings as errors (CI)
```

Configure in `Cargo.toml` or `clippy.toml`:

```toml
# Cargo.toml
[lints.clippy]
pedantic = { level = "warn", priority = -1 }
module_name_repetitions = "allow"
must_use_candidate = "allow"
```

### Rustfmt (Formatter)

```bash
cargo fmt                   # format
cargo fmt -- --check        # check (CI)
```

Configure in `rustfmt.toml`:

```toml
edition = "2024"
max_width = 100
use_field_init_shorthand = true
```

## CI/CD (GitHub Actions)

### Basic CI Workflow

```yaml
name: CI
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: dtolnay/rust-toolchain@stable
        with:
          components: clippy, rustfmt
      - uses: Swatinem/rust-cache@v2
      - run: cargo fmt -- --check
      - run: cargo clippy -- -D warnings
      - run: cargo test

  msrv:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: dtolnay/rust-toolchain@1.85  # MSRV
      - uses: Swatinem/rust-cache@v2
      - run: cargo test
```

## Documentation

### rustdoc (Built-In)

```bash
cargo doc --open              # build and open docs
cargo doc --no-deps           # skip dependency docs
```

Doc comments:

```rust
/// Adds two numbers.
///
/// # Examples
///
/// ```
/// use my_crate::add;
/// assert_eq!(add(1, 2), 3);
/// ```
///
/// # Panics
///
/// Panics on integer overflow in debug mode.
pub fn add(a: i32, b: i32) -> i32 {
    a + b
}
```

### docs.rs

- All crates published to crates.io get automatic documentation on [docs.rs](https://docs.rs).
- Configure with `[package.metadata.docs.rs]` in `Cargo.toml`:

```toml
[package.metadata.docs.rs]
all-features = true
```

## Cross-Compilation

### Common Targets

```bash
# Add a target
rustup target add x86_64-unknown-linux-musl

# Build for target
cargo build --release --target x86_64-unknown-linux-musl
```

### Cross-Compilation Tool: `cross`

```bash
cargo install cross
cross build --release --target aarch64-unknown-linux-gnu
```

### Binary Releases with cargo-dist

```bash
cargo install cargo-dist
cargo dist init                 # configure
cargo dist build                # build for all targets
```

Or use **GoReleaser** with `goreleaser-cross` for multi-arch binaries.

## Resources

- [The Rust Programming Language](https://doc.rust-lang.org/book/) — official book
- [Cargo Book](https://doc.rust-lang.org/cargo/) — cargo reference
- [Rust API Guidelines](https://rust-lang.github.io/api-guidelines/) — API design best practices
- [docs.rs](https://docs.rs) — automatic crate documentation
- [This Week in Rust](https://this-week-in-rust.org) — weekly newsletter

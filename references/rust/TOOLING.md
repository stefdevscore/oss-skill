# Rust Tooling Reference

## 1. Overview

This reference covers the primary development lifecycle tools for Rust projects: dependency management, testing, linting, formatting, documentation, and CI/CD pipelines.

## 2. Dependency Management & Scripts

Rust relies primarily on `cargo` for all standard package operations.

```bash
cargo add serde --features derive  # adds to Cargo.toml dependencies
cargo add tokio --dev              # dev-dependency
cargo update                       # resolves updates within SemVer bounds
cargo tree                         # view dependency graph
```

### Scripts Alternative (cargo-make)
Rust doesn't have a `package.json` "scripts" equivalent built-in natively, but developers often use `cargo-make` or standard `Makefile`.

```bash
cargo install cargo-make
cargo make test   # runs tasks from Makefile.toml
```

## 3. Testing

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

## 4. Linting and Formatting

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

## 5. Documentation

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

## 6. CI/CD (GitHub Actions)

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

### Cross-Compilation & Binary Distribution

```bash
# Common Tools:
cargo install cross
cross build --release --target aarch64-unknown-linux-gnu

# Binary Release Automation:
cargo install cargo-dist
cargo dist init                 # configure github actions CI
```

## 7. Troubleshooting & Limits
+
+### Crates.io Rate Limits (429)
+Crates.io enforces a **24-hour publishing limit** for new versions of the same crate. If performing rapid monorepo synchronizations, ensure each version is definitive to avoid hitting this wall.
+
+## 8. Resources
+
- [Clippy Lints](https://rust-lang.github.io/rust-clippy/master/)
- [Nextest](https://nexte.st/)
- [cargo-dist](https://opensource.axo.dev/cargo-dist/)

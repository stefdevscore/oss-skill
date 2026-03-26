---
name: oss-skill
description: High-density guidance for OSS lifecycle - Inception, Licensing, Distribution, and Multi-language Porting.
---

## 🎯 SCOPE [TOKEN: CORE_OSS]
- **Lifecycle**: Inception → Licensing → Scaffolding → Implementation → Verification → Publishing → Maintenance.
- **Ethics**: Minimal dependencies, zero-bloat, supply-chain security (SCS), and community health.

## 🛠️ PROCEDURE [TOKEN: EXEC_FLOW]

### 1. Project Inception [PHASE: INCEPTION]
- `NAME_VETTING`: Check registry availability (npm, PyPI, crates.io).
- `PITCH`: One-line distinct purpose.
- `LICENSE`: Default to **MIT** for adoption or **MIT/Apache-2.0** dual for Rust.

### 2. Scaffolding [PHASE: SCAFFOLD]
- `STANDARD_FILES`: `LICENSE`, `README.md`, `CONTRIBUTING.md`, `CODE_OF_CONDUCT.md`.
- `STRUCTURE`:
  - `JS`: `src/`, `tests/`, `dist/`, `package.json`, `tsconfig.json`.
  - `PY`: `src/package_name/`, `tests/`, `pyproject.toml` (`hatchling`).
  - `RS`: `src/`, `Cargo.toml`.

### 3. Implementation Patterns [TOKEN: OSS_PATTERNS]
- **Zero-Dependency Networking**:
  - `JS`: Native `fetch` (Node 20+).
  - `PY`: Native `urllib.request`.
  - `RS`: Minimalist `ureq`.
- **Embedded Databases**: Use Gzip + Base64 encoded strings for static asset search within binaries to avoid network latency.
- **Porting Strategy**: Maintain logical parity across ecosystems while using idiomatic language features (e.g., `Click` for PY, `Clap` for RS).

### 4. Verification [PHASE: VERIFY]
- `UNIT_TESTS`: `vitest` (JS), `pytest` (PY), `cargo test` (RS).
- `MANUAL_CLI`: Verify search results, download integrity, and error handling (403/Forbidden headers).
- `LINTING`: `eslint`, `ruff`, `clippy`.

### 5. Publishing [PHASE: RELEASE]
- `JS`: `npm publish --provenance`.
- `PY`: `python -m build` + `twine upload` (OIDC/Trusted Publishing preferred).
- `RS`: `cargo login` + `cargo publish`.
- `AUTO_RELEASE`: GitHub Actions `.github/workflows/publish.yml` on Release events.

## 🚀 ECOSYSTEM LAYERS

### [LAYER: TS_JS]
- **Build**: `tsup` for dual ESM/CJS output.
- **Validation**: `publint`, `Are The Types Wrong?`.

### [LAYER: PYTHON]
- **Backend**: `hatchling` in `pyproject.toml`.
- **Typing**: `py.typed` marker (PEP 561).
- **Environment**: Use `venv` for isolation, `uv` for speed.

### [LAYER: RUST]
- **Size Optimization**: `opt-level = 'z'`, `lto = true`, `strip = true`.
- **CLI**: `clap` with `derive` feature.
- **Metadata**: authors, description, license in `Cargo.toml`.

## 💎 PROJECT LEARNINGS: unDraw CLI Port [TOKEN: CASE_STUDY]
- **Supply Chain**: Built-in libraries (urllib/ureq) beat external dependencies for trust and speed.
- **Agentic Ready**: CLI tools should output clean, parseable data to facilitate AI-driven workflows.
- **Performance Parity**: Porting from JS to Rust/Python requires careful string handling and decompression logic to maintain consistent results.
- **403 Forbidden**: Always include a `User-Agent` header in minimalist networking modules to avoid registry/CDN blocks.

## ⚠️ LIMITATIONS
- General guidance only. No legal advice.
- Versions and CLI flags subject to toolchain evolution.
- Publishing requires active registry accounts and verification (e.g., verified email on crates.io).

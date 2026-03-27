---
name: oss-skill
description: Project-agnostic guidance for OSS lifecycle decisions across licensing, governance, packaging, verification, and release management.
---

## 🎯 SCOPE [TOKEN: CORE_OSS]
- **Lifecycle**: Inception → Licensing → Scaffolding → Implementation → Verification → Publishing → Maintenance.
- **Principles**: Clear purpose, explicit tradeoffs, supply-chain integrity, maintainability, and community health.
- **Stance**: Favor portable patterns and decision frameworks over stack-specific defaults.

## 🛠️ PROCEDURE [TOKEN: EXEC_FLOW]

### 1. Project Inception [PHASE: INCEPTION]
- `NAME_VETTING`: Check registry availability (npm, PyPI, crates.io).
- `PITCH`: One-line distinct purpose.
- `LICENSE`: Choose a license based on adoption goals, patent posture, copyleft intent, and contributor expectations. Common permissive choices include MIT, Apache-2.0, BSD, and dual-license patterns where appropriate.
- `AUDIENCE`: Decide whether the project is a library, CLI, service, template, or dataset because packaging and release norms differ by artifact type.

### 2. Scaffolding [PHASE: SCAFFOLD]
- `STANDARD_FILES`: `LICENSE`, `README.md`, `CONTRIBUTING.md`, `CODE_OF_CONDUCT.md`.
- `STRUCTURE`:
  - Keep source, tests, examples, scripts, and generated artifacts clearly separated.
  - Use ecosystem-standard manifests and lockfiles where appropriate.
  - Treat build output as generated unless the ecosystem has a strong reason to check it in.
  - Use a local-only folder such as `.internals/` for repository-specific operator notes, domain knowledge, and CI learnings that should not be promoted into canonical docs by default.

### 3. Implementation Patterns [TOKEN: OSS_PATTERNS]
- **Dependencies**: Prefer the simplest dependency set that preserves clarity, security, and maintainability; do not optimize for dependency count alone.
- **Artifacts**: Keep generated outputs and packaged assets out of the source layout by default. Binary embedding, vendoring, or generated datasets should be justified by distribution, performance, or offline requirements.
- **Portability**: When maintaining multiple ecosystem implementations, preserve behavioral parity at the contract level while allowing each language to stay idiomatic.
- **Compatibility**: Make runtime, platform, and support-matrix assumptions explicit before selecting tools or packaging strategies.

### 4. Verification [PHASE: VERIFY]
- `TEST_STRATEGY`: Use the ecosystem’s standard test runner or an established project convention.
- `QUALITY_GATES`: Validate formatting, linting, type checking, tests, package contents, and installability before release.
- `MANUAL_CHECKS`: Verify the user-facing contract for real install and runtime paths, not just source-tree execution.
- `KNOWLEDGE_CAPTURE`: Record repo-specific findings in a local-only layer first, then promote only durable and broadly reusable conclusions into tracked docs or ADRs.

### 5. Publishing [PHASE: RELEASE]
- `AUTH`: Prefer non-interactive CI publishing using trusted publishing or scoped registry tokens. Reserve interactive login flows for local/manual releases.
- `PACKAGING`: Build the exact distributable artifacts, inspect contents, and smoke-test installed artifacts before publish.
- `REGISTRY_RULES`: Account for registry-specific visibility, naming, provenance, and ownership rules.
- `AUTOMATION`: Release workflows should be explicit, idempotent, and separated from ordinary CI where helpful.

## 🚀 ECOSYSTEM LAYERS

### [LAYER: TS_JS]
- **Build**: Choose a build path based on package shape: unbundled `tsc`, a bundler, or runtime-native JS may each be appropriate.
- **Validation**: Check exports, types, tarball contents, and installability.

### [LAYER: PYTHON]
- **Backend**: Use a modern `pyproject.toml` build backend that matches project needs.
- **Typing**: Ship typing metadata when the package intends to be type-checker friendly.
- **Environment**: Isolate tooling and test environments; choose workflow tools for compatibility, speed, and contributor ergonomics.

### [LAYER: RUST]
- **Release Profile**: Tune binary size, debuggability, and build time according to the project’s distribution goals.
- **CLI/API Surface**: Choose crates and public interfaces that fit the project’s ergonomics and maintenance burden.
- **Metadata**: Keep package metadata complete, accurate, and aligned with the published artifact.

## 💎 GENERAL LEARNINGS [TOKEN: CASE_STUDY]
- **Supply Chain**: Fewer dependencies can reduce risk, but only when the replacement implementation remains maintainable and correct.
- **Machine-Friendly Output**: Tools intended for automation should expose stable, parseable output modes.
- **Cross-Ecosystem Ports**: Shared behavior matters more than identical internals; optimize for contract parity, not implementation symmetry.
- **Distribution Optimizations**: Embedding assets or datasets can be valuable for offline or single-binary distribution, but it should be an explicit tradeoff.
- **Network Robustness**: Minimal clients often still need user-agent, timeout, retry, and integrity decisions made deliberately.

## ⚠️ LIMITATIONS
- General guidance only. No legal advice.
- Registry rules, workflow semantics, and toolchain flags change over time.
- Publishing requires active registry access, ownership, and any registry-specific verification steps.

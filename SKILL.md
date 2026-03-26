---
name: internals-open-source-software
description: High-level guidance on creating, writing, and distributing open-source software — from project inception through licensing, documentation, publishing, community building, and long-term maintenance. Includes ecosystem-specific layers for TypeScript/JavaScript, Python, and Rust.
compatibility: "Language and ecosystem agnostic at the core layer. Ecosystem layers: TypeScript/JS (Node.js 18+), Python (3.10+), Rust (stable)."
metadata:
  author: "azk"
  version: "1.0.0"
license: MIT
---

## Scope

This skill covers the complete lifecycle of open-source software:

- **Project inception** — defining purpose, audience, and scope
- **Licensing** — choosing and applying an appropriate open-source license
- **Repository structure** — organizing files, directories, and community health files
- **Code quality** — writing readable, testable, and maintainable code
- **Documentation** — READMEs, API docs, guides, and changelogs
- **CI/CD** — automated testing, linting, and release pipelines
- **Distribution** — publishing to package registries, container registries, and release channels
- **Community** — attracting contributors, managing issues/PRs, and communication channels
- **Governance** — decision-making models, roles, and sustainability
- **Security** — vulnerability disclosure, supply-chain integrity, and signed releases

This skill does not provide legal advice, cover proprietary/source-available licensing models, or prescribe a specific programming language or framework.

## When to use

Use this skill when a user asks to:

- start a new open-source project from scratch
- choose or change an open-source license
- structure a repository with best-practice community health files
- write or improve a README, CONTRIBUTING guide, or CODE_OF_CONDUCT
- publish a library or tool to a package registry (npm, PyPI, crates.io, etc.)
- set up CI/CD for automated testing and releases
- build and grow an open-source community
- adopt a governance model or create a sustainability strategy
- handle security disclosures or supply-chain concerns

## Inputs

Required inputs:

- project type (library, CLI tool, framework, application, etc.)
- primary language / ecosystem

Optional inputs:

- intended audience (developers, end-users, enterprises)
- license preference (permissive, copyleft, public domain)
- distribution targets (npm, PyPI, Docker Hub, GitHub Releases, Homebrew, etc.)
- governance preference (solo maintainer, committee, foundation)
- security requirements (SLSA, SBOM, signing)
- existing repo state (greenfield or adding OSS practices to an existing project)

Assumptions:

- the user intends to release under a recognized open-source license (OSI-approved)
- the project will be hosted on a public Git forge (GitHub, GitLab, Codeberg, etc.)

## Outputs

Produce:

- actionable, ordered guidance tailored to the user's project
- file templates and scaffolding (LICENSE, README, CONTRIBUTING, etc.)
- recommended tooling and configurations
- checklists for launch readiness and ongoing maintenance

## Reference Guides (Core)

| Guide | Covers |
|---|---|
| [RESEARCH.md](RESEARCH.md) | Roadmap for future ecosystem expansions and advanced OSS patterns |
| [LICENSING.md](references/LICENSING.md) | Comparison matrix of common OSS licenses, decision flowchart, and how to apply a license |
| [DISTRIBUTION.md](references/DISTRIBUTION.md) | Package registries by ecosystem, SemVer, changelogs, and release automation |
| [GOVERNANCE.md](references/GOVERNANCE.md) | Governance models, community health files, contributor roles, and funding strategies |
| [SECURITY.md](references/SECURITY.md) | Supply chain security, SBOM, Sigstore, and vulnerability response lifecycle |

## Steps / Procedure

### 1. Define the project

- Clarify the project's purpose, target audience, and scope.
- Choose a clear, descriptive name. Check for conflicts on target registries.
- Write a one-line description and a short elevator pitch.

### 2. Choose a license

- Determine the user's intent: maximum adoption (permissive) vs. ensuring derivatives stay open (copyleft).
- Recommend a license from the decision matrix (see [LICENSING.md](references/LICENSING.md)).
- Place the `LICENSE` file at the repository root.
- Add SPDX license identifier headers to source files where appropriate.

### 3. Set up the repository

Create the standard structure (TypeScript variant shown):

```
project-root/
├── LICENSE
├── README.md
├── CONTRIBUTING.md
├── CODE_OF_CONDUCT.md
├── SECURITY.md
├── CHANGELOG.md
├── package.json
├── tsconfig.json
├── tsconfig.build.json
├── .github/
│   ├── ISSUE_TEMPLATE/
│   ├── PULL_REQUEST_TEMPLATE.md
│   └── workflows/           # CI/CD
├── src/                      # TypeScript source (.ts)
├── tests/                    # JavaScript tests (.mjs)
├── scripts/                  # automation scripts (.mjs)
├── dist/                     # build output (gitignored)
└── docs/                     # extended documentation
```

See [typescript/PROJECT.md](references/typescript/PROJECT.md) for detailed TypeScript project structure guidance.

### 4. Write quality code

- Follow the ecosystem's idiomatic style (linters, formatters).
- Write comprehensive tests (unit, integration, and where appropriate, end-to-end).
- Keep dependencies minimal and audited.
- Use meaningful commit messages (e.g., Conventional Commits).

### 5. Write documentation

- **README.md** — project name, badges, description, quick-start, installation, usage, contributing link, license.
- **CONTRIBUTING.md** — how to report bugs, suggest features, submit PRs, coding standards, and development setup.
- **CHANGELOG.md** — follow [Keep a Changelog](https://keepachangelog.com) format.
- **API / reference docs** — auto-generated where possible (JSDoc, Sphinx, rustdoc, etc.).

### 6. Set up CI/CD

- Automate testing on every push and PR.
- Automate linting and formatting checks.
- Automate release workflows (version bumping, changelog generation, registry publishing).
- Use dependency update bots (Dependabot, Renovate).

### 7. Publish and distribute

- Choose distribution channels appropriate to the ecosystem (see [DISTRIBUTION.md](references/DISTRIBUTION.md)).
- Follow Semantic Versioning (SemVer).
- Tag releases in Git and create GitHub/GitLab releases with release notes.
- Provide pre-built binaries or container images where applicable.

### 8. Build community

- Enable GitHub Discussions or link to a Discord/Matrix/Slack channel.
- Label issues with `good first issue` and `help wanted` for newcomers.
- Acknowledge contributors (All Contributors bot, CONTRIBUTORS file).
- Write a CODE_OF_CONDUCT (Contributor Covenant is the most widely adopted).
- Respond to issues and PRs promptly and respectfully.

### 9. Govern and sustain

- Choose a governance model appropriate to the project's scale (see [GOVERNANCE.md](references/GOVERNANCE.md)).
- Document decision-making processes.
- Explore sustainability options: sponsorships, grants, open-collective, dual licensing, paid support.
- Plan for maintainer succession and bus-factor reduction.

### 11. Secure the project

- Implement dependency scanning and SBOM generation.
- Establish a private vulnerability disclosure process.
- Sign releases and commits.

### 10. Maintain over time

- Triage issues and PRs regularly.
- Keep dependencies up to date.
- Deprecate old versions gracefully with migration guides.
- Communicate breaking changes clearly (major version bumps, deprecation notices).
- Monitor security advisories and respond to vulnerability reports.

---

## TypeScript / JavaScript Layer

For projects using **TypeScript as the primary language** with **JavaScript for tests, scripts, and config files** (`.mjs`), the following reference guides provide ecosystem-specific depth:

| Guide | Covers |
|---|---|
| [typescript/PROJECT.md](references/typescript/PROJECT.md) | tsconfig, project structure, module systems (ESM/CJS), type declarations, path aliases |
| [typescript/TOOLING.md](references/typescript/TOOLING.md) | Package managers, build tools (tsup), testing (vitest), linting (ESLint), CI/CD, and docs |
| [typescript/PUBLISHING.md](references/typescript/PUBLISHING.md) | package.json fields, exports map, npm provenance, scoped packages, and dist-tags |

### Quick-Start: TypeScript OSS Library

1. `npm init -y` → set `"type": "module"`
2. `npm install -D typescript tsup vitest`
3. Create `tsconfig.json` with strict mode and `NodeNext` module resolution
4. Write source in `src/` (`.ts`), tests in `tests/` (`.mjs`)
5. Configure `tsup` for ESM + CJS + `.d.ts` output
6. Set up `exports` map in `package.json`
7. Add `eslint.config.mjs` with `typescript-eslint`
8. Configure GitHub Actions for CI (typecheck, lint, test)
9. Validate with `npm pack --dry-run`, [publint](https://publint.dev), and [Are The Types Wrong?](https://arethetypeswrong.github.io)
10. `npm publish --provenance --access public`

---

## Python Layer

For projects using **Python**, the following reference guides provide ecosystem-specific depth:

| Guide | Covers |
|---|---|
| [python/PROJECT.md](references/python/PROJECT.md) | Project structure (src layout), pyproject.toml, and type hints (PEP 561) |
| [python/TOOLING.md](references/python/TOOLING.md) | uv, venv, pytest, ruff, mypy, CI/CD workflows, documentation, and task runners |
| [python/PUBLISHING.md](references/python/PUBLISHING.md) | Building packages, trusted publishing (OIDC), versioning (PEP 440), and pre-publish checklist |

### Quick-Start: Python OSS Library

1. Create project with src layout: `src/my_package/__init__.py`
2. Write `pyproject.toml` with `hatchling` build backend and full metadata
3. `uv venv && source .venv/bin/activate`
4. `uv pip install -e ".[dev]"`
5. Write tests with `pytest` in `tests/`
6. Configure `ruff` for linting/formatting and `mypy` for type checking
7. Add `py.typed` marker for PEP 561 compliance
8. Configure GitHub Actions for CI (multi-version matrix with 3.10–3.13)
9. Test on TestPyPI: `twine upload --repository testpypi dist/*`
10. Set up trusted publishing on PyPI, publish via GitHub Actions

---

## Rust Layer

For projects using **Rust**, the following reference guides provide ecosystem-specific depth:

| Guide | Covers |
|---|---|
| [rust/PROJECT.md](references/rust/PROJECT.md) | Project structure (lib/bin/workspace), Cargo.toml, editions, and features |
| [rust/TOOLING.md](references/rust/TOOLING.md) | cargo test, clippy, rustfmt, rustdoc, and CI/CD with cargo-dist |
| [rust/PUBLISHING.md](references/rust/PUBLISHING.md) | crates.io metadata, dual licensing, cargo publish, cargo-release, and yanking |

### Quick-Start: Rust OSS Library

1. `cargo new my-crate --lib`
2. Set edition, `rust-version`, description, license, and repository in `Cargo.toml`
3. Write source in `src/lib.rs`, unit tests inline with `#[cfg(test)]`
4. Add integration tests in `tests/`
5. Write doc comments with `///` and examples (tested by `cargo test`)
6. Configure clippy lints in `Cargo.toml`
7. Configure GitHub Actions for CI (fmt, clippy, test, MSRV)
8. `cargo package --verify` to validate the package
9. `cargo publish --dry-run` to preview
10. `cargo publish`

## Examples

**Example A: Start a new TypeScript CLI tool as OSS**

- Input: "I want to open-source a new CLI tool written in TypeScript"
- Output:
  1. Scaffold repo with `LICENSE` (MIT), `README.md`, `CONTRIBUTING.md`, `CODE_OF_CONDUCT.md`
  2. Initialize with `npm init`, add `tsconfig.json`, set up `vitest` for testing
  3. Configure GitHub Actions for CI (lint + test on push)
  4. Publish to npm with `npm publish --access public`
  5. Create a GitHub Release with changelog

**Example B: Add an open-source license to an existing project**

- Input: "My project has no license, I want maximum adoption"
- Output:
  1. Recommend MIT license (most permissive, widely understood)
  2. Create `LICENSE` file with current year and copyright holder
  3. Add `license` field to `package.json` / `pyproject.toml` / `Cargo.toml`
  4. Optionally add SPDX headers to source files

**Example C: Publish a Python library to PyPI**

- Input: "I want to distribute my Python library on PyPI"
- Output:
  1. Ensure `pyproject.toml` has complete metadata (name, version, description, license, URLs)
  2. Add classifiers and Python version requirements
  3. Build with `python -m build`
  4. Upload with `twine upload dist/*`
  5. Configure GitHub Actions to auto-publish on tagged releases

**Example D: Publish a Python library with trusted publishing**

- Input: "I want to publish my Python library to PyPI securely"
- Output:
  1. Ensure src layout with `py.typed` marker
  2. Configure `pyproject.toml` with `hatchling`, metadata, and classifiers
  3. Build with `python -m build`
  4. Set up trusted publisher on PyPI (link GitHub repo)
  5. Configure GitHub Actions with `pypa/gh-action-pypi-publish`

**Example E: Open-source a Rust CLI tool**

- Input: "I want to release my Rust CLI as an open-source binary"
- Output:
  1. Structure as `src/main.rs` + `src/lib.rs` for testability
  2. Set `Cargo.toml` metadata, dual license as `MIT OR Apache-2.0`
  3. Add clippy and rustfmt configs, GitHub Actions CI
  4. Use `cargo-dist` for cross-platform binary releases
  5. `cargo publish` for the crate, GitHub Release for binaries

**Example F: Publish a dual CJS/ESM TypeScript library to npm**

- Input: "I want to publish my TS library so it works with both ESM and CJS consumers"
- Output:
  1. Configure `tsup` with `format: ['esm', 'cjs']` and `dts: true`
  2. Set up `exports` map with `import` and `require` conditions
  3. Validate types with [Are The Types Wrong?](https://arethetypeswrong.github.io)
  4. Run `npm pack --dry-run` to verify package contents
  5. Publish with `npm publish --provenance --access public`

**Example G: Set up community infrastructure for a growing project**

- Input: "My project is getting contributors, how do I manage this?"
- Output:
  1. Add `CONTRIBUTING.md` with development setup, PR guidelines, and code standards
  2. Adopt Contributor Covenant as `CODE_OF_CONDUCT.md`
  3. Create issue templates (bug report, feature request)
  4. Add PR template with checklist
  5. Enable GitHub Discussions for Q&A
  6. Label existing issues with `good first issue` and `help wanted`

## Limitations / Failure modes

- This skill provides general guidance, not legal advice. Users should consult a lawyer for complex licensing decisions.
- Ecosystem-specific steps (npm, PyPI, crates.io, etc.) are illustrative — exact commands may change with tool updates.
- Community building advice is directional; outcomes depend on project quality, marketing, and timing.
- Governance models are recommendations — the right choice depends on project culture and contributor dynamics.

## Security / Tool access

- Never commit secrets, tokens, or credentials to the repository.
- Use `.gitignore` to exclude sensitive configuration files.
- Set up a `SECURITY.md` with a responsible disclosure policy.
- Sign releases and commits with GPG/SSH keys where feasible.
- Enable dependency vulnerability scanning (Dependabot alerts, `npm audit`, `pip audit`).
- Use provenance attestations when publishing to registries that support them.
- Review third-party dependencies before adding them to the project.

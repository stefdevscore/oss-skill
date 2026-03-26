# internals-open-source-software

A skill that provides high-level guidance on creating, writing, and distributing open-source software.

## What This Skill Covers

- Project inception and planning
- Choosing and applying open-source licenses
- Repository structure and community health files
- Code quality, testing, and documentation
- CI/CD and release automation
- Publishing to package registries and distribution channels
- Community building and contributor management
- Governance models and project sustainability
- Security and supply-chain integrity

## Structure

```
├── SKILL.md                      # Main skill instructions
├── RESEARCH.md                   # Roadmap and future additions
├── references/
│   ├── LICENSING.md              # License comparison and decision guide
│   ├── DISTRIBUTION.md           # Distribution channels and release practices
│   ├── GOVERNANCE.md             # Governance models and sustainability
│   │
│   ├── typescript/
│   │   ├── PROJECT.md            # TypeScript project setup and patterns
│   │   ├── TOOLING.md            # TS/JS build, test, lint, and release tools
│   │   └── PUBLISHING.md         # npm publishing guide
│   │
│   ├── python/
│   │   ├── PROJECT.md            # Python project setup and patterns
│   │   ├── TOOLING.md            # Python build, test, lint, and release tools
│   │   └── PUBLISHING.md         # PyPI publishing guide
│   │
│   └── rust/
│       ├── PROJECT.md            # Rust project setup and patterns
│       └── PUBLISHING.md         # crates.io publishing guide
├── README.md
└── LICENSE
```

## Reference Guides

### Core (Language-Agnostic)

| Guide | Description |
|---|---|
| [RESEARCH.md](RESEARCH.md) | Roadmap for future ecosystem expansions and advanced OSS patterns |
| [LICENSING.md](references/LICENSING.md) | Comparison matrix of common OSS licenses, decision flowchart, and how to apply a license |
| [DISTRIBUTION.md](references/DISTRIBUTION.md) | Package registries by ecosystem, SemVer, changelogs, and release automation |
| [GOVERNANCE.md](references/GOVERNANCE.md) | Governance models, community health files, contributor roles, and funding strategies |

### TypeScript / JavaScript Layer

| Guide | Description |
|---|---|
| [typescript/PROJECT.md](references/typescript/PROJECT.md) | tsconfig, project structure, module systems (ESM/CJS), type declarations, path aliases, JS test patterns |
| [typescript/TOOLING.md](references/typescript/TOOLING.md) | Build tools (tsup, esbuild), testing (vitest, jest), linting (ESLint), formatting, CI/CD, docs, release management |
| [typescript/PUBLISHING.md](references/typescript/PUBLISHING.md) | package.json fields, exports map, dual CJS/ESM, scoped packages, npm provenance, versioning |

### Python Layer

| Guide | Description |
|---|---|
| [python/PROJECT.md](references/python/PROJECT.md) | Project structure (src layout), pyproject.toml, type hints (PEP 561), virtual environments, dependency management |
| [python/TOOLING.md](references/python/TOOLING.md) | uv, pytest, ruff, mypy, CI/CD workflows, documentation (Sphinx, MkDocs), task runners |
| [python/PUBLISHING.md](references/python/PUBLISHING.md) | Building packages, trusted publishing (OIDC), TestPyPI, classifiers, versioning (PEP 440) |

### Rust Layer

| Guide | Description |
|---|---|
| [rust/PROJECT.md](references/rust/PROJECT.md) | Project structure (lib/bin/workspace), Cargo.toml, editions, features, testing, clippy, cross-compilation |
| [rust/PUBLISHING.md](references/rust/PUBLISHING.md) | crates.io metadata, dual licensing, cargo publish, cargo-release, yanking, binary releases |

## License

[MIT](LICENSE)

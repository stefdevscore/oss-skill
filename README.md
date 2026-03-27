# oss-skill

A project-agnostic skill for designing, structuring, verifying, and distributing open-source software across ecosystems.

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
- Decision frameworks for choosing tools rather than hard-coding one stack

## Structure

```
├── SKILL.md                      # Main skill instructions
├── RESEARCH.md                   # Roadmap and future additions
├── docs/
│   └── adrs/                     # Architecture decision records for the skill itself
├── references/
│   ├── LICENSING.md              # License comparison and decision guide
│   ├── DISTRIBUTION.md           # Distribution channels and release practices
│   ├── GOVERNANCE.md             # Governance models and sustainability
│   ├── SECURITY.md               # Supply chain and vulnerability disclosure
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
│   ├── rust/
│   │   ├── PROJECT.md            # Rust project setup and patterns
│   │   ├── TOOLING.md            # Rust testing, clippy, CI/CD, and docs
│   │   └── PUBLISHING.md         # crates.io publishing guide
├── README.md
└── LICENSE
```

## Reference Guides

### Core (Language-Agnostic)

| Guide | Description |
|---|---|
| [RESEARCH.md](RESEARCH.md) | Roadmap for future ecosystem expansions and advanced OSS patterns |
| [docs/adrs/README.md](docs/adrs/README.md) | Decision log for the skill's own structure, scope, and maintenance model |
| [LICENSING.md](references/LICENSING.md) | Comparison matrix of common OSS licenses, decision flowchart, and how to apply a license |
| [DISTRIBUTION.md](references/DISTRIBUTION.md) | Package registries by ecosystem, SemVer, changelogs, and release automation |
| [GOVERNANCE.md](references/GOVERNANCE.md) | Governance models, community health files, contributor roles, and funding strategies |
| [SECURITY.md](references/SECURITY.md) | Supply chain security, SBOM, Sigstore, and vulnerability response lifecycle |

### TypeScript / JavaScript Layer

| Guide | Description |
|---|---|
| [typescript/PROJECT.md](references/typescript/PROJECT.md) | tsconfig, project structure, module systems (ESM/CJS), type declarations, path aliases |
| [typescript/TOOLING.md](references/typescript/TOOLING.md) | Package managers, build/test/lint options, CI/CD, and docs |
| [typescript/PUBLISHING.md](references/typescript/PUBLISHING.md) | package.json fields, exports map, npm provenance, scoped packages, and dist-tags |

### Python Layer

| Guide | Description |
|---|---|
| [python/PROJECT.md](references/python/PROJECT.md) | Project structure (src layout), pyproject.toml, and type hints (PEP 561) |
| [python/TOOLING.md](references/python/TOOLING.md) | Environment management, testing, linting, typing, CI/CD workflows, documentation, and task runners |
| [python/PUBLISHING.md](references/python/PUBLISHING.md) | Building packages, trusted publishing (OIDC), versioning (PEP 440), and pre-publish checklist |

### Rust Layer

| Guide | Description |
|---|---|
| [rust/PROJECT.md](references/rust/PROJECT.md) | Project structure (lib/bin/workspace), Cargo.toml, editions, and features |
| [rust/TOOLING.md](references/rust/TOOLING.md) | Testing, linting, formatting, docs, release automation, and binary distribution options |
| [rust/PUBLISHING.md](references/rust/PUBLISHING.md) | crates.io metadata, dual licensing, cargo publish, cargo-release, and yanking |

## License

[MIT](LICENSE)

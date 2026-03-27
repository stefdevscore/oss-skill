# ADR Index

This folder records the major architectural and maintenance decisions for `oss-skill` itself.

## Ordering

ADRs are ordered by foundational weight first, then by sequence:

1. The workspace and ownership model
2. The skill's scope and abstraction model
3. The reference-writing style
4. Publishing and release guidance posture
5. The boundary between canonical docs and local-only repo knowledge

## Records

- [ADR-0001](ADR-0001-workspace-and-ownership.md) - Workspace root is a container, not a Git repo
- [ADR-0002](ADR-0002-project-agnostic-skill-scope.md) - `oss-skill` remains project-agnostic
- [ADR-0003](ADR-0003-comparative-reference-style.md) - References should be comparative, not tool-prescriptive
- [ADR-0004](ADR-0004-automation-first-publishing-guidance.md) - Publishing guidance should prefer automation-first registry-safe flows
- [ADR-0005](ADR-0005-local-internals-knowledge-layer.md) - Repository-specific working knowledge belongs in a local-only `.internals/` layer first

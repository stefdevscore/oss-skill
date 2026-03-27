# ADR-0004: Publishing Guidance Is Automation-First and Registry-Safe

## Status

Accepted

## Context

Manual publishing commands are easy to explain but often omit important registry behaviors such as scoped visibility rules, token requirements, trusted publishing, provenance, and installability checks. For a reusable OSS skill, release guidance should bias toward flows that are reproducible and safe in CI while still acknowledging local/manual publishing when necessary.

## Decision

Publishing guidance should prefer non-interactive CI-based release flows, exact artifact verification, and registry-specific caveats. Interactive login flows are treated as local/manual exceptions rather than the main path.

## Consequences

- Guidance should mention trusted publishing or scoped registry tokens where relevant.
- Release flows should emphasize package inspection and installed-artifact smoke tests, not only source-tree checks.
- Registry-specific constraints such as npm scoped public access, PyPI trusted publisher configuration, or Cargo token usage should be called out explicitly.
- The skill remains aligned with modern provenance and supply-chain expectations without binding itself to one exact CI provider layout.

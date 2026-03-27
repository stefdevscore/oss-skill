# ADR-0003: References Use Comparative Guidance, Not Hard Defaults

## Status

Accepted

## Context

Language-specific references naturally drift toward the toolchains most familiar to the maintainer. Over time this turns general OSS guidance into prescriptive stack recommendations, which is a poor fit for a reusable skill intended to support different repo ages, team preferences, and ecosystem constraints.

## Decision

Reference documents should frame tools as options with tradeoffs rather than universal defaults. When a tool is shown, the text should explain why it is useful and when a different option may be better.

## Consequences

- Terms like "recommended" should be used sparingly and only when the scope is narrow and justified.
- Comparative tables and tradeoff notes are preferred over one-tool prescriptions.
- Ecosystem-standard or widely-adopted tools can still be named, but they should not be presented as mandatory without context.
- The skill remains useful across greenfield, legacy, monorepo, library, CLI, and distribution-heavy projects.

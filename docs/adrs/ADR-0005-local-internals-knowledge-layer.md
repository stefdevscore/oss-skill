# ADR-0005: Local Internals Knowledge Layer

## Status
Accepted

## Context

When a reusable OSS workflow skill is applied repeatedly across real repositories, maintainers accumulate repository-specific and domain-specific knowledge that is useful operationally but not suitable as canonical, project-agnostic skill guidance.

Examples include:
- registry quirks for a specific package
- CI breadcrumbs and release sequencing notes
- temporary research and operational memory
- repository-specific domain context

If this information is mixed directly into tracked skill docs, the skill drifts away from its project-agnostic purpose.

## Decision

The skill adopts a local-only `.internals/` layer for repository-specific and domain-specific working knowledge.

The convention is:
- `.internals/` is ignored by Git by default
- `.internals/README.md` is tracked so the convention is discoverable
- temporary or repository-specific notes stay in `.internals/`
- durable, broadly reusable conclusions are promoted into tracked docs or ADRs

## Consequences

Positive:
- preserves a clean boundary between canonical guidance and operator memory
- gives agents and maintainers a predictable place to accumulate repo-specific context
- reduces the chance that temporary findings get encoded as permanent guidance

Negative:
- local-only notes require deliberate promotion if they become broadly useful
- some knowledge may remain machine-local unless maintainers curate it intentionally

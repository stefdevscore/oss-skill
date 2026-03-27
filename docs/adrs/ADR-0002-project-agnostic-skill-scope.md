# ADR-0002: `oss-skill` Remains Project-Agnostic

## Status

Accepted

## Context

Earlier versions of the skill accumulated project-specific guidance, product ideas, and release tactics derived from individual repos. That made the skill more brittle and caused future use to inherit assumptions that were only valid for one project or one release cycle.

## Decision

`oss-skill` is maintained as a project-agnostic decision framework for OSS lifecycle work. It may include examples, but it must not anchor its core guidance to a specific project, product name, repository layout, or historical release incident.

## Consequences

- Top-level guidance should describe principles and decision criteria first.
- Case studies must be expressed as generalized lessons unless a project-specific appendix is deliberately added.
- Product ideation notes and research prompts must avoid becoming hidden defaults for future work.
- References may use examples, but those examples should remain replaceable and clearly non-normative.

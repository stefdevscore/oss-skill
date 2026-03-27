# Local Internals

This folder is for local-only, repository-specific working knowledge that should not automatically become canonical project documentation.

Use it for:
- domain notes gathered while operating or extending a specific OSS repository
- CI breadcrumbs, release sequencing notes, and operator memory
- temporary research that may later be promoted into tracked docs or ADRs
- repo-specific context that helps an agent or maintainer work effectively

Do not use it for:
- public project docs that belong in `README.md`, `docs/`, or `references/`
- architecture decisions that should be recorded as ADRs
- secrets, credentials, API tokens, or exported environment values

Suggested layout:
- `.internals/domain.md`
- `.internals/releases.md`
- `.internals/ci.md`
- `.internals/research/`

Convention:
- treat `.internals/` as a scratchpad and memory layer
- keep it concise, factual, and repository-specific
- promote durable and broadly useful knowledge into tracked docs once it stabilizes

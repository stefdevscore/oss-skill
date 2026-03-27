# ADR-0001: Workspace Root Is a Container, Not a Git Repo

## Status

Accepted

## Context

The `open-source-software` directory is a local workspace that contains multiple independent projects. Some child folders are standalone repositories and some are plain source folders. Treating the workspace root as a Git repository creates misleading status, couples unrelated projects, and makes rename/migration work appear as cross-project deletions.

## Decision

The workspace root is not a repository. Each project folder decides independently whether it is versioned and how it is released.

## Consequences

- Git operations must be run from the actual project repository, not from the workspace root.
- Folders like `oss-skill` can exist as source directories without implying repository ownership.
- Cross-project coordination should be documented, not inferred from a shared root repo.
- Any future repo initialization at the workspace root would be an explicit exception and should be justified separately.

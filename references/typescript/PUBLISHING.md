# npm Publishing Reference

## Overview

This reference covers everything needed to publish a TypeScript package to npm — from `package.json` configuration through the `exports` map, pre-publish validation, scoped packages, and provenance.

## package.json — Essential Fields

```json
{
  "name": "my-package",
  "version": "1.0.0",
  "description": "A clear, one-line description",
  "type": "module",
  "main": "./dist/index.js",
  "types": "./dist/index.d.ts",
  "exports": {
    ".": {
      "types": "./dist/index.d.ts",
      "default": "./dist/index.js"
    }
  },
  "files": ["dist", "LICENSE", "README.md"],
  "engines": {
    "node": ">=20"
  },
  "scripts": {
    "build": "tsup",
    "prepublishOnly": "npm run build"
  },
  "keywords": ["keyword1", "keyword2"],
  "license": "MIT",
  "repository": {
    "type": "git",
    "url": "https://github.com/user/my-package.git"
  },
  "bugs": {
    "url": "https://github.com/user/my-package/issues"
  },
  "homepage": "https://github.com/user/my-package#readme"
}
```

### Field Reference

| Field | Purpose | Required |
|---|---|---|
| `name` | Package identifier on npm | Yes |
| `version` | SemVer version | Yes |
| `description` | Short description (shown in npm search) | Recommended |
| `type` | `"module"` for ESM, omit for CJS | Recommended |
| `main` | CJS entry point (legacy) | For CJS packages |
| `types` | TypeScript declaration entry | For TS packages |
| `exports` | Modern entry point map (takes precedence over `main`) | Recommended |
| `files` | Allowlist of files to include in the published package | Recommended |
| `engines` | Minimum Node.js version | Recommended |
| `license` | SPDX license identifier | Yes |
| `repository` | Link to source code | Recommended |
| `keywords` | Search terms | Recommended |

## The `exports` Map

The `exports` field is the modern way to define package entry points. It replaces `main`, `module`, and `types` at the top level.

### ESM-Only Package

```json
{
  "type": "module",
  "exports": {
    ".": {
      "types": "./dist/index.d.ts",
      "default": "./dist/index.js"
    }
  }
}
```

### Dual CJS/ESM Package

```json
{
  "exports": {
    ".": {
      "import": {
        "types": "./dist/index.d.mts",
        "default": "./dist/index.mjs"
      },
      "require": {
        "types": "./dist/index.d.cts",
        "default": "./dist/index.cjs"
      }
    }
  }
}
```

### Subpath Exports

Expose specific modules without exposing the entire `dist/`:

```json
{
  "exports": {
    ".": {
      "types": "./dist/index.d.ts",
      "default": "./dist/index.js"
    },
    "./utils": {
      "types": "./dist/utils/index.d.ts",
      "default": "./dist/utils/index.js"
    },
    "./package.json": "./package.json"
  }
}
```

### Rules

- **`types` must come first** in each condition block.
- **`default` must come last** — it's the fallback.
- Paths must start with `./`.
- If `exports` is defined, only exported paths are accessible to consumers.

## The `files` Field

Controls what gets included in the published package (allowlist):

```json
{
  "files": ["dist", "LICENSE", "README.md"]
}
```

### Always Included (Regardless of `files`)

- `package.json`
- `README.md` (any case)
- `LICENSE` / `LICENCE` (any case, any extension)
- `CHANGELOG` (any case, any extension)

### Always Excluded

- `node_modules/`
- `.git/`
- Files listed in `.npmignore` or `.gitignore`

### Validating Package Contents

```bash
# Preview what will be published
npm pack --dry-run

# Create the tarball without publishing
npm pack

# Inspect the tarball
tar -tzf my-package-1.0.0.tgz
```

## Scoped Packages

Scoped packages are namespaced under an organization: `@org/package-name`.

```json
{
  "name": "@my-org/my-package"
}
```

### Publishing Scoped Packages

Scoped packages are **private by default** on npm. To publish publicly:

```bash
npm publish --access public
```

Or set in `package.json`:

```json
{
  "publishConfig": {
    "access": "public"
  }
}
```

### Creating an npm Organization

1. Go to [npmjs.com/org/create](https://www.npmjs.com/org/create)
2. Choose a name (this becomes the `@org` scope)
3. Invite team members as needed

## npm Provenance

Provenance links a published package to its source commit and build workflow, providing supply-chain transparency.

### Requirements

- Package must be published from a supported CI environment (GitHub Actions, GitLab CI).
- The CI workflow must have `id-token: write` permission.

### GitHub Actions Setup

```yaml
permissions:
  contents: read
  id-token: write

steps:
  - run: npm publish --provenance --access public
    env:
      NODE_AUTH_TOKEN: ${{ secrets.NPM_TOKEN }}
```

The published package will show a "Provenance" badge on npmjs.com linking to the exact commit and workflow.

## Version Management

### Manual Versioning

```bash
npm version patch   # 1.0.0 → 1.0.1
npm version minor   # 1.0.0 → 1.1.0
npm version major   # 1.0.0 → 2.0.0
npm version 1.2.3   # explicit version
```

`npm version` updates `package.json`, creates a git commit, and tags it.

### Automated with Changesets

```bash
# During development — record a change
npx changeset
# Writes a markdown file to .changeset/ describing the change

# At release time — apply version bumps
npx changeset version
# Reads .changeset/ files, bumps versions, updates CHANGELOG.md

# Publish
npx changeset publish
# Runs npm publish and creates git tags
```

### Pre-release Versions

```bash
npm version 1.0.0-beta.1
npm publish --tag beta
```

Always use `--tag` for pre-releases to avoid overwriting the `latest` dist-tag.

## Pre-Publish Checklist

- [ ] `npm run build` succeeds
- [ ] `npm run typecheck` passes (no type errors)
- [ ] `npm test` passes
- [ ] `npm pack --dry-run` shows only expected files
- [ ] `package.json` has correct `name`, `version`, `description`, `license`
- [ ] `exports` map is correct (validate with [publint](https://publint.dev))
- [ ] Type declarations validate (check with [Are The Types Wrong?](https://arethetypeswrong.github.io))
- [ ] `README.md` contains installation and usage instructions
- [ ] `CHANGELOG.md` is updated for this version
- [ ] Git working directory is clean
- [ ] Version follows SemVer

## Dist-Tags

```bash
# Publish with a custom tag
npm publish --tag beta

# Add a tag to an existing version
npm dist-tag add my-package@1.0.0-beta.1 beta

# List tags
npm dist-tag ls my-package

# Remove a tag
npm dist-tag rm my-package beta
```

| Tag | Purpose |
|---|---|
| `latest` | Default install tag (`npm install my-package`) |
| `beta` | Pre-release testing |
| `next` | Upcoming major version |
| `canary` | Nightly/CI builds |

## Unpublishing and Deprecation

```bash
# Deprecate a version (soft — warns on install)
npm deprecate my-package@1.0.0 "Use 2.0.0 instead"

# Unpublish a specific version (hard — within 72h)
npm unpublish my-package@1.0.0

# Unpublish entire package (within 72h, if no dependents)
npm unpublish my-package --force
```

> **Prefer deprecation over unpublishing.** Unpublishing breaks dependents.

## Resources

- [npm docs — package.json](https://docs.npmjs.com/cli/configuring-npm/package-json)
- [Node.js — packages](https://nodejs.org/api/packages.html)
- [publint](https://publint.dev) — lint your package for common issues
- [Are The Types Wrong?](https://arethetypeswrong.github.io) — validate type exports
- [npm provenance](https://docs.npmjs.com/generating-provenance-statements) — supply-chain transparency

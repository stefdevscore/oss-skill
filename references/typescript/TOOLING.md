# TypeScript/JavaScript Tooling Reference

## 1. Overview

Recommended tooling for TypeScript/JavaScript open-source projects. Tools are grouped by function with opinionated recommendations for modern projects.

## 2. Dependency Management & Scripts

| Tool | Type | Best For |
|---|---|---|
| **tsc** | Type-checker + compiler | Type checking, `.d.ts` generation |
| **tsup** | Bundler (esbuild-based) | Libraries — produces CJS + ESM + `.d.ts` in one step |
| **esbuild** | Bundler/transpiler | Fast builds, no type checking |
| **rollup** | Bundler | Fine-grained control, tree-shaking |
| **unbuild** | Bundler (rollup-based) | Zero-config library builds, auto-infers from `package.json` |
| **swc** | Transpiler | Drop-in `tsc` replacement for speed (no type checking) |

### Recommended: `tsup`

Minimal config for a library:

```ts
// tsup.config.ts
import { defineConfig } from 'tsup';

export default defineConfig({
  entry: ['src/index.ts'],
  format: ['esm', 'cjs'],
  dts: true,
  clean: true,
  sourcemap: true,
});
```

```json
// package.json scripts
{
  "scripts": {
    "build": "tsup",
    "typecheck": "tsc --noEmit"
  }
}
```

### Build Strategy

1. **Use `tsc --noEmit`** for type checking only (CI and dev).
2. **Use `tsup` or `esbuild`** for fast compilation and bundling.
3. **Generate `.d.ts`** via `tsup --dts` or `tsc --emitDeclarationOnly`.

## 3. Testing

| Tool | Language | Style | Best For |
|---|---|---|---|
| **vitest** | JS/TS | Jest-compatible | Modern projects, fast, ESM-native |
| **jest** | JS/TS | BDD | Established projects, large ecosystem |
| **node:test** | JS | Built-in | Zero-dependency, Node.js-native |
| **playwright** | JS/TS | E2E | Browser and API testing |

### Recommended: `vitest`

```js
// vitest.config.mjs
import { defineConfig } from 'vitest/config';

export default defineConfig({
  test: {
    include: ['tests/**/*.test.{js,mjs}'],
  },
});
```

Example test file (`.mjs`):

```js
// tests/math.test.mjs
import { describe, it, expect } from 'vitest';
import { add } from '../src/math.ts';

describe('add', () => {
  it('should add two numbers', () => {
    expect(add(1, 2)).toBe(3);
  });
});
```

### Using `node:test` (Zero Dependencies)

```js
// tests/math.test.mjs
import { describe, it } from 'node:test';
import assert from 'node:assert/strict';
import { add } from '../dist/index.js';

describe('add', () => {
  it('should add two numbers', () => {
    assert.strictEqual(add(1, 2), 3);
  });
});
```

Run with: `node --test tests/`

### npm Scripts Pattern

```json
{
  "scripts": {
    "test": "vitest run",
    "test:watch": "vitest",
    "test:coverage": "vitest run --coverage"
  }
}
```

## 4. Linting & Formatting

### ESLint (Flat Config)

Modern ESLint uses flat config (`eslint.config.mjs`):

```js
// eslint.config.mjs
import eslint from '@eslint/js';
import tseslint from 'typescript-eslint';

export default tseslint.config(
  eslint.configs.recommended,
  ...tseslint.configs.strictTypeChecked,
  {
    languageOptions: {
      parserOptions: {
        projectService: true,
        tsconfigRootDir: import.meta.dirname,
      },
    },
  },
  {
    ignores: ['dist/', 'node_modules/'],
  },
);
```

### Key Packages

- `@eslint/js` — core ESLint rules
- `typescript-eslint` — TypeScript parser and rules (v8+ unified package)

```json
{
  "scripts": {
    "lint": "eslint .",
    "lint:fix": "eslint . --fix"
  }
}
```

### Formatting Logic

| Tool | Approach |
|---|---|
| **Prettier** | Opinionated, widely adopted, large plugin ecosystem |
| **Biome** | All-in-one (lint + format), fast (Rust-based), newer |

### Prettier

```json
// .prettierrc
{
  "semi": true,
  "singleQuote": true,
  "trailingComma": "all",
  "printWidth": 100
}
```

```json
{
  "scripts": {
    "format": "prettier --write .",
    "format:check": "prettier --check ."
  }
}
```

### Biome (Alternative)

```json
// biome.json
{
  "formatter": { "indentStyle": "space", "indentWidth": 2 },
  "linter": { "enabled": true }
}
```

```json
{
  "scripts": {
    "check": "biome check .",
    "check:fix": "biome check --write ."
  }
}
```

### Package Managers

| Manager | Lockfile | Workspaces | Speed | Disk Usage |
|---|---|---|---|---|
| **npm** | `package-lock.json` | Yes | Moderate | Moderate |
| **pnpm** | `pnpm-lock.yaml` | Yes | Fast | Low (content-addressable) |
| **yarn** (v4) | `yarn.lock` | Yes | Fast | Moderate |

**Recommendation**: Use `pnpm` for monorepos and disk efficiency. Use `npm` for simplicity and maximum compatibility.

## 6. CI/CD (GitHub Actions)

### Basic CI Workflow

```yaml
# .github/workflows/ci.yml
name: CI
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  check:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [20, 22]
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}
          cache: npm
      - run: npm ci
      - run: npm run typecheck
      - run: npm run lint
      - run: npm run format:check
      - run: npm test
```

### Release Workflow

```yaml
# .github/workflows/release.yml
name: Release
on:
  push:
    tags: ['v*']

jobs:
  publish:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      id-token: write  # required for provenance
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 22
          registry-url: https://registry.npmjs.org
      - run: npm ci
      - run: npm run build
      - run: npm publish --provenance --access public
        env:
          NODE_AUTH_TOKEN: ${{ secrets.NPM_TOKEN }}
```

## 5. Documentation

| Tool | Output | Best For |
|---|---|---|
| **TypeDoc** | HTML/Markdown | API reference from TSDoc comments |
| **TSDoc** | Standard | Comment syntax (TypeDoc, API Extractor consume it) |
| **Starlight** (Astro) | Static site | Full documentation sites |
| **VitePress** | Static site | Vue-flavored doc sites |

### TypeDoc Setup

```json
{
  "scripts": {
    "docs": "typedoc --out docs/api src/index.ts"
  }
}
```

TSDoc comment style:

```ts
/**
 * Adds two numbers.
 *
 * @param a - The first number
 * @param b - The second number
 * @returns The sum of `a` and `b`
 *
 * @example
 * ```ts
 * add(1, 2); // 3
 * ```
 */
export function add(a: number, b: number): number {
  return a + b;
}
```

### Release Management

| Tool | Approach | Best For |
|---|---|---|
| **changesets** | Manual changelog + version bumps via PR | Monorepos, team-managed releases |
| **semantic-release** | Fully automated from commit messages | Solo/small teams, conventional commits |
| **np** | Interactive CLI for publishing | Simple single-package projects |
| **release-please** | Automated release PRs | Google-style release workflow |

### Changesets Workflow

```bash
# Add a changeset (interactive)
npx changeset

# Version packages (bumps versions, updates changelogs)
npx changeset version

# Publish to npm
npx changeset publish
```

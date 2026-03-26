# TypeScript Project Reference

## Overview

This reference covers TypeScript-specific project setup, configuration, and patterns for open-source libraries and tools. The assumed workflow is **TypeScript for source code** and **JavaScript (`.mjs` / `.js`) for tests, scripts, and config files**.

## Project Structure

```
project-root/
├── src/                    # TypeScript source
│   ├── index.ts            # main entry point
│   └── utils/
├── tests/                  # JavaScript tests (.mjs or .js)
│   ├── index.test.mjs
│   └── utils.test.mjs
├── dist/                   # compiled output (gitignored)
│   ├── index.js
│   ├── index.d.ts
│   └── index.js.map
├── scripts/                # dev/build scripts (.mjs)
├── tsconfig.json
├── tsconfig.build.json     # extends tsconfig, excludes tests
├── package.json
├── .gitignore
├── LICENSE
└── README.md
```

### Key Conventions

- **`src/`** — all TypeScript source files (`.ts`, `.tsx`).
- **`tests/`** — test files in JavaScript (`.mjs` or `.js`). Tests import from the built `dist/` or use a test runner that resolves TS directly.
- **`dist/`** — build output, always gitignored, included in npm package via `files`.
- **`scripts/`** — automation scripts in `.mjs` for Node.js ESM compatibility.

## tsconfig.json

### Recommended Base for Libraries

```jsonc
{
  "compilerOptions": {
    // Type checking
    "strict": true,
    "noUncheckedIndexedAccess": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "exactOptionalPropertyTypes": true,

    // Module system
    "module": "NodeNext",
    "moduleResolution": "NodeNext",
    "target": "ES2022",

    // Output
    "outDir": "dist",
    "rootDir": "src",
    "declaration": true,
    "declarationMap": true,
    "sourceMap": true,

    // Interop
    "esModuleInterop": true,
    "forceConsistentCasingInImports": true,
    "isolatedModules": true,
    "verbatimModuleSyntax": true,

    // Other
    "skipLibCheck": true,
    "resolveJsonModule": true
  },
  "include": ["src"],
  "exclude": ["node_modules", "dist"]
}
```

### Build-Only Config

Create `tsconfig.build.json` to exclude test helpers and scripts:

```jsonc
{
  "extends": "./tsconfig.json",
  "exclude": ["node_modules", "dist", "tests", "scripts", "**/*.test.*", "**/*.spec.*"]
}
```

### Key Compiler Options Explained

| Option | Purpose |
|---|---|
| `strict` | Enables all strict type-checking flags |
| `module: "NodeNext"` | Matches Node.js native ESM/CJS resolution |
| `declaration` | Generates `.d.ts` type declaration files |
| `declarationMap` | Maps declarations back to source for IDE go-to-definition |
| `sourceMap` | Debug compiled code back to TypeScript source |
| `verbatimModuleSyntax` | Enforces explicit `import type` for type-only imports |
| `isolatedModules` | Ensures compatibility with single-file transpilers (esbuild, swc) |

## Module Systems

### ESM-First (Recommended)

Set `"type": "module"` in `package.json`. All `.js` files are treated as ESM. Use `.cjs` extension for any CommonJS files.

```json
{
  "type": "module"
}
```

### File Extensions

| Extension | Module System | Use Case |
|---|---|---|
| `.ts` | Determined by `tsconfig` | TypeScript source |
| `.mts` | Always ESM | TypeScript ESM (explicit) |
| `.cts` | Always CJS | TypeScript CJS (explicit) |
| `.mjs` | Always ESM | JavaScript ESM (tests, scripts, config) |
| `.cjs` | Always CJS | JavaScript CJS (legacy config, e.g. `jest.config.cjs`) |
| `.js` | Follows `"type"` field | Ambiguous — prefer `.mjs`/`.cjs` for clarity |

### Dual CJS/ESM Packages

If you must support both module systems, use the `exports` map in `package.json`:

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

Use a bundler like `tsup` to produce both outputs from a single source.

## Type Declarations

### Generating `.d.ts` Files

Set `"declaration": true` in `tsconfig.json`. TypeScript produces `.d.ts` files alongside `.js` output in `dist/`.

### package.json `types` Field

```json
{
  "types": "./dist/index.d.ts"
}
```

Or with the `exports` map (preferred):

```json
{
  "exports": {
    ".": {
      "types": "./dist/index.d.ts",
      "default": "./dist/index.js"
    }
  }
}
```

> **Order matters.** The `types` condition must come first in the `exports` map.

### Shipping Types for Subpath Exports

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
    }
  }
}
```

## Path Aliases

### Setting Up in tsconfig.json

```jsonc
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"]
    }
  }
}
```

### Build-Time Resolution

`tsc` does **not** rewrite path aliases in output. Use one of:

- **tsup / esbuild** — resolves aliases automatically.
- **tsc-alias** — post-compilation rewriting: `tsc && tsc-alias`.
- **`imports`** field in `package.json` (Node.js native subpath imports):

```json
{
  "imports": {
    "#*": "./src/*"
  }
}
```

This is resolved natively by Node.js and requires no build step.

## TypeScript with JavaScript Tests

### Why JS for Tests

- Tests run against the **compiled output** or via a TS-aware runner — no need for TS in tests.
- `.mjs` test files work natively with Node.js ESM without extra configuration.
- Avoids type-checking test files during the production build.

### Pattern: Importing from Package Name

Configure `exports` in `package.json` and import from the package name in tests:

```js
// tests/index.test.mjs
import { myFunction } from '../dist/index.js';
// or if using a test runner with module resolution:
// import { myFunction } from 'my-package';
```

### Pattern: Using a TS-Aware Test Runner

Runners like `vitest` can import `.ts` files directly, even in `.mjs` test files:

```js
// tests/index.test.mjs (vitest resolves TS source)
import { myFunction } from '../src/index.ts';
```

## Resources

- [TypeScript Handbook](https://www.typescriptlang.org/docs/handbook/) — official docs
- [tsconfig reference](https://www.typescriptlang.org/tsconfig) — all compiler options
- [Node.js ESM documentation](https://nodejs.org/api/esm.html) — native ESM in Node.js
- [Are The Types Wrong?](https://arethetypeswrong.github.io) — validate your package's type exports
- [publint](https://publint.dev) — lint your package for common publishing issues

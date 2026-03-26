# Open-Source Licensing Reference

## 1. Overview

An open-source license defines how others may use, modify, and distribute your software. Choosing the right license is one of the most important decisions when releasing an OSS project.

> **Not legal advice.** This reference provides general guidance. Consult a qualified attorney for specific licensing questions.

## 2. License Categories & Matrix

### Permissive Licenses

Allow almost unrestricted use, including incorporation into proprietary software. Minimal obligations beyond attribution.

### Copyleft Licenses

Require derivative works to be released under the same (or compatible) license. Ensures the software and its derivatives remain open.

### Public Domain Dedications

Waive all copyright, placing the work in the public domain. Maximally permissive but may not be legally recognized in all jurisdictions.

## License Comparison Matrix

| License | Type | Attribution Required | Patent Grant | Copyleft | Commercial Use | File-Level Copyleft |
|---|---|---|---|---|---|---|
| **MIT** | Permissive | Yes | No | No | Yes | No |
| **Apache-2.0** | Permissive | Yes | Yes | No | Yes | No |
| **BSD-2-Clause** | Permissive | Yes | No | No | Yes | No |
| **BSD-3-Clause** | Permissive | Yes | No | No | Yes | No |
| **ISC** | Permissive | Yes | No | No | Yes | No |
| **MPL-2.0** | Weak copyleft | Yes | Yes | File-level | Yes | Yes |
| **LGPL-3.0** | Weak copyleft | Yes | Yes | Library-level | Yes | No |
| **GPL-3.0** | Strong copyleft | Yes | Yes | Yes | Yes* | No |
| **AGPL-3.0** | Network copyleft | Yes | Yes | Yes (+ network) | Yes* | No |
| **Unlicense** | Public domain | No | No | No | Yes | No |

*GPL/AGPL allow commercial use, but derivative works must also be GPL/AGPL-licensed.

## 3. Decision Guide & Application (How-to)

```
Do you want maximum adoption with minimal friction?
├─ Yes → MIT or Apache-2.0
│   ├─ Need explicit patent protection? → Apache-2.0
│   └─ Prefer simplicity? → MIT
│
├─ Do you want derivatives to stay open?
│   ├─ Only modified files? → MPL-2.0
│   ├─ Entire linked work? → GPL-3.0
│   └─ Including network/SaaS use? → AGPL-3.0
│
└─ Want to dedicate to public domain?
    └─ Unlicense (or CC0 for non-software)
```

## How to Apply a License

1. **Add a `LICENSE` file** at the repository root with the full license text.
2. **Add the `license` field** to your package manifest:
   - `package.json`: `"license": "MIT"`
   - `pyproject.toml`: `license = "MIT"`
   - `Cargo.toml`: `license = "MIT"`
3. **Add SPDX headers** to source files (optional but recommended for copyleft licenses):
   ```
   // SPDX-License-Identifier: MIT
   ```
4. **State the license in README.md** with a badge or a dedicated section.

## 4. Multi-Licensing & Agreements (CLAs)

Some projects offer multiple licenses to accommodate different use cases:

- **Dual license (e.g., MIT OR Apache-2.0)**: common in Rust ecosystem; users choose whichever suits them.
- **Commercial + OSS (e.g., AGPL-3.0 + commercial)**: open-source for community use, paid license for proprietary use.

Express dual licensing in SPDX notation: `MIT OR Apache-2.0`.

## Contributor License Agreements (CLAs)

- A CLA ensures contributors grant you sufficient rights to distribute their contributions under the project's license.
- Alternatives: Developer Certificate of Origin (DCO) is lighter-weight and increasingly preferred.
- Tools: CLA Assistant, DCO bot.

## 5. Resources

- [choosealicense.com](https://choosealicense.com) — GitHub's license chooser
- [SPDX License List](https://spdx.org/licenses/) — canonical license identifiers
- [OSI Approved Licenses](https://opensource.org/licenses/) — Open Source Initiative list
- [tl;drLegal](https://tldrlegal.com) — plain-language license summaries

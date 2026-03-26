# Distribution Reference

## 1. Overview

Distribution is how your open-source software reaches users. Choosing the right channels and following best practices for versioning and releases ensures your software is discoverable, installable, and trustworthy.

## 2. Channels (Registries & System Package Managers)

| Ecosystem | Registry | Publish Command | Manifest File |
|---|---|---|---|
| **JavaScript/TypeScript** | [npm](https://www.npmjs.com) | `npm publish` | `package.json` |
| **Python** | [PyPI](https://pypi.org) | `twine upload dist/*` | `pyproject.toml` |
| **Rust** | [crates.io](https://crates.io) | `cargo publish` | `Cargo.toml` |
| **Java/Kotlin** | [Maven Central](https://central.sonatype.com) | Maven/Gradle publish | `pom.xml` / `build.gradle` |
| **Go** | [Go Module Proxy](https://proxy.golang.org) | `git tag` + module path | `go.mod` |
| **Ruby** | [RubyGems](https://rubygems.org) | `gem push` | `*.gemspec` |
| **.NET** | [NuGet](https://www.nuget.org) | `dotnet nuget push` | `*.csproj` |
| **PHP** | [Packagist](https://packagist.org) | Auto-sync from Git | `composer.json` |

## Other Distribution Channels

### Container Registries

- **Docker Hub** — `docker push`
- **GitHub Container Registry (ghcr.io)** — `docker push ghcr.io/user/image`
- Best for applications, services, and tools with complex dependencies.

### Binary Releases

- **GitHub Releases** — attach pre-built binaries to tagged releases.
- **GitLab Releases** — equivalent for GitLab-hosted projects.
- Cross-compile for multiple OS/arch targets (e.g., using GoReleaser, cross-rs, pkg).

### System Package Managers

- **Homebrew** (macOS/Linux) — create a Formula or tap.
- **apt / deb** (Debian/Ubuntu) — build `.deb` packages, host a PPA or use Launchpad.
- **dnf / rpm** (Fedora/RHEL) — build `.rpm` packages, use COPR.
- **Snapcraft / Flatpak** — cross-distro Linux packaging.
- **winget / Chocolatey / Scoop** (Windows) — Windows package managers.

## 3. Versioning Semantics (SemVer)

Follow [semver.org](https://semver.org) for version numbers: `MAJOR.MINOR.PATCH`

| Increment | When |
|---|---|
| **MAJOR** | Incompatible API changes (breaking changes) |
| **MINOR** | New functionality, backwards-compatible |
| **PATCH** | Bug fixes, backwards-compatible |

### Pre-release Versions

Use pre-release identifiers for unstable releases:
- `1.0.0-alpha.1` → early testing
- `1.0.0-beta.1` → feature-complete, testing
- `1.0.0-rc.1` → release candidate

### Initial Development

- `0.x.y` signals the API is unstable and may change at any time.
- Once stable, release `1.0.0`.

## 4. Changelogs & Release Automation

Follow [Keep a Changelog](https://keepachangelog.com):

```markdown
## [1.2.0] - 2025-03-15

### Added
- New `--verbose` flag for detailed output.

### Fixed
- Resolved crash when config file is missing.

### Changed
- Default timeout increased from 5s to 10s.

### Deprecated
- `--legacy-mode` flag will be removed in 2.0.0.
```

Group changes under: `Added`, `Changed`, `Deprecated`, `Removed`, `Fixed`, `Security`.

## Release Automation

### GitHub Actions Example

```yaml
name: Release
on:
  push:
    tags: ['v*']
jobs:
  publish:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
          registry-url: https://registry.npmjs.org
      - run: npm ci
      - run: npm publish
        env:
          NODE_AUTH_TOKEN: ${{ secrets.NPM_TOKEN }}
```

### Recommended Tools

- **semantic-release** — fully automated versioning and publishing based on commit messages.
- **changesets** — manage versioning and changelogs for monorepos.
- **GoReleaser** — cross-compile and release Go binaries.
- **release-please** — Google's automated release PR tool.

## 5. Distribution Checklist

- [ ] Package manifest has complete metadata (name, version, description, license, repository URL)
- [ ] README includes installation instructions for each distribution channel
- [ ] Version follows SemVer
- [ ] CHANGELOG is up to date
- [ ] CI/CD automates testing before publish
- [ ] Release is tagged in Git
- [ ] Pre-built binaries are available for relevant platforms (if applicable)
- [ ] Registry access tokens are stored as secrets, never committed

## 6. Resources

- [semver.org](https://semver.org)
- [Keep a Changelog](https://keepachangelog.com)
- [release-please](https://github.com/googleapis/release-please)

# Research & Expansion Roadmap

This document outlines the planned research and future expansions for `oss-skill`. As the open-source ecosystem evolves, this skill should expand to cover new languages, emerging tooling, and advanced sustainability patterns without collapsing into narrow tool prescriptions.

## 1. Planned Ecosystem Layers

### 1. Go (Golang)
- **Tooling**: `go mod`, `go build`, `go test`, `golangci-lint`.
- **Distribution**: The Go Module Proxy (`proxy.golang.org`), pseudo-versions, and `v2+` module pathing.
- **Publishing**: Tagging releases for the Go proxy, use of `goreleaser` for cross-compiling binaries and distributing via Homebrew taps and container registries.

### 2. C / C++
- **Tooling**: CMake, Make, Ninja, Catch2/GTest.
- **Dependency Management**: Conan, vcpkg.
- **Publishing**: Distributing source vs. pre-compiled binaries, ABI compatibility challenges.

### 3. Java / Kotlin
- **Tooling**: Maven, Gradle.
- **Publishing**: Maven Central requirements (PGP signing, javadoc, sources), Sonatype OSSRH.

## 2. Advanced Topics to Research

### Monorepo Tooling
- Deep dive into managing multi-package OSS repositories.
- **JS/TS**: Turborepo, Nx, Lerna (legacy), pnpm workspaces.
- **Rust**: Cargo workspaces plus workspace-aware release orchestration.
- **Python**: `pdm` workspaces, `pants` build system.

### Advanced Community & Governance
- **Incubation pathways**: Documenting the exact processes for donating a project to the CNCF (Cloud Native Computing Foundation) or the Apache Software Foundation.
- **Community Health Metrics**: How to use CHAOSS metrics, DevStats, and GitHub Insights to measure contributor churn, bus factor, and issue resolution time.
- **Triage Automation**: AI-assisted triage, GitHub Actions for stale issues, `probot` applications.

### Sustainability & Economics
- **Pricing Open Core**: Strategies for separating OSS "core" features from enterprise "premium" features.
- **Sponsorship ROI**: How to structure GitHub Sponsors tiers (e.g., logo placement, priority support, private roadmap access).
- **Grant Applications**: Identifying and applying for grants from organizations like the Sovereign Tech Fund, NLnet, and the Ford Foundation.

## 3. Research Methodology

To implement the above, we will:
1. **Analyze top repositories** in each target language to determine common tooling and packaging patterns.
2. **Review foundation requirements** directly from CNCF and Apache graduation guidelines.
3. **Monitor package registries** for new security, provenance, and publishing capabilities and update the publishing guides accordingly.

## 4. Contributing

If you have expertise in a specific ecosystem not covered here (e.g., Ruby, .NET, Elixir), add a new layer following the `PROJECT.md`, `TOOLING.md`, `PUBLISHING.md` pattern.

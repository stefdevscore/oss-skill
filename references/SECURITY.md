# Security & Supply Chain Reference

## 1. Overview

Security is paramount for open-source projects. Maintainers are responsible for protecting their users from vulnerabilities and ensuring the integrity of the software artifacts they distribute.

## 2. Supply Chain Integrity

Attackers often target the tools used to build and distribute software rather than the source code itself.

### SLSA (Supply-chain Levels for Software Artifacts)
- Follow [slsa.dev](https://slsa.dev) guidelines to protect against tampering.
- Use build provenance (e.g., GitHub Actions' native provenance) to link artifacts to source commits.

### SBOM (Software Bill of Materials)
- Generate an SBOM for every release to provide a machine-readable list of all components and dependencies.
- Tools: `syft`, `cyclonedx-cli`.

### Signed Artifacts
- Sign commits and releases with SSH or GPG keys.
- Use **Sigstore / Cosign** for keyless signing of container images and binaries.

## 3. Dependency Management

Most security issues in OSS come from third-party dependencies.

### Auditing
- Regularly run audit commands:
  - **TypeScript:** `npm audit` or `pnpm audit`
  - **Python:** `pip-audit`
  - **Rust:** `cargo audit` (via `cargo-audit` crate)
- Automate audits in CI to fail builds on critical/high vulnerabilities.

### Pinning Versions
- Use lock files (`package-lock.json`, `uv.lock`, `Cargo.lock`) to ensure deterministic builds.
- Use tools like **Dependabot** or **Renovate** to keep dependencies updated safely.

## 4. Vulnerability Disclosure (SECURITY.md)

Provide a clear pathway for researchers to report vulnerabilities privately.

### Responsible Disclosure Process
1.  **Reporting:** Provide a private way to report (email, GitHub Private Vulnerability Reporting).
2.  **Acknowledgment:** Aim to respond within 24-48 hours.
3.  **Triage:** Verify the vulnerability and determine the severity (CVSS score).
4.  **Fix:** Develop and test a fix privately.
5.  **Release:** Publish a patched version.
6.  **Public Disclosure:** Publish a security advisory (CVE) describing the issue.

### GitHub Security Advisories
- Use GitHub's built-in [Security Advisories](https://docs.github.com/en/code-security/security-advisories/repository-security-advisories) to collaborate on fixes privately and request a CVE.

## 5. Best Practices

- **Minimal Permissions:** Use the principle of least privilege for CI/CD tokens.
- **Secret Scanning:** Use GitHub Secret Scanning to prevent accidental commits of tokens/keys.
- **Static Analysis (SAST):** Use tools like **CodeQL** or **SonarQube** to find bugs and security patterns in source code.
- **Binary Provenance:** Ensure binaries (e.g., wheels, npm tarballs) are built in a verifiable environment (e.g., using GitHub Actions' `id-token: write`).

## 6. Resources

- [OpenSSF Best Practices Badge](https://bestpractices.coreinfrastructure.org/)
- [SLSA.dev](https://slsa.dev)
- [Sigstore](https://www.sigstore.dev/)
- [GitHub Security Documentation](https://docs.github.com/en/code-security)
- [CISA — Securing the Software Supply Chain](https://www.cisa.gov/resources-tools/resources/securing-software-supply-chain)

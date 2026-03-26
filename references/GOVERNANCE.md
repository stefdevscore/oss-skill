# Governance Reference

## Overview

Governance defines how decisions are made, who has authority, and how contributors advance within a project. The right model depends on the project's size, community, and goals.

## Governance Models

### BDFL (Benevolent Dictator for Life)

- **How it works**: a single person has final say on all decisions.
- **Best for**: small projects, early-stage projects, projects with a strong visionary.
- **Examples**: Python (historically), Linux kernel (Linus Torvalds).
- **Risks**: bus factor of 1, community friction if the BDFL becomes unresponsive or controversial.

### Meritocracy / Maintainer Council

- **How it works**: contributors earn commit access and decision-making power through sustained, quality contributions.
- **Best for**: mid-size projects with multiple active contributors.
- **Examples**: Node.js, many Apache projects.
- **Risks**: can become exclusionary if advancement criteria are unclear.

### Committee / Technical Steering Committee (TSC)

- **How it works**: a small elected or appointed group makes decisions, often by consensus or vote.
- **Best for**: larger projects with diverse stakeholders.
- **Examples**: Node.js TSC, Kubernetes Steering Committee.
- **Risks**: slow decision-making, political dynamics.

### Foundation-Backed

- **How it works**: a legal entity (foundation) provides governance structure, funding, and legal protection.
- **Best for**: large, critical-infrastructure projects.
- **Examples**: Apache Foundation, Linux Foundation, OpenJS Foundation, CNCF, Eclipse Foundation.
- **Risks**: bureaucratic overhead, may not suit small projects.

## Governance Comparison

| Model | Decision Speed | Scalability | Bus Factor | Overhead |
|---|---|---|---|---|
| BDFL | Fast | Low | 1 | Minimal |
| Meritocracy | Medium | Medium | Medium | Low |
| Committee/TSC | Slow–Medium | High | High | Medium |
| Foundation | Slow | Very High | Very High | High |

## Community Health Files

### CODE_OF_CONDUCT.md

- Sets behavioral expectations for all participants.
- **Recommended**: [Contributor Covenant](https://www.contributor-covenant.org) (most widely adopted).
- Should specify enforcement procedures and contact information.

### CONTRIBUTING.md

Essential sections:

1. **Getting started** — development environment setup
2. **How to report bugs** — issue template, expected information
3. **How to suggest features** — discussion process
4. **How to submit changes** — branch naming, commit style, PR process
5. **Code standards** — linting, formatting, testing requirements
6. **Review process** — who reviews, expected turnaround, merge criteria

### SECURITY.md

- Provide a responsible disclosure process (email, security advisory form).
- State expected response time (e.g., acknowledge within 48 hours).
- Reference a PGP key for encrypted communication if appropriate.
- Link to any bug bounty program.

### Issue and PR Templates

Place in `.github/ISSUE_TEMPLATE/` and `.github/PULL_REQUEST_TEMPLATE.md`:

- **Bug report template**: steps to reproduce, expected vs. actual behavior, environment details.
- **Feature request template**: problem statement, proposed solution, alternatives considered.
- **PR template**: description of changes, related issues, testing done, checklist.

## Contributor Roles

A common progression:

| Role | Permissions | Earned By |
|---|---|---|
| **User** | Open issues, comment | Using the project |
| **Contributor** | Submit PRs | Having a PR merged |
| **Triager** | Label/close issues | Sustained helpful triage |
| **Committer** | Merge PRs | Consistent quality contributions |
| **Maintainer** | Release, admin | Long-term stewardship and trust |

## Sustainability

### Funding Models

| Model | Description | Examples |
|---|---|---|
| **Sponsorships** | Individual/corporate sponsors via platforms | GitHub Sponsors, Open Collective, Patreon |
| **Grants** | One-time or recurring grants from foundations | Sovereign Tech Fund, NLnet, Ford Foundation |
| **Dual licensing** | OSS license + commercial license for proprietary use | MongoDB (SSPL + commercial), Qt (GPL + commercial) |
| **Open core** | Core is OSS, premium features are paid | GitLab, Grafana |
| **Paid support** | Free software, paid support/consulting | Red Hat model |
| **SaaS** | Host and operate the OSS as a service | WordPress.com, Sentry |

### Reducing Bus Factor

- Document all processes and decisions publicly.
- Share maintainer responsibilities among multiple people.
- Use team-based code review rather than single-maintainer gatekeeping.
- Store credentials and secrets in shared, auditable systems (1Password Teams, etc.).
- Write a succession plan for key maintainers.

### Maintainer Wellbeing

- Set clear expectations for response times and availability.
- It is okay to say no or defer work.
- Use automation (bots, CI) to reduce repetitive burden.
- Take breaks and communicate availability to the community.

## Resources

- [opensource.guide](https://opensource.guide) — GitHub's comprehensive OSS guides
- [Contributor Covenant](https://www.contributor-covenant.org) — code of conduct standard
- [TODO Group](https://todogroup.org) — open-source program office resources
- [Open Source Initiative](https://opensource.org) — OSI governance and license stewardship
- [CHAOSS](https://chaoss.community) — community health analytics

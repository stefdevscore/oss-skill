---
name: ecosystem-gap-analysis
description: Generic research prompts for identifying high-leverage gaps in open-source ecosystems.
---

# Ecosystem Gap Analysis

This document is a neutral research scratchpad for spotting underserved areas in open-source ecosystems. It is intentionally product-agnostic and should help future maintainers evaluate gaps without anchoring the skill to one historical project.

## Gap Identification Heuristics

1. **Workflow Compression**
   Find tasks that currently require several tools, brittle shell pipelines, or excessive manual glue and ask whether they can be reduced to one clean, auditable workflow.

2. **Legacy Replacement**
   Look for widely used but poorly maintained, over-dependent, or slow packages that solve a narrow problem with disproportionate complexity.

3. **Cross-Ecosystem Demand**
   Favor problems that recur across JavaScript, Python, Rust, and other ecosystems so guidance can transfer across language boundaries.

4. **Automation Readiness**
   Prefer tools and patterns that expose stable machine-readable outputs, explicit error modes, and predictable install/runtime behavior.

5. **Distribution Leverage**
   Prioritize ideas where packaging, provenance, portability, or offline distribution create real user value rather than novelty alone.

## Evaluation Dimensions

| Dimension | Question |
|---|---|
| Problem Sharpness | Is the user pain concrete and recurring? |
| Simplicity Gain | Does the new approach remove meaningful complexity? |
| Maintenance Burden | Can the project be kept healthy by a small team? |
| Portability | Can the concept travel across ecosystems or platforms? |
| Supply-Chain Posture | Does the solution improve trust, auditability, or installability? |

## Example Research Tracks

- Documentation generation and publishing workflows
- Codebase indexing and structural analysis
- Environment/configuration validation
- Asset optimization and packaging
- Release automation and provenance verification
- Cross-platform binary distribution

## Next-Step Questions

- Which gaps are currently solved only by large, fragile, or ecosystem-specific tools?
- Which categories would benefit most from clearer packaging and installability guidance?
- Which opportunities can be expressed as durable OSS patterns rather than one-off product ideas?

# 06 — Software Supply Chain

> Integrity of code, dependencies, build artifacts, and the pipelines that produce them. SLSA-aligned.

---

## Advisory Gist

**TL;DR.** Pin actions by SHA, sign artifacts (Sigstore / `attest-build-provenance`), generate SBOMs at build, enable Dependabot + dependency review on rulesets, restrict workflows to vetted reusable workflows. Aim at SLSA L3 for production paths.

**Decisions you will be asked to make**

- Allowed actions policy (allow-list vs verified-creators-only).
- SBOM format (SPDX vs CycloneDX) and where it's stored.
- Artifact signing model (keyless OIDC vs KMS-backed).
- Dependabot auto-merge policy.
- Internal action / reusable-workflow registry.

**Top edges**

- `@v1` pins resolve to mutable refs — supply chain blind spot.
- Custom actions in private repos still need SHA pinning + review.
- Dependabot at scale floods PRs — batching + auto-merge needs ruleset rigour.
- SBOMs are valueless without a downstream consumer (vuln scanner / VRM).

**Connects to**

- [04 GHAS](../04-security-ghas/README.md) — dependency review, secret scanning at build.
- [07 GitHub Actions](../07-github-actions/README.md) — reusable workflows, OIDC.
- [08 CI/CD & Infrastructure](../08-cicd-and-infrastructure/README.md) — deployment provenance.
- [17 Additional Services](../17-additional-services/README.md) — Packages / GHCR.

**Customer-fit questions**

- Which SLSA level does the customer's regulator or buyer require?
- Who consumes the SBOM (and have they confirmed format)?
- What is the customer's policy on third-party actions today — governed or open?

---

## Overview

GitHub's native supply-chain stack:

- **Dependency graph** (foundation for Dependabot + SBOM)
- **SBOM export** (SPDX 2.3 via API)
- **Artifact attestations** (SLSA provenance, signed via OIDC)
- **Sigstore / cosign** integration for keyless signing
- **Trusted publishing** (OIDC → PyPI / npm / Maven Central / RubyGems)
- **Dependency review action** (block vulnerable deps in PRs)
- **GHCR + cosign** for container image signing
- **Required workflows** for enforced security CI

Pair with [04-security-ghas](../04-security-ghas/README.md) for detection.

---

## Configuration

- Enable **dependency graph** at org level.
- Generate SBOM in release pipelines via `gh api` or `actions/sbom-action` analogues.
- Use `actions/attest-build-provenance` for SLSA Build L3 attestations.
- Configure **cosign** with GitHub OIDC for keyless container signing.
- Set up **OIDC trusted publishing** to registries — eliminates `NPM_TOKEN`, `PYPI_API_TOKEN`, etc.
- Add `actions/dependency-review-action` to PR workflows.
- Pin all Actions to **commit SHAs**, not tags.

---

## Usage

- SBOMs generated at build time, stored with the release artifact, and **consumed** downstream (VMS / CVE matching).
- Attestations **verified** at deployment time (`gh attestation verify` or registry-side).
- Trusted publishing replaces all long-lived registry tokens.
- Required workflows enforce SBOM + attestation generation on every release.

---

## Best Practices

- Standardize on **one** SBOM format (SPDX or CycloneDX) enterprise-wide.
- Pin Actions to SHAs — single highest-impact supply-chain control.
- Require **signed commits** on repos feeding production builds.
- Attestation **verification** at deploy gate is mandatory — generation alone is theater.
- Define your **target SLSA level** before designing the pipeline.

---

## Common Pitfalls

- SBOMs generated, never consumed.
- Attestations generated, never verified.
- Trusted publishing partial adoption — old PATs left active.
- Dependency graph missing files in monorepos.
- Mutable container tags (`:latest`) in production pulls.

---

## Implementation Notes

- **SLSA L2** achievable with native attestations on Actions.
- **SLSA L3** needs additional build-environment isolation.
- For containers: native attestation + cosign + GHCR digest pulls.
- Include open-source dependency stewardship in the program (see [GitHub Sponsors](../17-additional-services/12-sponsors.md)).

---

## Sources

- [Dependency graph](https://docs.github.com/en/code-security/supply-chain-security/understanding-your-software-supply-chain/about-the-dependency-graph)
- [Exporting an SBOM for your repository](https://docs.github.com/en/code-security/supply-chain-security/understanding-your-software-supply-chain/exporting-a-software-bill-of-materials-for-your-repository)
- [Using artifact attestations](https://docs.github.com/en/actions/security-for-github-actions/using-artifact-attestations/using-artifact-attestations-to-establish-provenance-for-builds)
- [actions/attest-build-provenance](https://github.com/actions/attest-build-provenance)
- [Sigstore / cosign](https://docs.sigstore.dev/cosign/overview/)
- [About OIDC for trusted publishing (PyPI)](https://docs.pypi.org/trusted-publishers/)
- [Configuring OpenID Connect in npm](https://docs.npmjs.com/generating-provenance-statements)
- [About security hardening with OpenID Connect (Actions)](https://docs.github.com/en/actions/security-for-github-actions/security-hardening-your-deployments/about-security-hardening-with-openid-connect)
- [actions/dependency-review-action](https://github.com/actions/dependency-review-action)
- [Required workflows](https://docs.github.com/en/actions/using-workflows/required-workflows)
- [SLSA framework](https://slsa.dev)

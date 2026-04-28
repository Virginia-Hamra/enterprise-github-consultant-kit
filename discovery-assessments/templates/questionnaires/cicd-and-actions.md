# CI/CD & GitHub Actions Questionnaire

> **Audience:** DevOps / Platform &nbsp; | &nbsp; **Time:** ~60 min

## Current CI/CD landscape
1. Existing CI tools (Jenkins, Azure DevOps, GitLab CI, CircleCI, ...)?
2. Number of pipelines / repos using each tool?
3. Build volumes (jobs/day, peak concurrency)?
4. Languages / build systems / package managers?

## GitHub Actions usage
1. Adoption today (% of repos)? Use cases (CI, CD, governance)?
2. Runner strategy: GitHub-hosted, self-hosted, ARC, larger runners?
3. Reusable workflows / composite actions library?
4. Allowed Actions policy (allowlist, verified creators only, internal only)?

## Secrets & credentials
1. Where are deployment credentials stored? GitHub secrets, Vault, cloud KMS?
2. OIDC adoption (AWS / Azure / GCP)?
3. Environment protection rules (required reviewers, wait timers, deployment branches)?

## Build & deploy targets
1. Cloud(s): AWS / Azure / GCP / on-prem?
2. Artifact registries (GHCR, ECR, ACR, Artifactory, Nexus)?
3. Deployment patterns (blue/green, canary, GitOps)?

## Pain points
1. Top 3 CI/CD pain points today?
2. Migration goals & success criteria?

## Artifacts requested
- [ ] Sample pipeline definitions from current tool
- [ ] Runner inventory
- [ ] Allowed Actions policy

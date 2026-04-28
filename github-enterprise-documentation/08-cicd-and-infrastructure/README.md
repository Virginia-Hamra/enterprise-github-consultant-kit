# 08 — CI/CD & Infrastructure

> The platform-level architecture for build, test, deploy, and the runner / artifact / secrets infrastructure underneath it.

---

## Advisory Gist

**TL;DR.** GitHub-hosted by default for security + zero-ops. **Actions Runner Controller (ARC)** on Kubernetes for scale, internal-network workloads, and high-volume Linux CI. Static self-hosted only as a last resort (and ephemeral, never persistent). Pair every runner topology with a clear cost + security model.

**Decisions you will be asked to make**

- Runner topology: GitHub-hosted / Larger / ARC / static self-hosted (per workload class).
- ARC platform: which K8s cluster, which scaling backend, which network policy.
- Cache and artifact strategy (size, retention, eviction).
- Deployment pattern: environment + OIDC, or external CD (ArgoCD / Flux)?
- Build vs deploy separation of duties.

**Top edges**

- macOS runners are 10×, Windows 2× — silent cost driver.
- Persistent self-hosted runners = lateral-movement risk; ephemeral only.
- ARC cluster outage = CI outage — needs the same SLO as production.
- Cache poisoning across branches — scope cache by ref.

**Connects to**

- [07 GitHub Actions](../07-github-actions/README.md) — workflow design.
- [11 Networking](../11-networking-and-connectivity/README.md) — runner egress, private connectivity.
- [13 Operational Management](../13-operational-management/README.md) — platform SLO.
- [15 Cost & Licensing](../15-cost-and-licensing/README.md) — minutes economics.

**Customer-fit questions**

- Where do builds need to reach — internet, internal network, both?
- Who operates the ARC platform on day 2?
- What's the cost ceiling per dev per month for CI?

---

## Overview

This domain is the **architectural pattern** layer that sits on top of [07-github-actions](../07-github-actions/README.md). Decisions here shape every team's pipelines.

| Concern | Choices |
|---------|---------|
| Runner topology | GitHub-hosted, larger, ARM, GPU, ARC (K8s), static VM |
| Egress / network | Public, IP-allowlisted, private (VNet / PrivateLink) |
| Cloud auth | OIDC federation (recommended) vs static keys |
| Secrets | GitHub env / org secrets, HashiCorp Vault, cloud KMS |
| Artifacts | GitHub Packages (GHCR / npm / Maven / NuGet / Ruby), external registries |
| Promotion | dev → staging → prod via Environments + rulesets |
| Release engineering | Tags, releases, immutable artifacts, attestations |
| GitOps | Argo CD / Flux pulling from GitHub |
| IaC | Terraform / Pulumi / Bicep / Crossplane in PRs |

---

## Configuration

### Reference pipeline pattern

```text
PR → CI (build, test, lint, GHAS, dep review)  ← required workflow
       └── on merge → release pipeline
                          ├── build + sign + attest artifact
                          ├── publish to registry
                          └── trigger CD via repository_dispatch / workflow_run
                                ├── deploy to dev (auto)
                                ├── deploy to staging (auto + smoke tests)
                                └── deploy to prod (env approval + canary)
```

### Runner architecture

- **ARC on Kubernetes** for self-hosted scale: ephemeral, autoscaled, namespace-isolated per security zone.
- **Runner groups** for blast-radius isolation.
- **Larger runners with VNet** when egress control is needed without going self-hosted.

### Cloud auth

- **OIDC** to AWS (`role-to-assume`), Azure (`workload-identity-federation`), GCP (`workload identity federation`).
- Vault: GitHub OIDC → Vault JWT auth method.

### Secrets hierarchy

1. Workload identity / OIDC (no secret)
2. Short-lived secret from Vault
3. Environment secret (scoped to env)
4. Org secret (only for low-sensitivity shared)

### Promotion

- Environments per stage with branch / tag policy.
- Rulesets enforce required workflows, signed commits, ≥1 reviewer.

---

## Usage

- All deploys go through GitHub Environments — no out-of-band tooling.
- Production deploys gated by reviewers + canary policy.
- Rollback = revert PR + redeploy (GitOps) or env-level rollback.
- Internal artifacts published to GHCR / Packages with attestations.

---

## Best Practices

- **GitOps** pull-based deploys: Git is source of truth, controllers reconcile.
- Reusable workflows for canonical patterns (build-and-publish, terraform, k8s deploy).
- One **org-wide CI required workflow** enforcing GHAS scans + dependency review.
- Track DORA metrics from Actions deployment events (see [DevOps & SRE](../../solution-architecture-knowledge/devops-and-sre.md)).
- Tag all infrastructure with `service`, `team`, `cost-center`, `env`, `compliance`.

---

## Common Pitfalls

- 1:1 port of legacy CI (Jenkins / Azure Pipelines) into Actions YAML.
- Self-hosted runners with broad corporate-network reachability.
- Environment without reviewers in production.
- Mixing artifact storage across multiple registries with no governance.
- Manual deploys bypassing Environments.

---

## Implementation Notes

- For GHES: GitHub-hosted runners are not available; ARC + cluster sizing is the production path.
- Larger runners can have **dedicated IP ranges** — useful for IP-allowlisted internal endpoints.
- Build cost dashboards: Actions minutes by repo, by runner type. Migrate macOS / Windows workloads to Linux where possible.
- Watch the Actions queue depth as an SLO; jobs queueing > 5 min indicates capacity issues.

---

## Sources

- [GitHub Actions docs](https://docs.github.com/en/actions)
- [Deployment with environments](https://docs.github.com/en/actions/deployment/targeting-different-environments/using-environments-for-deployment)
- [About self-hosted runners](https://docs.github.com/en/actions/hosting-your-own-runners/managing-self-hosted-runners/about-self-hosted-runners)
- [Actions Runner Controller (ARC)](https://docs.github.com/en/actions/hosting-your-own-runners/managing-self-hosted-runners-with-actions-runner-controller)
- [Larger runners](https://docs.github.com/en/actions/using-github-hosted-runners/about-larger-runners/about-larger-runners)
- [Configuring private networking for larger runners](https://docs.github.com/en/actions/using-github-hosted-runners/about-larger-runners/about-private-networking-with-larger-runners)
- [OpenID Connect with AWS](https://docs.github.com/en/actions/security-for-github-actions/security-hardening-your-deployments/configuring-openid-connect-in-amazon-web-services)
- [OpenID Connect with Azure](https://docs.github.com/en/actions/security-for-github-actions/security-hardening-your-deployments/configuring-openid-connect-in-azure)
- [OpenID Connect with GCP](https://docs.github.com/en/actions/security-for-github-actions/security-hardening-your-deployments/configuring-openid-connect-in-google-cloud-platform)
- [GitHub Packages](https://docs.github.com/en/packages)
- [About releases](https://docs.github.com/en/repositories/releasing-projects-on-github/about-releases)

# 07 — GitHub Actions

> Native CI/CD + workflow automation. At enterprise scale it is also the security control plane and supply-chain enforcement layer.

---

## Overview

| Surface | Purpose |
|---------|---------|
| Workflows | YAML files in `.github/workflows/` |
| Reusable workflows | `uses: org/repo/.github/workflows/x.yml@sha` |
| Composite Actions | Bundled steps shipped as an action |
| Required workflows | Org-enforced workflows on selected repos |
| Environments | Deployment targets with reviewers + secrets |
| Secrets | Repo / env / org scoped |
| OIDC | Cloud auth without long-lived secrets |
| Runners | GitHub-hosted (incl. larger / ARM / GPU) or self-hosted (incl. ARC) |

---

## Configuration

### Enterprise / org policy

- **Allowed Actions**: not "all". Use `Allow GitHub-owned + verified creators + selected actions` and provide an internal allow-list.
- **Default `GITHUB_TOKEN` permissions** = read-only. Workflows opt-in via `permissions:`.
- **Fork PR Actions**: require approval for first-time contributors; restrict secret access.
- **Workflow approvals** for outside collaborators.

### Runners

- **GitHub-hosted** for hygiene (clean VM per job, GitHub SLA).
- **Larger runners** (more CPU/RAM, dedicated IP ranges, optional VNet for Azure-hosted).
- **ARM** and **GPU** runners for specialized workloads.
- **Self-hosted** via **Actions Runner Controller (ARC)** in Kubernetes — preferred over static VM runners; ephemeral by design.
- Register self-hosted runners at the **org** level with **runner groups**.

### Environments

- Required reviewers, wait timer, deployment branch / tag policy.
- Production = manual approval from a defined group.
- **Environment secrets** scoped to that environment only.

### Secrets / OIDC

- Replace long-lived cloud keys with **OIDC federation** to AWS / Azure / GCP / Vault.
- Use environment secrets for deploy credentials; org secrets only for low-sensitivity shared values.

### Reusable workflows

- Centralize patterns in a `workflows-` repo: artifact build/publish, container build with attestation, terraform plan/apply with env gating, GHAS scan with policy enforcement.

---

## Usage

- CI on `push` / `pull_request` validates code; does not deploy.
- Deploy workflows separate, triggered by tag / dispatch / workflow_run.
- `GITHUB_TOKEN` is the default for in-repo API operations.
- Reusable workflows consumed by teams; composite actions for reusable steps.

---

## Best Practices

- **Pin Actions to commit SHAs** — single most impactful Actions security control.
- Vet third-party Actions (security review + SHA-pin + internal registry entry).
- OIDC > stored credentials.
- Short, composable workflows (≤ 200 lines).
- Treat `pull_request_target` with explicit care; never check out PR HEAD with secrets.

---

## Common Pitfalls

- `pull_request_target` checking out PR branch with full secret access — top exploited misconfiguration.
- Persistent self-hosted runners caching credentials between jobs.
- Org secrets accessible to all repos by default.
- `GITHUB_TOKEN` default `write` on every workflow.
- No environment protection on prod deploys.

---

## Implementation Notes

- **GHES**: only self-hosted runners are available — plan capacity accordingly.
- Build a library of **reusable workflows + required workflows** for org-wide patterns.
- Self-hosted runner infra = production compute; monitored, patched, capacity-planned.
- Stream Actions logs to durable storage if retention > 90 days is required.
- Approve and SHA-pin third-party Actions in an **internal Action registry**.

---

## Sources

- [GitHub Actions documentation](https://docs.github.com/en/actions)
- [Security hardening for GitHub Actions](https://docs.github.com/en/actions/security-for-github-actions/security-guides/security-hardening-for-github-actions)
- [About OpenID Connect](https://docs.github.com/en/actions/security-for-github-actions/security-hardening-your-deployments/about-security-hardening-with-openid-connect)
- [Reusable workflows](https://docs.github.com/en/actions/using-workflows/reusing-workflows)
- [Required workflows](https://docs.github.com/en/actions/using-workflows/required-workflows)
- [Using environments for deployments](https://docs.github.com/en/actions/deployment/targeting-different-environments/using-environments-for-deployment)
- [Self-hosted runners](https://docs.github.com/en/actions/hosting-your-own-runners)
- [Actions Runner Controller (ARC)](https://docs.github.com/en/actions/hosting-your-own-runners/managing-self-hosted-runners-with-actions-runner-controller)
- [Larger runners](https://docs.github.com/en/actions/using-github-hosted-runners/about-larger-runners/about-larger-runners)
- [GitHub-hosted runner IP ranges (`/meta`)](https://api.github.com/meta)
- [Restricting permissions for `GITHUB_TOKEN`](https://docs.github.com/en/actions/security-for-github-actions/security-guides/automatic-token-authentication)

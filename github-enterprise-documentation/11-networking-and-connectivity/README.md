# 11 — Networking & Connectivity

> How developers, runners, webhooks, and GHES instances connect to and from GitHub.

---

## Advisory Gist

**TL;DR.** Allow-list GitHub IP ranges from the customer's egress, treat the **GitHub meta API as authoritative** (not static lists), use **private connectivity** (Azure Private Link to GHEC where available, VNet-injected ARC for runners). For GHES: TLS, WAF, K8s ingress, network segmentation are *yours* to design.

**Decisions you will be asked to make**

- Egress allow-list source of truth (meta API consumer / automation).
- IP allow-list for org / enterprise (and Actions implications).
- Private connectivity (Private Link, Azure VNet, AWS PrivateLink) where supported.
- GHES topology: front-door, WAF, TLS termination.
- Webhook ingress (public endpoint vs reverse-proxy vs eventing bridge).

**Top edges**

- IP allow-lists silently break GitHub-hosted runners unless GitHub Actions IPs are explicitly allowed.
- The meta API list changes — static allow-lists drift to broken.
- mTLS to webhooks is not natively supported — reverse-proxy patterns required.
- GHES + WAF: many WAFs false-positive on git protocol traffic.

**Connects to**

- [01 Platform Options](../01-platform-options/README.md) — air-gap forces GHES.
- [07 GitHub Actions](../07-github-actions/README.md) / [08 CI/CD](../08-cicd-and-infrastructure/README.md) — runner egress.
- [12 Integrations & APIs](../12-integrations-and-apis/README.md) — webhook reachability.
- [13 Operational Management](../13-operational-management/README.md) — GHES networking ops.

**Customer-fit questions**

- Is there a hard egress allow-list policy, and who owns it?
- Where do webhooks need to land — cloud, on-prem, both?
- Does a WAF sit in the path, and has it been tuned for git?

---

## Overview

| Direction | Concern |
|-----------|---------|
| Dev → GHEC | IP allowlist, SSO, proxy |
| Runner → GitHub | Outbound HTTPS, long-poll |
| Runner → internal | Restricted by network zone |
| GitHub → webhook receiver | Published IP ranges, secret verification |
| GitHub → GHES (Connect) | Outbound HTTPS only |
| GHES → users | Internal DNS, TLS termination |

---

## Configuration

### IP allowlist (GHEC)

- Enterprise- or org-level allowlist of source IPs.
- **Must include GitHub-hosted runner IP ranges** (`/meta` endpoint) or Actions break.
- Optional: dynamic allowlist from `GitHub Apps` for time-bounded entries.

### Self-hosted runner network placement

- **Outbound** to: github.com, api.github.com, objects.githubusercontent.com, ghcr.io, codeload.github.com, copilot-proxy.githubusercontent.com (verify current list).
- **No inbound** required (long-poll out).
- Place in a segment with **least-privilege** access to internal systems.
- Separate runner pools by **security zone** (untrusted code vs prod-deploy).

### Larger runners with private networking

- **Azure VNet injection** for larger runners — reach internal endpoints without going self-hosted.
- Dedicated IP ranges for IP-allowlisted internal endpoints.

### GHES networking

- Static IP + hostname.
- TLS terminated at GHES or external LB (HA).
- Internal DNS for the hostname.
- GitHub Connect requires outbound HTTPS to github.com.

### Webhooks

- Receivers must verify the `X-Hub-Signature-256` header.
- For internal-only targets: webhook relay / event bridge in DMZ.
- IP allowlist of GitHub's published webhook ranges (`/meta`).

---

## Usage

- Devs reach GHEC over corporate / VPN; allowlist enforces it.
- Runners maintain persistent outbound — no firewall punches inbound.
- Webhooks are the event-driven integration pattern, not polling.

---

## Best Practices

- Automate `/meta` ingestion into firewall management.
- Isolate runner networks: production-deploy runners ≠ untrusted-code runners.
- Use **webhook relays** instead of opening inbound from GitHub IP ranges into the data center.
- Document GHES service IP, LB IP, DNS, certificates — inputs to runbooks.

---

## Common Pitfalls

- IP allowlist enabled, runner ranges missing → Actions silently break.
- Self-hosted runners in same segment as production databases.
- Webhook receivers skip signature verification ("internal network is enough").
- GHES TLS cert expires → instant outage.

---

## Implementation Notes

- For private networking from Actions to internal services, **larger runners with VNet injection** are usually simpler than VPN+self-hosted.
- GHES supports SAML / OIDC; built-in auth must not be used in production.
- Automate TLS renewal on GHES via ACME or your enterprise PKI.
- Run a **firewall ACL test** monthly against `/meta` to detect drift.

---

## Sources

- [Restricting network traffic to your enterprise (IP allow list)](https://docs.github.com/en/enterprise-cloud@latest/admin/configuration/hardening-security-for-your-enterprise/restricting-network-traffic-to-your-enterprise-with-an-ip-allow-list)
- [GitHub Meta endpoint (`api.github.com/meta`)](https://docs.github.com/en/rest/meta/meta)
- [Communications between self-hosted runners and GitHub](https://docs.github.com/en/actions/hosting-your-own-runners/managing-self-hosted-runners/about-self-hosted-runners#communication-between-self-hosted-runners-and-github)
- [About private networking for larger runners](https://docs.github.com/en/actions/using-github-hosted-runners/about-larger-runners/about-private-networking-with-larger-runners)
- [Webhook delivery & verification](https://docs.github.com/en/webhooks/using-webhooks/validating-webhook-deliveries)
- [Configuring TLS on GHES](https://docs.github.com/en/enterprise-server/admin/configuration/hardening-security-for-your-enterprise/configuring-tls)
- [About GitHub Connect](https://docs.github.com/en/enterprise-server/admin/configuration/configuring-github-connect/about-github-connect)
- [GitHub-hosted runner IP ranges](https://docs.github.com/en/actions/using-github-hosted-runners/connecting-to-a-private-network/about-using-github-hosted-runners-in-a-network)

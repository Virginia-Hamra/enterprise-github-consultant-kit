# 13 — Operational Management

> Day-to-day and incident-level operations for keeping GitHub Enterprise available, healthy, recoverable.

---

## Advisory Gist

**TL;DR.** For GHEC: ops = config drift + integrations + access. For GHES: ops = above + infra lifecycle (HA, backups, upgrades, capacity). Define **RTO / RPO for GitHub explicitly** — most enterprises don't. Match GitHub Support tier to RTO. Rehearse failover and restore.

**Decisions you will be asked to make**

- RTO / RPO for the GitHub platform.
- GHES upgrade cadence (security: 30d, feature: 60d).
- HA topology and failover rehearsal cadence.
- Backup destination + retention + restore-test cadence.
- Support tier (Standard / Premium / Premium Plus).

**Top edges**

- GHES HA failover requires re-sync of original primary — documented runbook required.
- Backups never restored → unverified DR.
- Disk capacity unmonitored → push outage.
- GitHub Connect quietly degrades → partial GHES outage that's hard to spot.

**Connects to**

- [01 Platform Options](../01-platform-options/README.md) — GHEC vs GHES ops cost.
- [11 Networking](../11-networking-and-connectivity/README.md) — GHES network ops.
- [16 Risks & Tradeoffs](../16-risks-and-tradeoffs/README.md) — hosted vs self-hosted runners.
- [17 Additional Services](../17-additional-services/README.md) — service-level features.

**Customer-fit questions**

- What's the documented RTO / RPO for GitHub today?
- Who is on-call for GHES, and what's their runbook?
- When was the last successful restore-from-backup test?

---

## Overview

| Surface | GHEC | GHES |
|---------|------|------|
| Infra ownership | GitHub | You |
| Patching | GitHub | You |
| Backups | GitHub | You (Backup Utilities) |
| HA / DR | GitHub | You (passive replica) |
| Capacity | GitHub | You |
| Monitoring | Status page + audit | SNMP / syslog / Mgmt Console + audit |

---

## Configuration

### GHEC

- Subscribe to **GitHub status page** (status notifications, RSS / webhook).
- Audit log streaming as the security + ops feed.
- Monitor enterprise / org metrics via API.

### GHES

- **GitHub Backup Utilities** (open-source, vendor-maintained).
- HA: passive hot standby; verify replication; rehearse failover annually.
- Health: Mgmt Console metrics endpoint + SNMP + syslog forwarding.
- Storage: plan **2× current git data size** for growth + snapshots.
- Disk-usage alerts at 70% and 85%.
- GitHub Connect monitoring (sync status).

### Incident response

- Runbook for: GitHub-side outages, GHES outages, SSO outages, Actions queue saturation.
- Communication plan: where developers see status, what work continues offline.

---

## Usage

- GHEC: ops responsibility = config drift, integrations, user access.
- GHES: ops responsibility = above + infrastructure lifecycle.
- Incident drills at least annually.

---

## Best Practices

- For GHES: **30 days for security releases, 60 days for feature releases**.
- Maintain a **GHES staging instance** for upgrade validation.
- **Quarterly** restore from backup tests.
- Define **RTO / RPO** for GitHub explicitly — most enterprises don't.
- Match GitHub Support tier (Premium / Premium Plus) to your RTO.

---

## Common Pitfalls

- No GHES staging → upgrade goes straight to prod.
- Backups never restored → unverified DR capability.
- Disk capacity unmonitored → push outage.
- GitHub Connect lost silently → degraded GHES.
- "GitHub down" not in the company incident plan.

---

## Implementation Notes

- Build a **GHES operational runbook**: upgrade, backup / restore, HA failover, storage expansion, admin password recovery, SSO break-glass.
- For GHES HA: original primary requires re-sync after promotion; document the sequence.
- At 10k+ devs: monitor Actions **queue depth** as a platform SLO.
- Build runner autoscaling (ARC) for predictable CI spikes.

---

## Sources

- [GitHub Status](https://www.githubstatus.com/)
- [Monitoring your GHES instance](https://docs.github.com/en/enterprise-server/admin/monitoring-and-managing-your-instance/monitoring-your-instance)
- [Backups on GHES (Backup Utilities)](https://docs.github.com/en/enterprise-server/admin/backing-up-and-restoring-your-instance/configuring-backups-on-your-instance)
- [GitHub Backup Utilities (GitHub repo)](https://github.com/github/backup-utils)
- [Configuring high availability on GHES](https://docs.github.com/en/enterprise-server/admin/configuration/configuring-high-availability/configuring-github-enterprise-server-for-high-availability)
- [Initiating a failover to your replica](https://docs.github.com/en/enterprise-server/admin/configuration/configuring-high-availability/initiating-a-failover-to-your-replica)
- [Upgrading GHES](https://docs.github.com/en/enterprise-server/admin/all-releases)
- [GHES release notes](https://docs.github.com/en/enterprise-server/admin/release-notes)
- [GitHub support plans](https://support.github.com/contact/plans)

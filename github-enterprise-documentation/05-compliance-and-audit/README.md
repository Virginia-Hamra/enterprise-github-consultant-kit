# 05 — Compliance & Audit

> The enterprise audit log is the system of record for "who did what" in GitHub. Stream it; don't query the UI.

---

## Advisory Gist

**TL;DR.** Audit log streaming → SIEM is non-negotiable for any regulated customer. Capture **all** event categories, retain to the regulator's clock, treat the UI log as a debug surface not evidence. Map controls to the customer's framework (SOC2 / ISO27001 / DORA / PCI / NIS2) explicitly.

**Decisions you will be asked to make**

- Streaming target: Splunk / Sentinel / Datadog / S3 / Azure Event Hub.
- Retention period (regulatory clock vs GitHub's default).
- Compliance framework mapping (SOC2 / ISO / DORA / NIS2 / PCI / HIPAA).
- Evidence collection cadence and ownership.
- Audit log streaming reliability monitoring.

**Top edges**

- The UI audit log is **not** complete enough for evidence; the streaming feed is.
- A silent streaming outage = compliance gap; alarm on absence of events.
- Some events are GHEC-only or GHES-only — confirm coverage per platform.
- DORA / NIS2 obligations cascade to *suppliers* of the customer — GitHub is one.

**Connects to**

- [02 Identity & Access](../02-identity-and-access-management/README.md) — auth events.
- [04 GHAS](../04-security-ghas/README.md) — security alert lifecycle as evidence.
- [10 Data & Privacy](../10-data-and-privacy/README.md) — retention boundaries.
- [12 Integrations & APIs](../12-integrations-and-apis/README.md) — streaming target wiring.

**Customer-fit questions**

- Which framework is the auditor reading against this year?
- Where does the SIEM live, who owns the GitHub feed, who reviews it?
- What is the longest retention window any regulator imposes on this tenant?

---

## Overview

GitHub records:

- **Enterprise / org / repo events** (settings changes, member changes)
- **Actions events** (workflow runs, secret access)
- **Git events** (push / clone / fetch — high volume, optional)
- **Billing events**
- **Copilot events** (`copilot.*`)
- **Secret scanning / code scanning events**

The native UI log has limited retention. Compliance retention requires **streaming** to a durable sink.

---

## Configuration

### Audit log streaming

Supported destinations (verify current list in docs):

- **Azure Event Hubs**
- **Azure Blob Storage**
- **Amazon S3** (with object lock for immutability)
- **Google Cloud Storage**
- **Splunk** (HEC)
- **Datadog**

Enable **git events** explicitly — off by default.

### Retention

- Native log retention is limited; verify current GHEC retention in docs.
- Streamed data lives in **your** storage with your retention + immutability rules.
- For GHES: equivalent log + streaming destinations vary by version.

### Logical structure

Events have an `action` (e.g., `org.add_member`, `repo.create`, `secret_scanning.alert_create`), an `actor`, a `created_at`, and event-specific payload. Build SIEM detections on the `action` taxonomy.

---

## Usage

- Compliance / security teams consume audit data from the **stream**, not the UI.
- Audit data feeds: access reviews, incident investigations, compliance reports.
- The log captures **actions, not payloads** (no code contents, no PR comment bodies beyond metadata).

---

## Best Practices

- Configure streaming **before** any compliance deadline — retroactive reconstruction is impossible.
- Include joiner-mover-leaver events: `org.add_member`, `org.remove_member`, `org.invitation_email`.
- Combine audit + GHAS for incident narrative.
- Verify destination immutability (S3 object-lock, Blob WORM).
- Build runbooks for the 5 most common investigations: user access review, secret incident, unauthorized repo access, Actions tampering, external collaborator activity.

---

## Common Pitfalls

- Git events excluded by default → insider-threat / exfiltration blind spot.
- UI log used as alerting system (it isn't).
- GHES decommissioned without exporting audit data → permanent loss.
- Streaming configured late → historical gap, document it.

---

## Implementation Notes

- Audit log API supports **cursor-based pagination** for bulk export. Don't paginate via the UI.
- For FedRAMP / ITAR: verify GitHub's current certification scope per your control catalog.
- DORA / EU AI Act: maintain an ICT third-party register including GitHub + audit-log streaming destination.
- Build a **Compliance Dashboard** showing access reviews, alert SLAs, and approvals from a single SIEM source.

---

## Sources

- [Audit log for your enterprise](https://docs.github.com/en/enterprise-cloud@latest/admin/monitoring-activity-in-your-enterprise/reviewing-audit-logs-for-your-enterprise/about-the-audit-log-for-your-enterprise)
- [Streaming the audit log](https://docs.github.com/en/enterprise-cloud@latest/admin/monitoring-activity-in-your-enterprise/streaming-the-audit-log-for-your-enterprise/streaming-the-audit-log-for-your-enterprise)
- [Audit log events for your enterprise](https://docs.github.com/en/enterprise-cloud@latest/admin/monitoring-activity-in-your-enterprise/reviewing-audit-logs-for-your-enterprise/audit-log-events-for-your-enterprise)
- [Audit log API reference](https://docs.github.com/en/enterprise-cloud@latest/rest/enterprise-admin/audit-log)
- [GHES audit log](https://docs.github.com/en/enterprise-server/admin/monitoring-and-managing-your-instance/log-forwarding/about-log-forwarding)
- [GitHub Trust Center — compliance certifications](https://github.com/trust-center)
- [GitHub Data Processing Agreement](https://docs.github.com/en/site-policy/privacy-policies/github-data-protection-agreement)
- [Reviewing the audit log for your organization](https://docs.github.com/en/organizations/keeping-your-organization-secure/managing-security-settings-for-your-organization/reviewing-the-audit-log-for-your-organization)

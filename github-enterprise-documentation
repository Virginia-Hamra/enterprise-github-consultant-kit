# GitHub Enterprise — Documentation & Review Categories

> **Audience:** Enterprise architects, platform engineers, security leads, compliance officers, and procurement stakeholders evaluating or operating GitHub at scale.
> **Purpose:** A structured reference for governance review, due diligence, and ongoing operational oversight of GitHub Enterprise.
> **Note:** GitHub product capabilities change frequently. Where indicated, verify current state in official GitHub documentation at [docs.github.com](https://docs.github.com).

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [Platform Options (Enterprise Cloud vs Enterprise Server)](#2-platform-options-enterprise-cloud-vs-enterprise-server)
3. [Identity & Access Management](#3-identity--access-management)
4. [Organization & Repo Governance](#4-organization--repo-governance)
5. [Security (GitHub Advanced Security)](#5-security-github-advanced-security)
6. [Compliance & Audit](#6-compliance--audit)
7. [Software Supply Chain](#7-software-supply-chain)
8. [GitHub Actions](#8-github-actions)
9. [Copilot in the Enterprise](#9-copilot-in-the-enterprise)
10. [Data & Privacy](#10-data--privacy)
11. [Networking & Connectivity](#11-networking--connectivity)
12. [Integrations & APIs](#12-integrations--apis)
13. [Operational Management](#13-operational-management)
14. [Migration & Adoption](#14-migration--adoption)
15. [Cost & Licensing Review](#15-cost--licensing-review)
16. [Risks, Tradeoffs, and Decision Checklist](#16-risks-tradeoffs-and-decision-checklist)
17. [Additional GitHub Services & Features](#17-additional-github-services--features)

---

## 1. Executive Summary

**Purpose:** Provide a concise, factual overview of the enterprise's GitHub posture — deployment model, governance maturity, security posture, and known gaps — to orient decision-makers before deeper review.

### What to Review

- Current GitHub deployment model (Cloud, Server, or hybrid)
- Number of enterprises, organizations, repositories, and active users
- Current licensing tier and renewal timeline
- Known compliance obligations (SOC 2, FedRAMP, ISO 27001, HIPAA, etc.)
- Outstanding risks or audit findings related to GitHub
- Existing GitHub governance documentation and ownership

### Review Checklist

- [ ] Enterprise account structure documented (enterprise → orgs → repos)
- [ ] Total licensed seat count reconciled against actual active users
- [ ] Data classification applied to repository contents
- [ ] Primary stakeholder and DRI (Directly Responsible Individual) identified
- [ ] GitHub deployment model formally documented
- [ ] Open security advisories or Dependabot alerts triaged
- [ ] GitHub contract and SLA terms reviewed
- [ ] Prior audit or pen test findings related to GitHub documented

### Key Stakeholder Questions

- Who owns GitHub governance at the enterprise level today?
- What compliance frameworks apply, and have they been mapped to GitHub controls?
- What is the current gap between desired and actual governance maturity?
- Has GitHub ever been involved in a security incident, data breach, or compliance finding?
- What is the roadmap for GitHub feature adoption (Actions, Copilot, GHAS)?
- Are there shadow GitHub orgs or accounts outside the enterprise umbrella?
- What is the process for onboarding new teams to GitHub today?
- How are GitHub access rights reviewed and revoked?

### Evidence to Collect

- Enterprise account overview screenshot (organizations, members, seat count)
- Current licensing contract and renewal date
- Any existing GitHub governance or runbook documents
- Compliance mapping documents referencing GitHub
- Recent audit logs export (enterprise-level)

---

## 2. Platform Options (Enterprise Cloud vs Enterprise Server)

**Purpose:** Define which GitHub deployment model is in use or under consideration, and document the capability, compliance, and operational tradeoffs between each option.

### What to Review

- Current deployment: GitHub Enterprise Cloud (GHEC), GitHub Enterprise Server (GHES), or GitHub AE (verify current availability in GitHub docs)
- Data residency and sovereignty requirements
- Feature parity gaps between Cloud and Server at current Server version
- Upgrade cadence and operational overhead for GHES
- Disaster recovery and availability SLAs for each model

### Review Checklist

- [ ] Deployment model formally chosen and documented
- [ ] Compliance and data residency requirements mapped to each option
- [ ] GHES version pinned and upgrade schedule maintained (if applicable)
- [ ] Feature gap analysis completed between current GHES version and GHEC
- [ ] SLA and uptime requirements documented and compared to GitHub's published SLAs
- [ ] Hybrid model connectivity (if GHEC + GHES) documented
- [ ] GitHub AE eligibility and current status verified in GitHub docs (verify current state)
- [ ] Decision rationale recorded for chosen deployment model

### Key Stakeholder Questions

- Do any regulatory or contractual requirements mandate on-premises data storage?
- Is the organization's infrastructure team prepared to operate and patch GHES?
- What is the acceptable upgrade lag for GHES relative to GHEC feature releases?
- Are there latency or connectivity constraints that affect GHEC usability?
- How does the chosen model affect GitHub Advanced Security feature availability?
- What is the failover plan if GHEC has an outage?
- Is GitHub Connect configured for GHES, and what data does it transmit?
- Has data residency been evaluated for GitHub Enterprise Managed Users (EMU)?

### Evidence to Collect

- Screenshot or export of enterprise account type from GitHub settings
- GHES version and patch history (if applicable)
- GitHub Connect configuration documentation (if applicable)
- Feature comparison matrix (Cloud vs current Server version)
- Vendor-provided SLA documents

---

## 3. Identity & Access Management

**Purpose:** Document and assess how users are authenticated, provisioned, and authorized within GitHub Enterprise, including SSO, SCIM, and role-based access controls.

### What to Review

- SSO configuration: SAML 2.0 or OIDC provider, binding, and fallback behavior
- SCIM provisioning: which attributes are synced, deprovisioning behavior, sync errors
- Enterprise Managed Users (EMU) vs standard enterprise: identity ownership model
- RBAC: enterprise owner, organization owner, member, billing manager, security manager roles
- External collaborator access patterns and reviews
- Service accounts and machine user management
- Personal access tokens (PATs) — classic vs fine-grained — issuance and expiry policies
- SSH key and GPG key policies

### Review Checklist

- [ ] SAML/OIDC SSO enforced at enterprise or organization level
- [ ] SCIM provisioning configured and tested for deprovisioning
- [ ] SSO fallback (recovery codes) process documented and tested
- [ ] EMU vs standard identity model explicitly decided and documented
- [ ] Minimum required roles defined for each team persona
- [ ] External collaborator access reviewed and aged accounts removed
- [ ] PAT policies defined: expiration enforced, fine-grained PATs preferred over classic
- [ ] SSH key age and rotation policy defined
- [ ] Service/machine accounts inventoried and access scoped appropriately
- [ ] Role assignments audited in the last 90 days

### Key Stakeholder Questions

- Which IdP is authoritative for GitHub identities, and is the integration tested end-to-end?
- What happens to a GitHub account when a user is offboarded from the IdP?
- Are there accounts that bypass SSO (e.g., built before SSO was enforced)?
- How are fine-grained PATs governed — is there a request and approval process?
- What is the process for granting and revoking external collaborator access?
- Are machine users subject to the same access review cycle as human users?
- How is the Security Manager role used, and who holds it?
- Is MFA enforced at the enterprise or organization level?

### Evidence to Collect

- SAML/OIDC SSO configuration screenshots (IdP and GitHub sides)
- SCIM provisioning logs showing recent sync events and errors
- List of enterprise owners and organization owners
- Export of external collaborators and their repository access
- PAT policy configuration screenshots
- SCIM attribute mapping documentation
- MFA enforcement policy screenshot

---

## 4. Organization & Repo Governance

**Purpose:** Document the structural policies, naming conventions, templates, and standards that govern how organizations, repositories, teams, and collaborators are managed at scale.

### What to Review

- Enterprise and organization policy hierarchy (what is enforced at enterprise vs delegated to orgs)
- Repository creation policies: who can create, public vs private vs internal visibility
- Default branch protections: required reviews, status checks, linear history, force push restrictions
- Repository templates and `.github` default community health files
- Team structure and team sync with IdP groups
- Webhook configuration at enterprise, org, and repo levels
- Naming conventions and repository lifecycle (archiving, deletion, transfer)
- Fork policies and internal vs public fork governance
- External collaborator policies

### Review Checklist

- [ ] Enterprise-level policies documented (what is locked vs configurable by orgs)
- [ ] Repository creation restricted to defined roles or teams
- [ ] Default branch protection rules applied across all production repositories
- [ ] Repository template(s) maintained with required files (CODEOWNERS, SECURITY.md, etc.)
- [ ] Team structure mirrors organizational structure and is synced to IdP
- [ ] Webhooks inventoried at enterprise, org, and repo levels with owners documented
- [ ] Repository visibility policy (no public repos without approval, for example) defined
- [ ] Repository archiving process documented for inactive repos
- [ ] CODEOWNERS files present in critical repositories
- [ ] Fork policy defined: internal forks allowed, public forks restricted or prohibited

### Key Stakeholder Questions

- Which GitHub policies are enforced at the enterprise level vs left to org owners?
- Is there a defined approval process for creating public repositories?
- How is CODEOWNERS used, and are code review requirements enforced consistently?
- What is the lifecycle for a repository — from creation to archiving to deletion?
- Are webhooks regularly audited? Who is responsible for decommissioning stale webhooks?
- How does team structure in GitHub map to the organizational chart?
- What is the process for onboarding external collaborators, and is it documented?
- Are there any repositories with no identifiable owner?
- How are organization-level base permissions set (read, write, none)?

### Evidence to Collect

- Enterprise policy configuration screenshots
- Branch protection rule configurations for production repositories
- Repository template contents
- CODEOWNERS file examples
- Webhook inventory (org and enterprise level)
- Team list with member counts and IdP sync status
- Repository visibility breakdown (public, private, internal)

---

## 5. Security (GitHub Advanced Security)

**Purpose:** Document the configuration and coverage of GitHub Advanced Security (GHAS) features — secret scanning, code scanning, CodeQL, and dependency management — and assess their operational effectiveness.

### What to Review

- GHAS license coverage: which orgs and repos have GHAS enabled
- Secret scanning: push protection status, custom patterns, alert triage process
- Code scanning: which tools are configured (CodeQL, third-party SAST), scan triggers, alert SLAs
- CodeQL: query packs in use, language coverage, scheduled vs PR-triggered scans
- Dependency review: enabled on PRs, blocking policy for high/critical CVEs
- Dependabot alerts: auto-dismiss policies, SLA for triage, auto-PR configuration
- Security overview: enterprise-level visibility into alert status across orgs

### Review Checklist

- [ ] GHAS enabled on all repositories containing production or sensitive code
- [ ] Secret scanning push protection enabled and tested
- [ ] Custom secret scanning patterns defined for organization-specific secrets (API keys, internal tokens)
- [ ] Code scanning configured with at minimum CodeQL on default languages
- [ ] Code scanning alerts triaged within defined SLA (verify current SLA policy)
- [ ] Dependabot alerts enabled and triaged — no unaddressed critical CVEs >30 days old
- [ ] Dependabot security updates (auto-PRs) enabled where appropriate
- [ ] Dependency review action configured to block PRs introducing vulnerable dependencies
- [ ] Security manager role assigned to the security team
- [ ] Security overview dashboard reviewed regularly by security team

### Key Stakeholder Questions

- What percentage of repositories have GHAS fully enabled?
- What is the process for triaging and closing code scanning alerts?
- Are secret scanning alerts treated as incidents, or are they reviewed on a schedule?
- Has push protection ever blocked a real secret commit? What was the outcome?
- Which languages are covered by CodeQL, and are there gaps?
- What is the SLA for resolving critical Dependabot alerts?
- Are third-party SAST tools integrated into code scanning, and how are results correlated?
- How is the security overview used operationally?
- Are there known classes of vulnerabilities that GHAS does not detect in your stack?

### Evidence to Collect

- GHAS enablement report (repos enabled vs total)
- Open secret scanning alerts and age distribution
- Open code scanning alerts by severity and age
- Dependabot alert dashboard screenshot
- Custom secret scanning patterns list
- Code scanning tool configuration (workflow files or default setup configuration)
- Security policy (`SECURITY.md`) from key repositories

---

## 6. Compliance & Audit

**Purpose:** Document GitHub's audit logging capabilities, data retention configuration, and how audit data supports regulatory compliance obligations.

### What to Review

- Audit log: enterprise-level audit log access, streaming configuration, and retention
- Audit log streaming: configured destinations (Splunk, Datadog, S3, Azure Event Hubs, etc.)
- Log coverage: which event categories are captured (git events, web events, API events)
- Retention: GitHub's default audit log retention period (verify current state in GitHub docs) vs organizational requirements
- Data residency: where audit log data is stored, and whether it meets regulatory requirements
- Compliance certifications: GitHub's current SOC 2, ISO 27001, FedRAMP, and other certifications (verify current state in GitHub docs)
- Access to audit data by compliance and security teams

### Review Checklist

- [ ] Enterprise audit log streaming configured to a durable, centralized destination
- [ ] Audit log streaming tested end-to-end (events appear in SIEM/storage)
- [ ] Retention period of streamed audit logs meets regulatory requirements
- [ ] Git event audit logging enabled (captures push, clone, fetch events)
- [ ] Compliance certifications for GitHub reviewed and within validity period
- [ ] Access to audit logs restricted to authorized compliance/security personnel
- [ ] Process documented for responding to audit evidence requests using GitHub logs
- [ ] Audit log alerts configured for high-risk events (e.g., enterprise owner changes, SSO disable)

### Key Stakeholder Questions

- What is the retention period for GitHub's native audit log, and does it meet your requirements?
- Where does streamed audit data land, and who has access to it?
- Are git-level events (push, clone) included in audit streaming?
- Which compliance frameworks has the organization mapped to GitHub controls?
- How is the audit trail used in incident investigations?
- Has GitHub's SOC 2 report been reviewed, and are there exceptions relevant to your environment?
- Are there regulatory requirements for in-country data residency that affect audit log streaming destinations?
- How quickly can you produce audit evidence for a specific user's activity over a 90-day window?

### Evidence to Collect

- Audit log streaming configuration screenshots
- Sample streamed audit log events from SIEM
- GitHub compliance certification documents (SOC 2 Type II, ISO 27001, etc.)
- Documented retention policy for GitHub audit data
- Screenshot of enterprise audit log with recent events
- Access control list for audit log streaming destination

---

## 7. Software Supply Chain

**Purpose:** Document how the enterprise manages software provenance, artifact integrity, SBOM generation, and trusted publishing to reduce supply chain risk.

### What to Review

- SBOM generation: which tools (Dependency graph, Syft, CycloneDX, SPDX) are in use, at what stage of the pipeline
- Artifact signing: Sigstore/cosign integration, GitHub's artifact attestation feature (verify current state in GitHub docs)
- Provenance: build provenance attestations generated and verifiable
- Trusted publishing: OIDC-based publishing to npm, PyPI, or other registries without long-lived secrets
- Dependency review and supply chain policies: use of `dependency-review-action`
- GitHub-managed supply chain features: Dependency graph, SBOM export via API, Dependabot
- Repository rulesets enforcing signed commits

### Review Checklist

- [ ] Dependency graph enabled for all relevant repositories
- [ ] SBOM export tested via GitHub API for key repositories
- [ ] Artifact attestation or equivalent signing configured for production build outputs
- [ ] Build provenance recorded and verifiable for production artifacts
- [ ] OIDC-based trusted publishing configured for package registries (where applicable)
- [ ] `dependency-review-action` configured to block introduction of known-vulnerable dependencies
- [ ] Signed commits required via repository or organization ruleset (where applicable)
- [ ] SBOM format (SPDX or CycloneDX) standardized across the organization
- [ ] Supply chain security requirements documented in internal policy

### Key Stakeholder Questions

- At what stage of the CI/CD pipeline are SBOMs generated and stored?
- Are build artifacts cryptographically signed, and is signature verification enforced downstream?
- How is provenance verified before deploying an artifact to production?
- Are OIDC-based trusted publishing integrations used, or are long-lived tokens still in use for registry publishing?
- Has the organization evaluated GitHub's artifact attestation feature? (verify current state in GitHub docs)
- How would you detect a compromised third-party GitHub Action in your workflows?
- What is the process for responding to a CVE in a direct or transitive dependency?
- Are SBOM outputs consumed by any vulnerability management platform?

### Evidence to Collect

- Sample SBOM output from a production repository (SPDX or CycloneDX)
- Artifact attestation or signing configuration from CI workflow
- `dependency-review-action` configuration
- Trusted publishing configuration for package registries
- Screenshot of Dependency graph for a key repository

---

## 8. GitHub Actions

**Purpose:** Document the configuration, security posture, and governance of GitHub Actions across the enterprise, including runner infrastructure, secrets management, permissions, and reusable workflow standards.

### What to Review

- Runner types: GitHub-hosted vs self-hosted vs larger runners — which are in use and for what workloads
- Runner security: self-hosted runner isolation, ephemeral vs persistent runners, OS and network posture
- Workflow permissions: default `GITHUB_TOKEN` permissions (read vs write), `permissions` key usage in workflows
- Secrets management: how secrets are scoped (repo, org, enterprise), rotation cadence, and use of external secret managers
- Environments: deployment environment protections, required reviewers, wait timers, deployment branch restrictions
- Reusable workflows: which reusable workflows are maintained, pinning practices, internal marketplace usage
- Action pinning: use of commit SHA pinning vs tag pinning for third-party Actions
- Enterprise-level Actions policies: allowed Actions list, Actions allowed from verified creators only
- Workflow run retention and artifact retention settings

### Review Checklist

- [ ] Enterprise or org-level Actions policy restricts which Actions can be used (no "allow all")
- [ ] Third-party Actions pinned to commit SHA, not mutable tags
- [ ] Default `GITHUB_TOKEN` permissions set to `read-only` at org or enterprise level
- [ ] Workflows explicitly declare required permissions using the `permissions` key
- [ ] Self-hosted runners are ephemeral (or documented justification for persistent runners)
- [ ] Self-hosted runners isolated from production networks where possible
- [ ] Secrets scoped to the minimum necessary level (repo vs org vs environment)
- [ ] Secrets rotation process documented and practiced
- [ ] Deployment environments configured with required reviewers for production
- [ ] Reusable workflows inventoried and owned by a defined team
- [ ] Workflow run logs and artifact retention periods configured and documented
- [ ] `pull_request_target` trigger usage reviewed for security implications

### Key Stakeholder Questions

- What is the policy for approving new third-party Actions for use across the enterprise?
- How are self-hosted runners provisioned, and what is the network exposure of the runner host?
- Are any workflows triggered by `pull_request_target` that could be exploited by fork PRs?
- How are secrets rotated, and who is notified when a secret is due for rotation?
- Are there workflows with overly broad `GITHUB_TOKEN` write permissions?
- Who owns and maintains the organization's reusable workflows?
- Are there any long-running or persistent self-hosted runners that could be compromised between jobs?
- How are production deployments gated — are environment protections enforced?
- Is there a process for decommissioning stale or unused workflows?

### Evidence to Collect

- Enterprise/org Actions policy configuration screenshots
- Sample workflow file showing `permissions` key usage and Action pinning
- Self-hosted runner inventory with OS, network segment, and ephemeral/persistent status
- Environment protection configuration screenshots
- Secrets inventory (names and scopes — not values)
- Reusable workflow repository and ownership documentation

---

## 9. Copilot in the Enterprise

**Purpose:** Document the deployment, controls, telemetry, and risk posture of GitHub Copilot within the enterprise, including policy configuration and data handling considerations.

### What to Review

- Copilot license type: Copilot Business vs Copilot Enterprise (verify current state in GitHub docs for current SKU names)
- Seat assignments: who has Copilot enabled, by org and team
- Policy configuration: duplication detection (public code matching), suggestions matching public code blocked or flagged
- Telemetry and data collection: what usage data is collected, where it flows, and who has access
- Content exclusions: which files, directories, or repositories are excluded from Copilot context
- Copilot Chat: enabled status, model used (verify current state), data handling for chat interactions
- Copilot in the CLI, PRs, and other surfaces: what features are enabled
- Acceptable use policy for Copilot output — has one been communicated to developers?

### Review Checklist

- [ ] Copilot license tier documented and reconciled against seat assignments
- [ ] Seat assignment process documented (who can request, who approves)
- [ ] Duplication detection / public code matching policy configured
- [ ] Content exclusions defined for sensitive repositories or file types
- [ ] Acceptable use policy for Copilot communicated to all enabled users
- [ ] Copilot usage telemetry reviewed — data flows to GitHub, not to external AI providers without review
- [ ] Developer training provided on responsible Copilot use (IP considerations, hallucination risk, sensitive data in prompts)
- [ ] Copilot Chat enabled/disabled decision documented with rationale
- [ ] Legal and IP review of Copilot output ownership completed
- [ ] Enterprise audit log captures Copilot-related events (verify current event coverage in GitHub docs)

### Key Stakeholder Questions

- Has legal reviewed the IP implications of Copilot-generated code in the codebase?
- Are developers aware they should not paste sensitive data, credentials, or PII into Copilot Chat?
- What is the policy if a developer identifies that Copilot suggested code that closely matches a known open-source function?
- Is Copilot usage telemetry reviewed for anomalies or policy violations?
- Who can enable or disable Copilot for an individual or team?
- Are content exclusions configured for repositories containing proprietary algorithms or sensitive business logic?
- What training has been provided on Copilot's limitations (hallucinations, outdated suggestions)?
- Is there an escalation path if a developer has a concern about Copilot output?

### Evidence to Collect

- Copilot policy configuration screenshots (duplication detection, content exclusions)
- Seat assignment report (users enabled by org)
- Acceptable use policy document
- Copilot usage metrics dashboard screenshot (if available)
- Content exclusion configuration files or documentation

---

## 10. Data & Privacy

**Purpose:** Document how GitHub handles organizational data, developer data, and content at rest and in transit, including considerations for AI model training, PII, and data boundaries.

### What to Review

- Data stored in GitHub: code, issues, PRs, comments, wikis, Actions logs, packages, discussions
- PII exposure: names, emails, commit metadata, issue content — where it appears and who can access it
- GitHub's data use for AI model training: Copilot training data opt-out status (verify current state in GitHub docs)
- Content retention: how long GitHub retains deleted content, soft delete behavior
- Data export: available export tools (GitHub Archive Program, API, migration tools)
- Third-party data flows: which integrations receive repository or user data
- GitHub's privacy policy applicability to enterprise accounts
- GDPR, CCPA, or other applicable privacy regulation requirements

### Review Checklist

- [ ] Organizational data classification applied to GitHub content (what tier of data is permitted in GitHub)
- [ ] Copilot training data opt-out confirmed if required by policy (verify current setting in GitHub docs)
- [ ] PII in commit metadata (email addresses) reviewed against privacy requirements
- [ ] Integrations and OAuth apps audited for data they access and transmit
- [ ] Data export capability tested for key repositories
- [ ] Retention requirements for GitHub data documented (issues, PRs, Actions logs)
- [ ] GitHub's privacy policy reviewed and confirmed acceptable for the organization's regulatory context
- [ ] Process defined for responding to data subject access requests (DSARs) involving GitHub data

### Key Stakeholder Questions

- What classes of data are permitted to be stored in GitHub repositories, and is this documented?
- Is GitHub used to store any regulated data (PHI, PCI, ITAR, etc.)?
- Has the organization opted out of Copilot training data collection for private repositories?
- How are employees notified that their commit metadata (email, name) is stored in GitHub?
- What is the process for responding to a request to delete a specific user's data from GitHub?
- Are OAuth apps and GitHub Apps regularly reviewed for the data they access?
- How is long-term retention of Actions logs managed — are they exported before GitHub deletes them?

### Evidence to Collect

- Copilot training opt-out setting screenshot
- Data classification policy document referencing GitHub
- OAuth app and GitHub App access review
- GitHub's Data Processing Agreement (DPA) signed copy
- DSAR response process documentation

---

## 11. Networking & Connectivity

**Purpose:** Document the network configuration governing access to and from GitHub, including IP allowlists, proxy configurations, and runner network posture.

### What to Review

- IP allowlist: configured at enterprise or org level to restrict access to known IP ranges
- GitHub-hosted runner IP ranges: whether these are included in allowlists for Actions to function
- Self-hosted runner network placement: what they can reach (internet, internal services, production)
- Proxy configuration: HTTP proxy for GHES or runners behind corporate proxies
- Private networking for Actions: GitHub's private networking features for runners (verify current state in GitHub docs)
- VPN or private link requirements: whether GitHub must be accessed via VPN or private endpoint
- Firewall rules: outbound from developer workstations to GitHub, inbound webhook delivery

### Review Checklist

- [ ] IP allowlist configured and documented, with justification for each allowed range
- [ ] GitHub-hosted runner IP ranges accommodated in allowlist for Actions (or Actions restricted to self-hosted)
- [ ] Self-hosted runner network segmentation documented and reviewed
- [ ] Webhook delivery IP ranges allowlisted in receiving systems
- [ ] Proxy configuration documented for GHES or self-hosted runners (if applicable)
- [ ] Private networking for runners evaluated against internal service access requirements
- [ ] VPN or zero-trust access requirements for GitHub access documented
- [ ] Firewall change process includes GitHub IP range updates

### Key Stakeholder Questions

- Is the IP allowlist actively maintained when GitHub updates its IP ranges?
- Can self-hosted runners reach production systems, and is that access intentional and documented?
- Are developers required to use a VPN to access GitHub?
- How are webhooks delivered to internal systems — through a DMZ, reverse proxy, or directly?
- Is private networking for GitHub Actions runners in use or being evaluated?
- What is the process for updating firewall rules when GitHub changes its published IP ranges?
- Are there any known connectivity issues between development environments and GitHub?

### Evidence to Collect

- IP allowlist configuration screenshot
- Network diagram showing runner placement and connectivity
- Firewall rules relevant to GitHub traffic
- Proxy configuration documentation
- GitHub's published IP range documentation (meta API endpoint)

---

## 12. Integrations & APIs

**Purpose:** Document the APIs, webhooks, OAuth apps, and GitHub Apps integrated with GitHub Enterprise, and assess their access scopes, security posture, and operational ownership.

### What to Review

- GitHub Apps: installed apps at enterprise, org, and repo levels — permissions and event subscriptions
- OAuth Apps: authorized by users or orgs — scopes and last used dates
- PATs (Personal Access Tokens): used in integrations — scope, owner, expiry
- REST API usage: authenticated vs unauthenticated, rate limits, token types in use
- GraphQL API usage: same considerations as REST
- Webhooks: payloads, delivery targets, secrets for payload verification, failure handling
- Enterprise integrations: Jira, Slack, IDEs, CI/CD platforms — data shared and access granted
- API rate limit management: how integrations handle secondary rate limits

### Review Checklist

- [ ] All installed GitHub Apps inventoried with owner, permissions, and business justification
- [ ] OAuth Apps audited — stale or unused apps revoked
- [ ] PATs used in integrations inventoried and converted to GitHub Apps or fine-grained PATs where possible
- [ ] Webhook payload secrets configured for all webhooks
- [ ] Webhook delivery failures monitored and alerting configured
- [ ] API tokens rotated on a defined schedule
- [ ] Rate limit handling implemented in all custom integrations
- [ ] GitHub Apps follow principle of least privilege (minimum required permissions)
- [ ] Decommissioning process defined for removing integrations when no longer needed

### Key Stakeholder Questions

- Is there an approved process for requesting and installing GitHub Apps?
- Are there OAuth Apps with broad scopes (e.g., `repo` on all repos) that could be scoped down?
- How are API tokens managed for service accounts — are they stored in a secrets manager?
- Is webhook payload authenticity verified using HMAC signatures?
- Are there any integrations sending GitHub data to external vendors without a reviewed data processing agreement?
- Who is the operational owner of each integration, and what happens when they leave?
- Are there any undocumented or personal integrations running against production repositories?

### Evidence to Collect

- GitHub App installation list with permissions
- OAuth App authorization list
- PAT inventory (names, scopes, expiry — not values)
- Webhook list with delivery targets
- Integration architecture diagram
- Sample webhook payload verification implementation

---

## 13. Operational Management

**Purpose:** Document how GitHub Enterprise is operated day-to-day, including monitoring, alerting, incident response, backup, and disaster recovery procedures.

### What to Review

- Monitoring: GitHub status page subscriptions, uptime tracking, GHES health monitoring (if applicable)
- GHES operational management: patching cadence, admin console access, backup tool (GitHub Backup Utilities or equivalent), HA configuration
- Incident response: documented runbooks for GitHub-related incidents (outages, security events, data loss)
- Backup and recovery: backup frequency, recovery time objective (RTO), recovery point objective (RPO), tested restore process
- DR for GHES: high availability replica configuration, failover procedure
- Support: GitHub Enterprise Support tier, support contact list, SLA for critical tickets
- Change management: process for making enterprise or org-level configuration changes

### Review Checklist

- [ ] GitHub status page (githubstatus.com) subscribed to for relevant components
- [ ] GHES backup configured and tested within the last 90 days (if applicable)
- [ ] GHES HA replica configured and failover tested (if applicable)
- [ ] GHES patching schedule defined and current — no more than N versions behind (define N in policy)
- [ ] Incident runbooks documented for GitHub outage, security event, and data loss scenarios
- [ ] GitHub Enterprise Support tier reviewed against organizational SLA requirements
- [ ] Admin console access to GHES restricted and audited
- [ ] RTO and RPO defined and tested for GitHub service
- [ ] Change management process applied to GitHub enterprise configuration changes

### Key Stakeholder Questions

- Who is responsible for patching GHES, and what is the current version lag?
- Has a restore from backup been tested, and what was the result?
- What is the RTO and RPO for GitHub, and has it been tested?
- What is the escalation path for a GitHub outage during business hours? After hours?
- What GitHub Support tier is active, and what is the SLA for P1 tickets?
- Are GHES admin console credentials managed in a privileged access management (PAM) tool?
- Is there a change advisory board process for GitHub enterprise configuration changes?
- How is GHES capacity (CPU, memory, storage) monitored?

### Evidence to Collect

- GHES backup configuration and last successful backup timestamp
- GHES version and patch history
- Incident runbooks for GitHub
- GitHub Support tier documentation
- HA configuration documentation (if applicable)
- RTO/RPO documentation and last DR test results

---

## 14. Migration & Adoption

**Purpose:** Document the approach to migrating to GitHub Enterprise, onboarding teams, and driving consistent adoption across the organization.

### What to Review

- Migration source: current or prior SCM platforms (GitLab, Bitbucket, Azure DevOps, other GitHub instances)
- Migration tooling: GitHub Enterprise Importer (GEI), `gl-exporter`, `bbs2gh`, or custom tooling
- Migration scope: what is in scope (repos, wikis, issues, PRs, Actions, packages, secrets)
- Validation approach: how migrated repositories are verified post-migration
- Phased rollout plan: pilot teams, early adopters, general availability, forced cutover
- Training and enablement: developer training resources, internal champions, office hours
- Change management: stakeholder communication, resistance patterns, feedback loops
- Cutover approach: hard cutover vs parallel operation, DNS/redirect strategy

### Review Checklist

- [ ] Source platform inventory completed (repos, users, integrations, pipelines)
- [ ] Migration tooling selected and tested against representative data
- [ ] Migration scope documented — explicit list of what is and is not migrated
- [ ] Post-migration validation checklist defined and executed for pilot migrations
- [ ] Phased rollout plan documented with milestones and owners
- [ ] Developer training materials prepared and reviewed for accuracy
- [ ] Internal champion network identified
- [ ] Stakeholder communication plan drafted and approved
- [ ] Cutover runbook documented and rehearsed
- [ ] Rollback plan documented in case of migration failure

### Key Stakeholder Questions

- Which teams are highest risk for migration, and why?
- Are there integrations on the source platform that have no equivalent on GitHub?
- What is the acceptable window for a migration-related outage or freeze?
- How will active PRs or in-flight work be handled at cutover?
- Are there large binary files or LFS-heavy repositories that require special handling?
- What does success look like at 30, 60, and 90 days post-migration?
- How will resistance from teams comfortable with the old platform be addressed?
- Is there a plan for sunsetting the source platform after migration?

### Evidence to Collect

- Source platform repository and user inventory
- Migration tooling selection documentation
- Pilot migration results and validation reports
- Rollout plan document with milestones
- Training materials
- Cutover runbook
- Post-migration validation checklist

---

## 15. Cost & Licensing Review

**Purpose:** Document GitHub's licensing model, current spend, and optimization opportunities to ensure licenses are right-sized and cost drivers are understood.

### What to Review

- License tier: GitHub Enterprise Cloud vs Enterprise Server, with or without GitHub Advanced Security, Copilot
- Seat count: licensed seats vs active users (GitHub defines "active" — verify current definition in GitHub docs)
- Billing model: per-seat, consumption (Actions minutes, storage, Packages bandwidth), add-ons
- Actions consumption: minutes used by runner type (Linux, Windows, macOS, larger runners)
- Packages and storage: packages bandwidth and storage costs
- Copilot seats: assigned vs active users
- GHAS seats: enabled vs licensed
- Cost optimization levers: self-hosted runners, storage cleanup, seat reclamation

### Review Checklist

- [ ] License agreement reviewed and renewal date tracked
- [ ] Active user count reconciled against licensed seat count
- [ ] Actions minutes consumption reviewed by runner type
- [ ] Packages storage and bandwidth costs reviewed
- [ ] Copilot seat assignments reviewed — unused seats identified
- [ ] GHAS enablement reconciled against GHAS seat license count
- [ ] Self-hosted runners evaluated as cost reduction for high-volume Actions workloads
- [ ] Overage alerts configured for Actions minutes and storage
- [ ] Cost allocation model defined — are GitHub costs chargeback to teams?

### Key Stakeholder Questions

- What is the current monthly or annual GitHub spend, broken down by component?
- Are there users with GitHub seats who have not been active in the last 90 days?
- What percentage of Actions usage is on expensive runner types (macOS, Windows, larger runners) that could be replaced with Linux?
- Is there a defined process for reclaiming Copilot or GHAS seats from inactive users?
- Are there repositories generating significant Actions minutes that could be optimized?
- How is GitHub cost allocated or reported internally?
- What is the cost impact of enabling GHAS on all repositories?
- Are there alternative runners (self-hosted, third-party) being evaluated for cost reduction?

### Evidence to Collect

- GitHub billing dashboard screenshot
- License agreement and seat count
- Actions usage report by runner type
- Copilot and GHAS seat assignment reports
- Packages storage and bandwidth usage report
- Cost forecast for next contract period

---

## 16. Risks, Tradeoffs, and Decision Checklist

**Purpose:** Consolidate identified risks, platform tradeoffs, and outstanding decisions into a single reference for governance review and executive sign-off.

### What to Review

- Open risks from each prior section
- Architectural tradeoffs: GHEC vs GHES, EMU vs standard, self-hosted vs GitHub-hosted runners
- Security control gaps: areas where GitHub alone is insufficient and compensating controls are needed
- Compliance gaps: outstanding items before GitHub can be used for specific data classifications
- Vendor dependency: reliance on GitHub as a single platform for CI/CD, SCM, security scanning, and AI

### Review Checklist

- [ ] Risk register created or updated with GitHub-specific risks
- [ ] Each risk assigned an owner and a remediation or acceptance decision
- [ ] All architectural decisions documented with rationale, alternatives considered, and date decided
- [ ] Compliance gaps documented with target remediation date
- [ ] Compensating controls documented where GitHub native controls are insufficient
- [ ] Vendor risk assessment for GitHub (Anthropic/Microsoft ownership chain) completed
- [ ] Business continuity plan includes GitHub dependency
- [ ] Outstanding decisions escalated to appropriate stakeholders

### Key Stakeholder Questions

- What are the top three risks in the current GitHub configuration?
- Are there any compliance requirements that GitHub cannot meet natively?
- What is the risk of vendor lock-in, and is there a viable migration path if needed?
- Are there workloads that should not be on GitHub, and have those decisions been documented?
- Who has authority to accept residual risk for identified gaps?
- Is there a defined review cadence for this risk register?
- Are compensating controls formally documented and tested?

### Decision Log Template

| Decision | Options Considered | Decision Made | Rationale | Owner | Date |
|---|---|---|---|---|---|
| Deployment model | GHEC / GHES / Hybrid | | | | |
| Identity model | EMU / Standard | | | | |
| Runner strategy | GitHub-hosted / Self-hosted / Mixed | | | | |
| GHAS rollout | All repos / High-risk repos only | | | | |
| Copilot enablement | All / Opt-in / Disabled | | | | |

### Evidence to Collect

- Risk register document
- Decision log
- Compensating controls documentation
- Vendor risk assessment report

---

## 17. Additional GitHub Services & Features

This section covers each remaining GitHub service and feature relevant to enterprise environments.

---

### 17.1 GitHub Agents

**Purpose:** Document the use of agentic AI capabilities in GitHub, including Copilot agents, coding agents, and any GitHub-native agent workflows. (Verify current state and feature availability in GitHub docs — this area is evolving rapidly.)

#### What to Review

- Which agent features are enabled (e.g., Copilot coding agent, custom agents)
- How agents are authorized to act on repositories (token scope, branch targeting)
- Review and approval gates before agent-generated changes are merged
- Data sent to external model providers as part of agent task execution
- Logging and auditability of agent actions

#### Review Checklist

- [ ] Agent feature availability confirmed and matched to licensed tier
- [ ] Agent permissions scoped to minimum required access
- [ ] Human review required before any agent-generated PR is merged to a protected branch
- [ ] Data handling implications of agent task execution reviewed
- [ ] Acceptable use policy for agents communicated to developers

#### Key Stakeholder Questions

- Are any automated agents currently operating with write access to production repositories?
- What review gates exist before agent-generated code is merged?
- Is there an audit trail of actions taken by Copilot or other agents?
- Has the security team reviewed the permissions granted to agentic workflows?
- What is the policy for using agents on repositories with sensitive or regulated data?

#### Evidence to Collect

- Agent configuration documentation
- Sample agent-generated PR with review history
- Permissions granted to agent tokens or apps

---

### 17.2 AI Models in GitHub

**Purpose:** Document which AI models are accessible through GitHub products (Copilot, Copilot Chat, code scanning AI features) and the data handling implications of each. (Verify current model availability and configuration options in GitHub docs.)

#### What to Review

- Models available through Copilot (default model, alternative models if selectable — verify current state)
- Whether users can select their own model or model selection is centrally controlled
- Data handling differences between models (e.g., first-party vs third-party model providers)
- Enterprise controls for restricting model access

#### Review Checklist

- [ ] Default model used by Copilot documented
- [ ] Model selection policy defined (centrally controlled vs user choice)
- [ ] Data handling reviewed for each available model
- [ ] Enterprise model restriction controls configured if available (verify current state)

#### Key Stakeholder Questions

- Which AI models are accessible through GitHub Copilot in our enterprise?
- Do developers have the ability to switch models, and is that appropriate?
- Are there data handling differences between models that affect compliance?
- Is model usage logged in the enterprise audit trail?

#### Evidence to Collect

- Copilot model configuration screenshots
- GitHub documentation on model data handling (verify current state)

---

### 17.3 GitHub Codespaces

**Purpose:** Document the use, configuration, and security posture of GitHub Codespaces as a cloud-based development environment.

#### What to Review

- Codespaces enablement: enabled at org level, per user or all members
- Machine type policy: which machine sizes are allowed (cost and resource governance)
- Idle timeout and retention policies: how long codespaces persist before automatic deletion
- Secrets in Codespaces: how repository and user secrets are injected
- Network access: what Codespaces instances can reach (internet, internal services)
- Cost: Codespaces compute and storage consumption
- Dotfiles and dev container configurations: standardization across teams

#### Review Checklist

- [ ] Codespaces enablement policy defined (who can use, for which repos)
- [ ] Machine type restrictions configured to prevent unnecessary cost
- [ ] Idle timeout and maximum retention configured
- [ ] Codespaces usage and cost monitored
- [ ] Secrets injected into Codespaces reviewed for appropriate scoping
- [ ] Dev container configurations maintained for key repositories
- [ ] Network access from Codespaces reviewed and limited where needed

#### Key Stakeholder Questions

- Are developers using Codespaces for production-sensitive work, and is that appropriate?
- How is Codespaces cost tracked and attributed to teams?
- Are there internal services that Codespaces needs to access, and how is that access managed?
- What happens to a Codespace and its contents when it expires or is deleted?
- Are dotfiles or dev containers standardized to reduce onboarding friction?

#### Evidence to Collect

- Codespaces policy configuration screenshots
- Codespaces usage and cost report
- Dev container configuration examples

---

### 17.4 GitHub Packages

**Purpose:** Document the use of GitHub Packages as an artifact registry for containers, npm, Maven, NuGet, and other package types.

#### What to Review

- Registries in use: container (GHCR), npm, Maven, Gradle, NuGet, RubyGems
- Access control: public vs private packages, token permissions for publishing
- Retention and cleanup: untagged images, stale packages — is there a lifecycle policy?
- Cost: storage and egress costs for Packages
- Security scanning: whether published images are scanned for vulnerabilities
- Trusted publishing: OIDC-based publishing configured vs long-lived tokens

#### Review Checklist

- [ ] Package registries in use documented
- [ ] Publishing permissions restricted (not all users can publish to production registries)
- [ ] Package retention and cleanup policy configured
- [ ] Container images scanned for vulnerabilities before publication
- [ ] Packages storage and egress costs monitored
- [ ] OIDC-based trusted publishing used where possible
- [ ] Public package publishing policy defined

#### Key Stakeholder Questions

- Are any packages publicly visible that should not be?
- How are stale or untagged container images cleaned up?
- Who has permission to publish packages, and how is this governed?
- Are published container images scanned for known CVEs?
- Is the Packages registry the source of truth for internal artifacts, or is it one of several?

#### Evidence to Collect

- Package registry list with visibility settings
- Retention policy configuration
- Publishing token and permissions inventory
- Packages cost report

---

### 17.5 GitHub Pages

**Purpose:** Document the use of GitHub Pages for publishing static sites from repositories.

#### What to Review

- Pages enablement policy: which repositories or orgs can publish Pages
- Visibility: public vs private Pages (private Pages availability — verify current state in GitHub docs)
- Custom domains: configured and HTTPS enforced
- Build sources: Actions-based builds vs legacy branch-based builds
- Content review: who approves what is published via Pages

#### Review Checklist

- [ ] Pages enablement policy defined (not open to all repositories by default)
- [ ] All Pages sites use HTTPS with valid certificates
- [ ] Custom domain ownership verified and documented
- [ ] Content published via Pages reviewed for appropriateness and data classification
- [ ] Actions-based Pages builds use pinned Actions and reviewed workflows

#### Key Stakeholder Questions

- Is there an approval process for enabling Pages on a repository?
- Are there Pages sites publishing internal information publicly?
- How are Pages sites monitored for content compliance?
- Are Pages used as a deployment target for any customer-facing content?

#### Evidence to Collect

- List of repositories with Pages enabled and their visibility
- Custom domain configuration
- Pages deployment workflow examples

---

### 17.6 Gists

**Purpose:** Document the governance of GitHub Gists, which can be used to share code snippets publicly or privately.

#### What to Review

- Gist creation policy: can users create public gists under the enterprise identity?
- Content in gists: risk of sensitive data, credentials, or proprietary code being shared via public gists
- Visibility: public gist creation policy at enterprise or org level

#### Review Checklist

- [ ] Gist creation policy reviewed — public gist creation restricted or monitored
- [ ] Developers informed of gist data handling policies
- [ ] Secret scanning coverage for gists reviewed (verify current state in GitHub docs)

#### Key Stakeholder Questions

- Are developers aware that public gists are visible to anyone, including outside the enterprise?
- Has secret scanning been configured to cover gists?
- Is there an acceptable use policy for gists?

#### Evidence to Collect

- Gist policy configuration
- Secret scanning configuration for gists

---

### 17.7 GitHub Discussions

**Purpose:** Document the use of GitHub Discussions as an asynchronous communication forum within repositories or organizations.

#### What to Review

- Discussions enablement: which repositories have Discussions enabled
- Moderation: who can moderate discussions, and is there a moderation policy
- Data retention: discussions as organizational knowledge — archiving and export
- Integration with internal tools: whether discussions are linked to ticketing or documentation systems

#### Review Checklist

- [ ] Discussions enabled only on repositories where the feature adds value
- [ ] Moderation roles assigned and policy documented
- [ ] Discussions content reviewed for sensitive information
- [ ] Long-term retention strategy for discussions defined

#### Key Stakeholder Questions

- Are Discussions used for decision-making that should be documented elsewhere?
- Is there a process for moderating or escalating problematic discussions?
- How are discussions archived or exported if a repository is migrated or archived?

#### Evidence to Collect

- Discussions enablement list
- Moderation policy documentation

---

### 17.8 GitHub Apps

**Purpose:** Document the GitHub Apps ecosystem — first-party and third-party — installed across the enterprise, including their permissions, event subscriptions, and operational ownership.

#### What to Review

- Installed GitHub Apps at enterprise, org, and repo levels
- Permissions granted: repository permissions, organization permissions, user permissions
- Event subscriptions: which webhook events each app receives
- Authentication method: GitHub App installation tokens (preferred) vs PATs
- Data handling: what data each app accesses and transmits
- App ownership and decommissioning process

#### Review Checklist

- [ ] Complete inventory of installed GitHub Apps maintained
- [ ] Each app has a documented owner and business justification
- [ ] Permissions reviewed for least-privilege compliance
- [ ] Unused or stale apps revoked
- [ ] Decommissioning process documented
- [ ] Third-party apps reviewed for data handling and privacy compliance

#### Key Stakeholder Questions

- Is there a formal approval process before installing a GitHub App?
- Are there GitHub Apps with `contents: write` access to all repositories?
- How are GitHub App installation tokens managed and rotated?
- Who is responsible for reviewing and revoking stale GitHub Apps?

#### Evidence to Collect

- GitHub App installation list with permissions
- Approval and review process documentation

---

### 17.9 Scheduled Reminders

**Purpose:** Document the use of GitHub's scheduled reminders feature to notify teams about pending pull request reviews via Slack or Microsoft Teams.

#### What to Review

- Which teams have scheduled reminders configured
- Integration with Slack or Microsoft Teams: OAuth app permissions, data sent to communication platforms
- Reminder frequency and content: what information is included in reminder messages

#### Review Checklist

- [ ] Scheduled reminders configured for teams with high PR review volume
- [ ] OAuth integration with Slack/Teams reviewed for appropriate scopes
- [ ] Reminder content reviewed — no sensitive data exposed in notification payloads

#### Key Stakeholder Questions

- Are scheduled reminders reducing PR review latency measurably?
- What data is sent to Slack or Teams in reminder notifications?
- Are reminder integrations included in OAuth app review cycles?

#### Evidence to Collect

- Scheduled reminders configuration per team
- Slack/Teams OAuth app permission review

---

### 17.10 GitHub Projects

**Purpose:** Document the use of GitHub Projects (Projects v2) for work tracking, planning, and workflow automation across repositories and organizations.

#### What to Review

- Project visibility: public vs private vs org-internal
- Automation: built-in automation rules and custom workflows in projects
- Integration with issues and PRs: cross-repository project tracking
- Data in projects: custom fields, sensitive milestone or roadmap information
- Access control: who can view and edit projects

#### Review Checklist

- [ ] Project visibility policy defined — sensitive roadmap projects are not public
- [ ] Project automation rules reviewed for unintended side effects
- [ ] Access permissions for sensitive projects restricted appropriately
- [ ] Data classification applied to project content (roadmaps, capacity, financial milestones)

#### Key Stakeholder Questions

- Are there projects containing roadmap or financial information that should not be publicly visible?
- How are projects used alongside existing project management tools (Jira, Linear, etc.)?
- Is there a governance process for archiving or closing completed projects?

#### Evidence to Collect

- Project inventory with visibility settings
- Sample project automation configuration

---

### 17.11 GitHub Insights

**Purpose:** Document the use of GitHub Insights (Enterprise Insights) for engineering metrics and productivity analysis.

#### What to Review

- Insights availability: which tier includes Insights (verify current state in GitHub docs)
- Metrics tracked: PR cycle time, review time, deployment frequency, contributor activity
- Data access: who can view Insights data, and what level of individual contributor visibility exists
- Privacy considerations: individual-level metrics and their implications for psychological safety

#### Review Checklist

- [ ] Insights availability confirmed for current license tier
- [ ] Access to Insights restricted to engineering leadership and approved stakeholders
- [ ] Policy defined for how Insights data is used (aggregate team health vs individual performance)
- [ ] Privacy implications of individual-level metrics communicated to developers

#### Key Stakeholder Questions

- How is Insights data used — for team health or individual performance evaluation?
- Are developers informed that their activity metrics are tracked?
- Is Insights data shared outside the engineering organization?
- What actions are taken based on Insights data?

#### Evidence to Collect

- Insights access configuration
- Policy document on Insights data use

---

### 17.12 GitHub Sponsors (Enterprise Relevance)

**Purpose:** Document whether GitHub Sponsors is relevant to the enterprise context — primarily relevant if the organization maintains open-source projects or funds open-source contributors.

#### What to Review

- Whether the enterprise funds open-source maintainers via Sponsors
- Whether enterprise repositories are set up to receive sponsorships
- Financial compliance implications of Sponsors transactions

#### Review Checklist

- [ ] Sponsors usage reviewed against financial and procurement policy
- [ ] Any Sponsors-enabled repositories reviewed for appropriateness

#### Key Stakeholder Questions

- Does the organization use GitHub Sponsors to fund open-source dependencies?
- Are there enterprise repositories set up to receive sponsorships, and is that appropriate?

#### Evidence to Collect

- Sponsors configuration on enterprise repositories

---

*End of Document*

---

> **Maintenance note:** This document should be reviewed and updated on a defined cadence (recommended: quarterly) or when significant GitHub feature releases, organizational changes, or audit findings require it. All "verify current state in GitHub docs" callouts should be resolved against [docs.github.com](https://docs.github.com) at the time of review.

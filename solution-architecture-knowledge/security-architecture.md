# Security Architecture

> Threat-modeling, zero-trust, and secure-by-default patterns a senior consultant brings to every engagement.

---

## 1. The Mental Model: Defense in Depth + Zero Trust

| Layer | Controls |
|-------|----------|
| Identity | SSO, MFA, conditional access, EMU |
| Device | Managed, attested, posture-checked |
| Network | mTLS, private networking, egress control |
| Application | OWASP ASVS, input validation, authz checks |
| Data | Encryption at rest/in transit, classification, DLP |
| Operations | Logging, monitoring, IR, BCP |
| Supply Chain | Signed artifacts, SBOM, provenance |

Zero Trust principle: **never trust, always verify**, **assume breach**, **least privilege**.

---

## 2. Threat Modeling — STRIDE

For each component, ask:

| Letter | Threat | Mitigation Lens |
|--------|--------|-----------------|
| **S**poofing | Identity faking | Strong auth, signing |
| **T**ampering | Data alteration | Integrity, signatures, RBAC |
| **R**epudiation | Denial of action | Audit logs, non-repudiation |
| **I**nformation Disclosure | Data leak | Encryption, least privilege |
| **D**enial of Service | Availability loss | Rate limit, autoscale, isolation |
| **E**levation of Privilege | Unauthorized escalation | Authz, principle of least privilege |

Run STRIDE on a **C4 L2 diagram** for every new system or significant change.

---

## 3. Threat Modeling — PASTA (when you need depth)

Process for Attack Simulation and Threat Analysis — 7 stages from business objectives through attack trees to countermeasures. Reserve for high-stakes systems (payments, healthcare, identity).

---

## 4. Secure Software Development Lifecycle (SSDLC)

| Phase | Practice |
|-------|----------|
| Plan | Threat model, abuse cases, security stories |
| Build | Secure coding standards, IDE security plugins |
| Verify | SAST (CodeQL), DAST, SCA, secrets scan, IaC scan |
| Release | SBOM, signing (cosign / Sigstore), provenance (SLSA) |
| Operate | Runtime detection, vuln management, IR |
| Govern | Audit, compliance evidence, policy-as-code |

In GitHub: this is **GHAS + Actions + Rulesets + Audit Log**.

---

## 5. Zero Trust on GitHub

- SSO + SCIM with conditional access
- Enterprise Managed Users for high-isolation contexts
- IP allow lists where regulation requires
- Signed commits required (Sigstore / GPG)
- Required CODEOWNERS + ≥1 reviewer
- OIDC to clouds — no long-lived secrets
- Push protection for secrets
- Required workflows for security checks

---

## 6. Cryptography Defaults

| Use | Default |
|-----|---------|
| TLS | 1.3, ECDHE, AEAD |
| At-rest | AES-256-GCM (envelope) |
| Hashing (passwords) | Argon2id (or bcrypt cost ≥12) |
| Tokens | JWT with rotating keys; prefer short TTL + refresh |
| Signing | Ed25519 / ECDSA P-256; Sigstore for artifacts |

Never roll your own crypto. Use the platform.

---

## 7. Secrets Management

Hierarchy of preference:

1. Workload identity / OIDC federation (no secret)
2. Short-lived tokens from a vault (HashiCorp Vault, AWS STS, Azure Identity)
3. GitHub Environments + secrets, scoped per env
4. Org-level secrets — only for shared, low-sensitivity values

Never:

- Commit secrets (push protection on)
- Use long-lived PATs in CI
- Use a single shared key across environments

---

## 8. Identity Patterns

- **OIDC** for app-to-cloud
- **SAML/OIDC SSO** for human-to-app
- **mTLS** for service-to-service in trusted networks
- **SPIFFE/SPIRE** for workload identity at scale
- **Token exchange (RFC 8693)** for delegation

---

## 9. Supply Chain (SLSA + SBOM)

| SLSA Level | What It Means |
|------------|---------------|
| 1 | Build process documented |
| 2 | Version-controlled, hosted build |
| 3 | Hardened build platform, provenance |
| 4 | Two-party review, hermetic builds |

Pair with:

- SBOM in SPDX or CycloneDX
- Sigstore signing on artifacts
- `actions/attest-build-provenance`
- Dependency review in PRs

---

## 10. Common Compliance Mappings

See [compliance-and-governance.md](./compliance-and-governance.md). Key ones:

- ISO 27001 (Annex A)
- SOC 2 Trust Services Criteria
- NIST 800-53 / 800-218 (SSDF)
- PCI-DSS v4
- HIPAA Security Rule
- DORA (EU)
- EU AI Act

---

## 11. References

- [GitHub Advanced Security](../README.md#23-security--advanced-security-ghas)
- [Platform Engineering](./platform-engineering.md)
- [Resilience Patterns](./resilience-patterns.md)

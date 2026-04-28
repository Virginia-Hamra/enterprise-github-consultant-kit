# GHAS Readiness Checklist

Owner: Security Advisor. Use before broad GHAS enablement.

## Licensing & scope
- [ ] GHAS seats sufficient for active committers in target repos
- [ ] Repo cohorts identified (pilot → tier 1 → tier 2)
- [ ] Exemption / exception process defined

## Pre-flight cleanup
- [ ] Existing secrets remediated where feasible
- [ ] Triage owners assigned per repo / org (CODEOWNERS-aligned)
- [ ] SLA matrix by severity agreed

## Configuration
- [ ] Secret scanning + push protection enabled
- [ ] Custom patterns reviewed & deployed
- [ ] Dependabot alerts + version updates configured
- [ ] CodeQL: default setup vs. advanced decided per language
- [ ] Security policy (`SECURITY.md`) standard defined

## Operational
- [ ] Alert routing to ticketing / SIEM
- [ ] Dashboards / metrics defined
- [ ] Triage runbook published
- [ ] Developer enablement materials prepared

## Validation
- [ ] Test secrets blocked by push protection (canary test)
- [ ] CodeQL produces alerts on known-vulnerable test repo
- [ ] Dependabot PRs auto-merge rules tested in pilot

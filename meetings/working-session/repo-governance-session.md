# Repository Governance Working Session

> **Duration:** 2–3h &nbsp; | &nbsp; **Driver:** customer platform engineer

## Objective
Deploy org-wide rulesets via Terraform and validate enforcement on a pilot repo.

## Definition of Done
- [ ] Terraform module for ruleset checked into IaC repo
- [ ] PR-applied to one org with required reviews + status checks + signed commits + linear history
- [ ] Bypass list scoped to break-glass only
- [ ] Pilot repo demonstrates enforcement
- [ ] Drift-detection workflow scheduled

## Pre-requisites
- [ ] Org owner access
- [ ] IaC repo with Terraform + provider configured
- [ ] Approval to apply ruleset
- [ ] Pilot repo identified

## Step-by-step
1. Author ruleset in Terraform module
2. `terraform plan` → review with customer
3. PR with plan output attached
4. Approve & apply
5. Test enforcement on pilot repo:
   - Attempt force-push → blocked
   - Open PR without required check → blocked from merge
   - Bypass attempt by non-listed user → denied
6. Configure drift-detection workflow (scheduled `terraform plan`)

## Validation
- [ ] Force-push blocked
- [ ] Direct push to default branch blocked
- [ ] PR merge blocked without checks / reviews
- [ ] Bypass list correct
- [ ] Drift workflow successfully runs

## Rollback
1. `terraform destroy` on the ruleset resource
2. (Or) revert PR & re-apply

## Risks
- Locking out automation accounts → audit existing automation prior; add to bypass if needed (and document)

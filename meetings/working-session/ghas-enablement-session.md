# GHAS Enablement Working Session

> **Duration:** 2h

## Objective
Enable GHAS (secret scanning + push protection, CodeQL default setup, Dependabot) for a pilot cohort and validate alert flow.

## Definition of Done
- [ ] GHAS enabled on all pilot repos
- [ ] Push protection enforced
- [ ] CodeQL default setup configured for primary languages
- [ ] Dependabot alerts + security updates enabled
- [ ] Triage owners assigned (CODEOWNERS-aligned)
- [ ] One synthetic alert validated end-to-end

## Pre-requisites
- [ ] GHAS seats available
- [ ] Pilot cohort agreed
- [ ] Triage SLAs defined
- [ ] Sample known-vulnerable test repo

## Step-by-step
1. Confirm seat allocation
2. Enable GHAS at org or per-repo level
3. Enable secret scanning + push protection
4. Configure CodeQL default setup (or advanced where required)
5. Enable Dependabot alerts + version updates
6. Confirm CODEOWNERS / triage assignment
7. Synthetic test:
   - Push fake secret → blocked
   - Open PR with vulnerable dep → Dependabot alert
   - Vulnerable code path → CodeQL alert

## Validation
- [ ] Synthetic secret push blocked
- [ ] CodeQL scan completes & finds known issue
- [ ] Dependabot PRs generated
- [ ] Alerts visible in security overview

## Rollback
- Disable GHAS at org / repo level
- Document any in-flight alerts before rollback

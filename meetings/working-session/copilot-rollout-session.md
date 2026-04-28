# Copilot Pilot Rollout Working Session

> **Duration:** 1.5h

## Objective
Enable Copilot for the pilot cohort with policies, content exclusions, and audit aligned to enterprise standards.

## Definition of Done
- [ ] Pilot cohort assigned seats
- [ ] Policies applied: public code matching, content exclusions
- [ ] Audit streaming captures Copilot events
- [ ] Pilot users confirmed working in IDE + chat
- [ ] Office hours scheduled

## Pre-requisites
- [ ] Legal / IP / Privacy approvals on file
- [ ] Pilot roster agreed
- [ ] Acceptable use policy published
- [ ] List of repos / paths needing content exclusion

## Step-by-step
1. Enable Copilot Business / Enterprise on the org(s)
2. Configure organization policies (public code matching, network, etc.)
3. Configure content exclusions (paths, file types, repos)
4. Assign seats to pilot cohort (via team or individuals)
5. Validate audit log captures `copilot.*` events
6. Smoke test with 2–3 pilot users (inline + chat)
7. Schedule training + office hours

## Validation
- [ ] Pilot users see Copilot active in IDE
- [ ] Excluded paths suppress suggestions (sample test)
- [ ] Audit events for sign-in + usage visible
- [ ] Acceptance / satisfaction baselining started

## Rollback
- Remove seat assignments; org-level config can remain

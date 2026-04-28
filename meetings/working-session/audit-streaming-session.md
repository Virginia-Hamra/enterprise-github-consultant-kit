# Audit Log Streaming Working Session

> **Duration:** 2h

## Objective
Configure GitHub enterprise audit log streaming to the customer's SIEM and validate end-to-end ingestion + alerting.

## Definition of Done
- [ ] Streaming destination configured (Splunk / Sentinel / S3 / Datadog / Azure Event Hubs)
- [ ] Events flowing within retention SLO
- [ ] Critical-event alerts firing (test cases pass)
- [ ] Retention policy aligned with compliance requirement

## Pre-requisites
- [ ] Enterprise owner access
- [ ] SIEM admin access
- [ ] Approved destination & encryption settings
- [ ] Critical-event list agreed (SSO disable, owner add, ruleset bypass, secret found, ...)

## Step-by-step
1. Create destination (e.g. S3 bucket with KMS, Splunk HEC token, Sentinel workspace)
2. Configure GitHub streaming with destination + credentials
3. Verify pause/resume behavior
4. Generate test events (e.g. add then remove an org owner in a sandbox org)
5. Confirm events visible in SIEM
6. Author alert rules for the agreed critical events
7. Trigger one test event per alert; confirm alert fires

## Validation
- [ ] Test events visible in SIEM within expected window
- [ ] Each critical-event alert fires on test
- [ ] Retention policy applied & verified

## Rollback
- Pause streaming; destination remains intact

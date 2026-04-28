# Actions Pipeline Migration Working Session

> **Duration:** 3h

## Objective
Migrate one representative pipeline from the current CI tool to GitHub Actions using the reusable workflow library.

## Definition of Done
- [ ] Pipeline migrated to GitHub Actions
- [ ] Uses org-shared reusable workflow(s) where applicable
- [ ] Build + test parity with source pipeline (artifacts identical)
- [ ] OIDC for any cloud auth (no long-lived secrets)
- [ ] Branch protection requires the new workflow status check

## Pre-requisites
- [ ] Source pipeline definition exported
- [ ] Target repo identified
- [ ] Reusable workflows library available
- [ ] Runner strategy decided (hosted / larger / self-hosted / ARC)
- [ ] OIDC trust pre-configured in cloud

## Step-by-step
1. Map source steps → Actions equivalents
2. Author workflow `.github/workflows/ci.yml` with `permissions:` minimized
3. Reference reusable workflows where possible; SHA-pin third-party Actions
4. Configure secrets / environments as needed
5. Trigger run → debug
6. Compare artifacts vs. source pipeline
7. Update branch protection to require new workflow check

## Validation
- [ ] Workflow succeeds on target repo
- [ ] Artifacts match source
- [ ] OIDC token exchange works (audit cloud trail)
- [ ] Required check enforced

## Rollback
- Disable / delete the workflow
- Re-enable source pipeline trigger

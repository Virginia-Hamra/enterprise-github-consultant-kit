# OIDC Cloud Auth Working Session

> **Duration:** 2h

## Objective
Establish OIDC trust between GitHub Actions and the customer's cloud account(s) so workflows authenticate without long-lived secrets.

## Definition of Done
- [ ] OIDC trust policy created with least-privilege scoping (`repo:`, `ref:`, `environment:` claims)
- [ ] Sample workflow assumes the role and runs a read-only operation
- [ ] No long-lived cloud credential present in any GitHub secret used by the workflow
- [ ] Audit trail captured in cloud (STS event)

## Pre-requisites
- [ ] Cloud admin access (AWS / Azure / GCP)
- [ ] Target repo / org for trust scoping
- [ ] Naming conventions agreed for roles

## Step-by-step
### AWS path
1. Create IAM OIDC provider for `token.actions.githubusercontent.com`
2. Create IAM role with trust policy scoped to `repo:org/repo:ref:refs/heads/main`
3. Attach minimum permissions
4. In workflow: `permissions: id-token: write`, use `aws-actions/configure-aws-credentials@<sha>`
5. Run workflow; confirm STS `AssumeRoleWithWebIdentity` event in CloudTrail

### Azure path
1. Create app registration & federated credential matching repo / branch
2. Assign minimum RBAC role on subscription / RG
3. In workflow: use `azure/login@<sha>` with `client-id`, `tenant-id`, `subscription-id`
4. Confirm sign-in event in Entra audit logs

### GCP path
1. Create Workload Identity Pool & Provider
2. Bind service account with attribute condition
3. In workflow: use `google-github-actions/auth@<sha>`
4. Confirm token exchange in Cloud Audit Logs

## Validation
- [ ] Workflow assumes role successfully
- [ ] Cloud audit log shows the OIDC assumption
- [ ] No static credentials present

## Rollback
- Detach role from OIDC provider, or remove federated credential

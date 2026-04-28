# 12 — Integrations & APIs

> The integration surface of GitHub: REST, GraphQL, Webhooks, GitHub Apps, OAuth Apps, PATs.

---

## Overview

| Surface | When to use |
|---------|-------------|
| **GitHub Apps** | Default for any service-to-GitHub integration |
| **OAuth Apps** | User-context integrations only |
| **Fine-grained PAT** | Scripts a user runs as themselves |
| **Classic PAT** | Avoid; phase out |
| **Webhooks** | Event-driven integration |
| **REST API** | Stable, well-documented for most ops |
| **GraphQL API** | Complex shapes, fewer round-trips |
| **MCP servers** | Agent-to-tool integration |

---

## Configuration

### GitHub Apps

- One app per integration; minimum permissions.
- Installation tokens expire in 1 hour — refresh, don't cache.
- Private key stored in a **secrets manager**, never in source.
- Multiple active keys supported for zero-downtime rotation.

### OAuth Apps

- Org owner restricts which OAuth Apps are authorizable.
- Quarterly review.

### PAT policies

- Org policy: require approval for fine-grained PATs with write access.
- Max-expiration policy.
- Audit and revoke classic PATs.

### Webhooks

- Always set a payload secret.
- Verify `X-Hub-Signature-256` on every request.
- Idempotent handlers (GitHub retries).
- Use enterprise / org webhooks for cross-cutting events.

### Rate limits

- Primary (per hour) + secondary (burst, concurrency).
- Implement exponential backoff with jitter.
- Use the GraphQL API's point budget for analytics workloads.

---

## Usage

- New internal integrations = GitHub Apps.
- CI / automation status updates = `GITHUB_TOKEN` (Actions).
- Bulk data export = GraphQL with cursor pagination.
- Event-driven ops = webhooks, never polling.

---

## Best Practices

- Maintain an **integration registry**: name, purpose, app id, permissions, owner, last review.
- Rotate App private keys on a schedule.
- Use **fine-grained PATs** instead of classic when an App isn't viable.
- Verify webhook signatures unconditionally.
- Use GraphQL Explorer for prototyping queries.

---

## Common Pitfalls

- Machine-user PAT integrations that break when the human leaves.
- OAuth Apps with `repo` scope = de-facto org-wide read/write.
- Webhooks accepted without signature verification.
- App private keys in source / unencrypted S3 / plain CI secrets.
- Polling every minute instead of subscribing via webhook.

---

## Implementation Notes

- For **enterprise iteration** (all repos in all orgs), use GraphQL + cursor pagination + batch processing — sequential REST takes hours.
- Build a token-acquisition library for GitHub Apps; never let teams reimplement JWT signing per service.
- Consider a **central webhook fan-out service** — one endpoint receives, validates, and routes to internal consumers.
- For MCP servers integrated with Copilot: see [Copilot Extensions](../09-copilot-in-the-enterprise/README.md).

---

## Sources

- [GitHub REST API documentation](https://docs.github.com/en/rest)
- [GitHub GraphQL API documentation](https://docs.github.com/en/graphql)
- [GraphQL Explorer](https://docs.github.com/en/graphql/overview/explorer)
- [Creating a GitHub App](https://docs.github.com/en/apps/creating-github-apps/registering-a-github-app/registering-a-github-app)
- [Authenticating as a GitHub App](https://docs.github.com/en/apps/creating-github-apps/authenticating-with-a-github-app/about-authentication-with-a-github-app)
- [About OAuth Apps](https://docs.github.com/en/apps/oauth-apps/building-oauth-apps/creating-an-oauth-app)
- [Personal access tokens (fine-grained)](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens)
- [Webhooks documentation](https://docs.github.com/en/webhooks)
- [Validating webhook deliveries](https://docs.github.com/en/webhooks/using-webhooks/validating-webhook-deliveries)
- [REST rate limits](https://docs.github.com/en/rest/overview/rate-limits-for-the-rest-api)
- [GraphQL rate limits](https://docs.github.com/en/graphql/overview/resource-limitations)
- [Best practices for using the API](https://docs.github.com/en/rest/guides/best-practices-for-using-the-rest-api)

# Question Bank

Reusable questions by domain. Pull from here when scoping discovery interviews. Pair with the matching guide in `../../discovery-assessments/templates/interview-guides/` and questionnaire in `../../discovery-assessments/templates/questionnaires/`.

> Use these as **starters and probes**, not as a checklist. A real interview adapts to what you've heard.

## Cross-cutting (any domain)

### Outcome / framing
- What does success look like for you in 12 months?
- What's the single most important thing we should not break?
- What's not in scope but probably should be?
- What's the biggest constraint we should plan around?
- Who else needs to be in this conversation?

### Current state
- Walk me through how that works today, end to end.
- Where does the current way of working break down?
- What's working well that you'd be reluctant to change?
- What's the biggest source of toil for the team right now?

### Decision history
- What was the original driver behind the current setup?
- What did you try before this that didn't work?
- What's a decision you'd revisit if you could?

### Reality check
- Where is implementation different from policy?
- What's licensed but not actively used?
- What gets done because someone individually carries it?

---

## Identity & access

### Discovery
- Which IdP is the source of truth, and is it consistent across business units?
- Is SSO enforced at enterprise level today, or just enabled?
- How is SCIM provisioning configured? What's the source of truth for membership?
- How are outside collaborators approved and reviewed?
- What's the policy on PATs (classic vs. fine-grained, expiration, allowed scopes)?

### Probes
- Walk me through how a new hire gets access to a repo today.
- What happens when someone leaves the org — how fast is access revoked?
- How do you handle break-glass / emergency access?
- Where do bots and service accounts come from, and how are they managed?
- Have you had a token-related incident in the last 18 months? What did it teach you?

### Hypothesis tests
- It sounds like SCIM is in place but inconsistently scoped — fair?
- I'm reading: identity is well-controlled, but team-to-repo mapping is manual. Yes?

---

## Org & repo governance

### Discovery
- How are organizations structured, and what drove that topology?
- What are base permissions on each org today?
- Who can create repositories?
- Are rulesets in use, or classic branch protection, or both?
- How is CODEOWNERS used? Required for review?

### Probes
- Pick a real repo — let's look at its protection settings together.
- What's on the bypass list for rulesets, and who reviews it?
- How does a new repo get its initial config? Manual, template, automation?
- Where is config the source of truth — IaC, console, or scripts?
- What drift have you noticed between intended and actual state?

---

## Security & GHAS

### Discovery
- Which frameworks are in scope for the next audit, and when?
- What GHAS coverage do you have today? Licensed vs. active?
- Is push protection enforced, and on which orgs?
- What's the triage SLA by severity, and who owns it?
- Where do GHAS alerts go — security tab only, or also SIEM / ticketing?

### Probes
- How would you detect a leaked PAT today?
- Walk me through what happens when secret scanning fires.
- What's the IR playbook for a malicious GitHub Action?
- What custom secret patterns have you added?
- For each critical control, what evidence does your auditor require?

### Hypothesis tests
- It looks like GHAS is broadly licensed but triage is uneven across teams — fair?
- I'm hearing audit log streaming is in place, but alert rules are minimal. Yes?

---

## CI/CD & GitHub Actions

### Discovery
- What CI tools are in use, and what fraction of pipelines are on each?
- Where is GitHub Actions adopted today? Use cases?
- What's your runner strategy — hosted, larger, self-hosted, ARC?
- How do workflows authenticate to cloud — secrets, OIDC, or both?
- Do you have a reusable workflow / composite action library?

### Probes
- Pick a representative pipeline — walk me through what migrating it would look like.
- What's the worst-performing workflow right now, and why?
- Where are runners the bottleneck, and what's the queue depth at peak?
- What's the allowed Actions policy — verified, allowlist, internal only?
- What's the default `GITHUB_TOKEN` permission — read, write, none?
- How do you handle deployment approvals — environments, manual gates, ChatOps?

---

## Audit, compliance, observability

### Discovery
- What's audit log retention today, and what does the framework require?
- Where is the audit log streamed, and what's it queried for?
- Which events trigger alerts? Who responds?
- What evidence does your auditor require for change-management controls?

### Probes
- Show me the alert rule for "ruleset bypass."
- When was the last audit finding on this platform? What was it?
- How is evidence collected today — automated, scripted, manual?
- What's the gap between what you have and what passes the next audit?

---

## Migration

### Discovery
- What's the source platform, and how many repos / users / pipelines?
- What's the active vs. archive split?
- What's the downtime tolerance per repo? Per org?
- What's the chain-of-custody requirement, if any?
- Have you tried this migration before? What happened?

### Probes
- Pick a representative repo — what makes it easy or hard to migrate?
- What integrations point to source URLs that we'd need to update?
- What's your wave strategy — by team, by criticality, by risk?
- What's the rollback criteria per wave?
- What freeze windows are non-negotiable?

---

## Copilot / AI tooling

### Discovery
- Has Copilot been approved by Legal / Privacy / Security?
- What's the policy on public code matching?
- Are content exclusions needed — for which paths or repos?
- What's the pilot cohort, and what metrics will we measure?
- Are you adopting Copilot Business or Enterprise?

### Probes
- What concerns has the security team raised? Have they been resolved?
- What's the acceptable use policy say about AI-generated code review?
- How will Copilot events be logged and audited?
- What's the support / champion model for the rollout?
- What does "successful Copilot adoption" look like — acceptance, satisfaction, cycle time?

---

## Developer experience

### Discovery
- Walk me through your last PR from open to merge.
- What's the time-to-first-commit for a new hire?
- What's the most-used internal tool besides GitHub?
- Are DORA metrics tracked? Where?
- Is there a developer survey? What did the last one say?

### Probes (best with 2–3 ICs in the room)
- What's the worst part of how things work today?
- What have you worked around rather than raised?
- What tools do you use *besides* GitHub for a typical PR?
- If we could fix one thing in 90 days, what should it be?
- What's something that's working well that you'd be reluctant to change?

---

## Cost & licensing

### Discovery
- How is GitHub spend allocated across business units?
- What's the seat trajectory for Enterprise / GHAS / Copilot?
- What's the Actions-minute trajectory? Where is most consumed?
- Is there a chargeback / showback model?

### Probes
- What's the variance-to-forecast on Actions minutes this year?
- At what threshold of Copilot seats does pricing tier change for you?
- Where is GHAS licensed but not used?
- What's the true-up cycle for GHAS seats — committer-based?

---

## Operations & support

### Discovery
- Who owns the GitHub platform — RACI?
- What's the change-management process for org-level settings?
- What's the on-call rotation? SLA?
- What runbooks exist for common ops tasks?

### Probes
- What breaks at 3am, and what's the runbook?
- What's the bus factor on platform decisions?
- What support tickets are the most common right now?
- If <key person> were out for two weeks, what would not get done?

---

## Closing questions (always ask one)

These are the questions that produce the meeting's most valuable insight 30% of the time. End with one:

- What haven't I asked about that I should have?
- If you were running this engagement, what would you want to know that we haven't covered?
- Who else should we be talking to before drawing conclusions?
- What's the question you've been waiting for me to ask?
- If everything goes perfectly, what's still going to be hard?

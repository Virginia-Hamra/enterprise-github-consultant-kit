# Audience Tuning

The same topic requires very different questions depending on who's in the room. Tuning to audience is what separates senior consultants from junior ones.

## The general rule

Each audience has:

- A **vocabulary** they use (and one they don't).
- A **time horizon** they care about.
- A **success metric** they're measured on.
- A **failure mode** they fear.
- A set of **politically loaded** topics for them.

Your questions land when they speak to these. They miss when you ask everyone the same way.

## Executive sponsors / VPs / Directors

**Vocabulary:** Outcomes, risk, cost, timeline, dependency, precedent, board narrative.
**Time horizon:** Quarters and fiscal years.
**Measured on:** Business outcomes, headcount, budget adherence, audit results.
**Fear:** Surprise, public failure, missed commitment to leadership above them.
**Loaded topics:** Headcount, budget overruns, peers' programs, vendor lock-in.

### Question style

- **Short.** 30 minutes max, often 20.
- **Outcome-shaped, not technical.** *"What does success look like in 12 months?"* not *"What rulesets do you envision?"*
- **One layer of "why" only.** Don't ladder them.
- **Always offer an out.** *"If you'd rather defer, who should I speak to?"*

### High-value executive questions

- *"What outcome are you accountable for that this engagement supports?"*
- *"What's the single most important thing we should not break?"*
- *"What does failure look like? What would you regret in 12 months?"*
- *"Who else needs to see results, and in what form?"*
- *"What's the biggest risk you're carrying right now that this engagement should reduce?"*
- *"Where is your air cover going to be tested?"*
- *"What part of this is non-negotiable, and what's negotiable?"*
- *"Are there organizational changes coming that we should plan around?"*
- *"How do you want to be kept informed — cadence, format?"*

### Avoid

- Tool names, version numbers, or feature deep-dives.
- "How does X work today?" — they don't know operationally; you'll embarrass them.
- More than one question per turn.

## Platform / DevOps / Infrastructure leads

**Vocabulary:** Pipelines, runners, IaC, drift, ownership, runbooks, on-call, toil.
**Time horizon:** Sprint to quarter.
**Measured on:** Uptime, throughput, cost, support tickets.
**Fear:** Breaking change rolling out unannounced; tool sprawl; unowned config.
**Loaded topics:** Why a previous decision was made; tools chosen by peer teams; budget cuts.

### Question style

- Concrete. Use real names of systems, repos, scripts.
- Numbers help. *"Roughly how many?"* always works.
- Allow time. They want to give complete answers.

### High-value platform questions

- *"Walk me through how a new repo gets created and configured today."*
- *"Where is config the source of truth — IaC, console, scripts, tribal?"*
- *"What's your top toil item this quarter?"*
- *"What breaks at 3am, and what's the runbook?"*
- *"Which automation is fragile — what would you not want to touch right now?"*
- *"Where is drift happening between intended and actual state?"*
- *"What's the change-management process for org-level settings?"*
- *"If I gave you one quarter of dedicated platform investment, what would you do?"*

## Security / AppSec / GRC

**Vocabulary:** Controls, frameworks (SOC 2, ISO, NIST), evidence, severity, SLA, attestation, incident.
**Time horizon:** Audit cycle, IR cycle, regulatory deadlines.
**Measured on:** Audit findings, mean-time-to-remediate, incident count, framework coverage.
**Fear:** Audit failure, undetected breach, unowned alerts, "we have it but don't use it."
**Loaded topics:** Visibility into developer behavior, alerts triaged by no one, prior incidents.

### Question style

- Frame in **control + evidence** language. Security people respect that vocabulary.
- Be precise about what is *implemented* vs. *enforced* vs. *evidenced*.
- Acknowledge the implementation/enforcement gap explicitly — it's their daily reality.

### High-value security questions

- *"Which frameworks are in scope for the next audit, and when?"*
- *"What's the gap between implemented and enforced for branch protection today?"*
- *"For each critical control, who triages and what's the SLA?"*
- *"When was the last incident on this platform? What did the post-mortem say?"*
- *"What's the audit log retention requirement, and is it met?"*
- *"How would you detect a leaked PAT today?"*
- *"What's the bypass list on rulesets, and who reviews it?"*
- *"What's the IR playbook for a malicious GitHub Action?"*
- *"What evidence artifact does your auditor require for control X?"*

## Individual contributors / engineers

**Vocabulary:** PRs, branches, builds, tests, local env, IDE, friction, "it just works."
**Time horizon:** Today, this PR, this sprint.
**Measured on:** Shipping work, often informally.
**Fear:** Looking incompetent, getting blamed, being slowed down.
**Loaded topics:** Manager / team disagreements; tools imposed from above.

### Question style

- Make it **safe to be honest.** Frame: *"We're not auditing you — we want your real experience."*
- Avoid leading questions toward what you think they should say.
- Mix in 2–3 ICs with 1–2 leads in the same room — single ICs often defer to the lead.

### High-value IC questions

- *"Walk me through your last PR from open to merge."*
- *"Where do you wait the most? Where do you context-switch?"*
- *"What tools did you use besides GitHub for this PR?"*
- *"What have you worked around rather than raised?"*
- *"What's the worst part of onboarding a new repo?"*
- *"If we could fix one thing in 90 days, what should it be?"*
- *"What do you like about how things work today? What would you not want changed?"* (asks the **strengths** question — most consultants forget this)

### Avoid

- Asking about strategy or roadmap (not their seat).
- Pointing out broken processes their lead designed.
- Putting them on the spot in front of leadership.

## Operations / SRE / Production

**Vocabulary:** SLO, error budget, on-call, paging, runbook, rollback, blast radius.
**Time horizon:** Incident-to-incident.
**Measured on:** Uptime, page rate, MTTR.
**Fear:** Change without rollback; unowned alerts; deployment-induced incidents.
**Loaded topics:** Developer team's deployment habits; freeze windows.

### High-value ops questions

- *"What's your deployment freeze policy, and what triggers it?"*
- *"Show me your last incident's post-mortem."*
- *"Which integrations would page you if they broke?"*
- *"What's the rollback procedure for a bad deploy?"*
- *"How is on-call rotated and compensated?"*

## Legal / Privacy / IP

**Vocabulary:** DPA, DPIA, processor / sub-processor, data residency, jurisdiction, IP indemnity, license.
**Time horizon:** Contract term.
**Measured on:** Contract risk avoided.
**Fear:** Unreviewed vendor agreement; unflagged data flow; IP exposure.
**Loaded topics:** AI training, telemetry, public-cloud data residency.

### Question style

- Be precise. Vague questions get cautious "no" answers.
- Bring artifacts (DPA, sub-processor list) so they can review concretely.

### High-value legal questions

- *"What's the approved jurisdiction for processing source code?"*
- *"What's the position on AI tools that may use code as training input? (For Copilot Business/Enterprise: data is not used to train.)"*
- *"What's the IP indemnity requirement for third-party Actions / Apps?"*
- *"What's the contract review threshold for new tooling adoption?"*
- *"What evidence is required from us to satisfy your audit team?"*

## Finance / FinOps

**Vocabulary:** Run-rate, unit economics, license SKU, true-up, charge-back, allocation.
**Time horizon:** Monthly, quarterly, fiscal year.
**Measured on:** Variance to forecast, unit cost.
**Fear:** Surprise overruns, unallocated spend.

### High-value FinOps questions

- *"How is GitHub spend allocated across business units today?"*
- *"What's your forecast for Actions minutes this quarter?"*
- *"At what threshold of Copilot seats does pricing tier change for you?"*
- *"What's the true-up cycle for GHAS seats — committer-based?"*

## Adapting on the fly

When you don't know who's in the room (mixed audience), default to:

1. **Start broad and outcome-shaped.** Anyone can answer "What does success look like?"
2. **Watch who answers what.** That tells you who owns what.
3. **Direct follow-ups by role.** *"Jane, from the security seat — how does that land?"*
4. **End with the "what's missing" question** — gives quiet attendees a chance.

## Cross-audience: questions every audience can answer

When you don't know what to ask, these never miss:

- *"What does success look like for you in this engagement?"*
- *"What's the most painful part of the current way of working?"*
- *"What would you not want changed?"*
- *"Who else should we be talking to?"*
- *"What haven't I asked that I should have?"*

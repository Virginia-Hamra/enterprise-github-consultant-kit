# Copilot Demo Script

> **Audience:** Eng leadership / IC engineers &nbsp; | &nbsp; **Run time:** 30–45 min

## Narrative arc
"Move from generic suggestions to organizationally-aware AI that respects your code, policies, and context."

## Setup
- VS Code with Copilot Business/Enterprise signed in
- Sample repo with `.github/copilot-instructions.md` and a `.github/skills/` folder
- Copilot Chat, code review, and (optionally) coding agent enabled
- Knowledge base / Space pre-populated with internal docs (if Enterprise)

## Flow

### 1. Inline completion + context (5 min)
- Show ghost-text completion in 2 languages
- Toggle model picker; explain when to switch
- Mention content exclusions

### 2. Chat (8 min)
- `/explain` on a complex function
- `@workspace` Q&A
- Multi-file edit with chat
- Reference custom instructions / skills shaping behavior

### 3. Prompt files & custom agents (5 min)
- Run a prompt file
- Mention agent-mode customization

### 4. Copilot in PRs (5 min)
- Show Copilot code review on an open PR
- Discuss governance + acceptable use

### 5. Coding agent (8 min, if enabled)
- Assign an issue to Copilot
- Walk through the agent's session log
- Show governance: scope, branch protection, required review

### 6. Knowledge bases / Spaces (Enterprise) (5 min)
- Ask a question grounded in internal docs
- Discuss curation & maintenance

## Key talking points
- `copilot-instructions.md` is your "house style" lever
- Skills package domain knowledge for repeatable workflows
- Content exclusions + acceptable use policy = governance posture
- Metrics: acceptance rate, satisfaction, cycle time

## FAQ pocket
- "Does my code train the model?" → no (Business / Enterprise)
- "Public code matching?" → policy lever, default block
- "Audit?" → audit log captures Copilot events

## Fallback
- Pre-recorded clips per scenario in case of network issues

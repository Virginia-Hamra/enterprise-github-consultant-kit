# Agent and Model Switching

## Overview
Switch between agents and models based on task requirements to optimize performance and cost.

## Switching Models in Chat

### Using UI
1. Open Copilot Chat panel
2. Click model selector (top of chat)
3. Choose from available models:
   - Claude Sonnet 4.5 (default)
   - Claude Opus 4.5/4.6 (premium)
   - GPT-5.2, GPT-5.1 variants
   - Claude Haiku 4.5 (fast)

### Using Commands
```
/model claude-opus-4.5
/model gpt-5.1-codex
/model claude-haiku-4.5
```

## Switching Custom Agents

### In Chat
```
@security-reviewer analyze this code
@test-generator create unit tests
@docs-writer document this function
```

### Via Slash Command
```
/agent security-reviewer
/agent test-generator
```

### Reset to Default
```
/agent default
```

## CLI Model Switching

### Single Request
```bash
copilot --model claude-opus-4.5 -p "Complex architectural analysis"
copilot --model gpt-5.1-codex -p "Generate tests"
copilot --model claude-haiku-4.5 -p "Fix typo"
```

### Interactive Session
```bash
copilot --model gpt-5.1-codex
# Model persists for entire session
```

### Agent with Model Override
```bash
copilot --agent security-reviewer --model claude-opus-4.6
```

## Model Comparison

| Model | Speed | Quality | Cost | Best For |
|-------|-------|---------|------|----------|
| Claude Opus 4.5/4.6 | Slow | Highest | Premium | Critical reviews, complex architecture |
| Claude Sonnet 4.5 | Medium | High | Standard | General development, most tasks |
| GPT-5.2 Codex | Medium | High | Standard | Code generation, refactoring |
| GPT-5.1 Codex | Fast | Good | Standard | Quick code edits, completions |
| Claude Haiku 4.5 | Fastest | Good | Standard | Simple tasks, quick questions |

## Strategy: Model Cascading

Start with faster models, escalate for complexity:

```
1. Claude Haiku 4.5 → Simple fixes, typos
2. Claude Sonnet 4.5 → Standard development
3. GPT-5.1 Codex → Code generation
4. Claude Opus 4.5 → Architecture, critical reviews
```

## Practical Examples

### Task-Based Selection

**Simple Question**
```
/model claude-haiku-4.5
What does this function do?
```

**Code Generation**
```
/model gpt-5.1-codex
Generate CRUD API for User model
```

**Security Review**
```
/model claude-opus-4.5
Review this authentication system for vulnerabilities
```

**Documentation**
```
/model claude-sonnet-4.5
Write API documentation for these endpoints
```

## Agent Switching Workflows

### Sequential Agent Use
```
1. @architect-agent: Design system architecture
2. @coder-agent: Implement the design
3. @test-agent: Generate test suite
4. @docs-agent: Document the system
```

### Specialized Reviews
```
1. @security-reviewer: Security analysis
2. @performance-reviewer: Performance optimization
3. @accessibility-reviewer: A11y compliance
```

## Best Practices

### When to Switch
- **Task complexity changes**: Simple → Complex
- **Different expertise needed**: Code → Security → Docs
- **Premium request conservation**: Use standard models first
- **Speed requirements**: Fast iterations vs. deep analysis

### When to Stay
- **Consistent context**: Keep same agent for related tasks
- **Mid-conversation**: Switching loses context
- **Working well**: Don't fix what isn't broken

### Context Preservation
- Model switches preserve conversation history
- Agent switches may reset some context
- Use `@workspace` to maintain workspace context
- Reference previous messages: "based on your last response..."

## Automation

### Project Scripts
```bash
#!/bin/bash
# review.sh - Multi-agent code review

copilot --agent security-reviewer -p "Review $1" > security.md
copilot --agent performance-reviewer -p "Review $1" > perf.md
copilot --agent accessibility-reviewer -p "Review $1" > a11y.md
```

### VS Code Tasks
```json
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "Security Review",
      "type": "shell",
      "command": "copilot --agent security-reviewer -p 'Review current file'"
    }
  ]
}
```

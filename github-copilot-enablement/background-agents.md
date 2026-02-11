# Background Agents

## Overview
Background agents run autonomously in separate context windows to handle specific tasks without disrupting your main workflow.

## Types of Background Agents

### 1. Task Agent
Executes commands with verbose output handling.

**Use Cases:**
- Running test suites
- Building projects
- Installing dependencies
- Linting and formatting
- CI/CD operations

**Characteristics:**
- Returns brief summary on success
- Shows full output on failure
- Keeps main context clean
- Uses Claude Haiku (fast, efficient)

### 2. Explore Agent
Fast codebase exploration and analysis.

**Use Cases:**
- Finding files by patterns
- Searching for code snippets
- Understanding code structure
- Answering questions about codebase
- Quick documentation lookup

**Characteristics:**
- Focused answers under 300 words
- Uses grep/glob/view tools
- Safe to call in parallel
- Uses Claude Haiku (fast)

### 3. General-Purpose Agent
Full-capability agent for complex tasks.

**Use Cases:**
- Multi-step implementations
- Complex refactoring
- Architecture changes
- Research and analysis
- Advanced problem-solving

**Characteristics:**
- Complete toolset access
- Runs in separate context window
- Uses Claude Sonnet (high quality)
- Maintains own conversation state

### 4. Code Review Agent
Specialized for reviewing code changes.

**Use Cases:**
- Pre-commit reviews
- PR analysis
- Security audits
- Performance reviews
- Best practice validation

**Characteristics:**
- High signal-to-noise ratio
- Only flags important issues
- Never comments on style
- Won't modify code
- Focuses on bugs, security, logic errors

## Using Background Agents in CLI

### Task Agent
```bash
# Run tests in background
copilot --agent task -p "Run the test suite"

# Build project
copilot --agent task -p "Build the project"

# Install dependencies
copilot --agent task -p "Install npm dependencies"
```

### Explore Agent
```bash
# Find authentication files
copilot --agent explore -p "Find all authentication-related files"

# Search for API endpoints
copilot --agent explore -p "What API endpoints exist in this project?"

# Understand architecture
copilot --agent explore -p "How is the database layer structured?"
```

### General-Purpose Agent
```bash
# Complex refactoring
copilot --agent general-purpose -p "Refactor auth system to use dependency injection"

# Multi-step feature
copilot --agent general-purpose -p "Add rate limiting to all API endpoints"
```

### Code Review Agent
```bash
# Review staged changes
copilot --agent code-review -p "Review my staged changes"

# Review specific branch
copilot --agent code-review -p "Review changes in feature/auth-v2"
```

## Parallel Agent Execution

### Running Multiple Agents
Background agents can run simultaneously for different tasks:

```bash
# Terminal 1: Run tests
copilot --agent task -p "Run unit tests"

# Terminal 2: Explore codebase
copilot --agent explore -p "Find all database queries"

# Terminal 3: Review code
copilot --agent code-review -p "Review PR #123"

# Terminal 4: Main development
copilot
# Continue your main conversation
```

### Safe Parallel Operations
```bash
# Multiple explore agents (read-only, safe)
copilot --agent explore -p "Find React components" &
copilot --agent explore -p "Find test files" &
copilot --agent explore -p "Find configuration files" &
wait

# Task and explore together
copilot --agent task -p "Run linter" &
copilot --agent explore -p "Find linting rules"
```

### Avoid Parallel Side Effects
```bash
❌ Don't run in parallel:
copilot --agent general-purpose -p "Refactor auth.js" &
copilot --agent general-purpose -p "Refactor user.js" &
# May cause file conflicts

✓ Run sequentially:
copilot --agent general-purpose -p "Refactor auth.js"
copilot --agent general-purpose -p "Refactor user.js"
```

## Workflow Patterns

### Development Pipeline
```bash
#!/bin/bash
# dev-pipeline.sh

echo "🔍 Exploring codebase..."
copilot --agent explore -p "Analyze project structure" > analysis.txt

echo "🏗️  Building..."
copilot --agent task -p "Build project"

echo "🧪 Testing..."
copilot --agent task -p "Run all tests"

echo "🔍 Reviewing..."
copilot --agent code-review -p "Review recent changes"

echo "✅ Pipeline complete"
```

### Pre-Commit Workflow
```bash
#!/bin/bash
# pre-commit.sh

# Run in parallel (safe, read-only)
copilot --agent code-review -p "Review staged changes" > review.md &
copilot --agent task -p "Run linter on staged files" &
copilot --agent task -p "Run tests affected by changes" &
wait

echo "✅ Pre-commit checks complete"
cat review.md
```

### Research and Implement
```bash
# Step 1: Research in background
copilot --agent explore -p "How is authentication currently implemented?" > auth-analysis.txt

# Step 2: Main agent implements based on research
copilot -p "Add 2FA based on analysis in auth-analysis.txt"
```

## Agent Communication

### Sharing Context Between Agents
Agents are stateless and independent - use files to share context:

```bash
# Agent 1: Analyze
copilot --agent explore -p "Analyze architecture" > architecture.md

# Agent 2: Implement based on Agent 1 output
copilot --agent general-purpose -p "Implement feature X based on #file:architecture.md"

# Agent 3: Test implementation
copilot --agent task -p "Test the feature X implementation"

# Agent 4: Review
copilot --agent code-review -p "Review feature X"
```

## VS Code Integration

### Task Tool
In main conversation, delegate to background agents:

```
You: "I need to understand the database schema and run tests"

Copilot uses task tool internally:
- Spawns explore agent: "Analyze database schema"
- Spawns task agent: "Run test suite"
- Returns consolidated results to main conversation
```

### When Copilot Uses Background Agents
Copilot automatically delegates to background agents for:
- Codebase exploration questions
- Test execution requests
- Build commands
- Complex multi-step tasks

## Best Practices

### When to Use Background Agents

**Use Task Agent When:**
- Running long-running commands
- Need clean success/failure status
- Don't want output cluttering main chat
- Executing CLI operations

**Use Explore Agent When:**
- Quick codebase questions
- Finding files or patterns
- Understanding code structure
- Need fast responses

**Use General-Purpose When:**
- Complex multi-step tasks
- Need full reasoning capability
- Task requires multiple tool operations
- Implementing substantial features

**Use Code Review When:**
- Pre-commit validation
- PR reviews
- Security audits
- Only need issue identification (not fixes)

### When to Use Main Agent

**Stay in Main Conversation For:**
- Interactive development (back-and-forth)
- Incremental changes with feedback
- Learning and explanations
- Building context over multiple turns

## Monitoring Background Agents

### CLI Output
```bash
# Verbose mode shows agent activity
copilot --log-level debug

# Output shows:
# - Agent spawned
# - Agent task
# - Agent completion
# - Result summary
```

### Status Tracking
```bash
# Check running agents
ps aux | grep copilot

# Agent log files
ls ~/.copilot/logs/agents/
```

## Advanced: Custom Background Agents

### Define in AGENTS.md
```markdown
## @background-tester
Role: Continuous test runner
Model: claude-haiku-4.5
Instructions:
- Run tests on file changes
- Report only failures
- Suggest fixes for failed tests
Tools: bash, view
Background: true

## @background-linter
Role: Continuous code quality monitor
Model: claude-haiku-4.5
Instructions:
- Run linter on changes
- Report violations
- Suggest auto-fixes
Tools: bash, view, edit
Background: true
```

### Invoke Custom Background Agents
```bash
copilot --agent background-tester -p "Monitor test suite"
```

## Limitations

### What Background Agents Cannot Do
- Access main conversation context automatically
- Modify main conversation state
- Interrupt main conversation
- Share memory between instances

### Workarounds
- Use files to share context
- Pass results back via file references
- Coordinate via scripts
- Structure tasks to be independent

## Performance Considerations

### Resource Usage
```
Task Agent: Low CPU, I/O bound
Explore Agent: Low CPU, fast searches
General-Purpose: Higher CPU, complex reasoning
Code Review: Medium CPU, analysis focused
```

### Optimization
- Use explore instead of general-purpose for simple queries
- Use task agent for command execution
- Batch similar operations
- Limit parallel general-purpose agents (resource intensive)

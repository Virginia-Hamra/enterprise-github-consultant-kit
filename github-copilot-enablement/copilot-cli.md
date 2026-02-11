# GitHub Copilot CLI

## Overview
Command-line interface for GitHub Copilot, enabling AI-assisted development in terminal environments.

**Current Version**: 0.0.406 (as of reference)
**Check version**: `copilot version` or `copilot --version`

## Installation

### Prerequisites
- GitHub account with Copilot subscription
- Node.js 16+ (for npm installation)

### Install via npm
```bash
npm install -g @github/copilot-cli
```

### Authentication
```bash
# Login via OAuth device flow
copilot login

# Follow the prompts to authenticate
```

## Usage Modes

### 1. Interactive Mode (Default)
```bash
copilot

# Starts interactive session
# Type questions, get responses
# Use /commands for actions
# Conversational back-and-forth
```

### 2. Single Prompt Mode
```bash
# Execute one prompt and exit
copilot -p "What files changed recently?" --allow-all-tools

# Non-interactive, exits after completion
# Useful for scripting
```

### 3. Auto-Execute Mode
```bash
# Start interactive and run prompt automatically
copilot -i "Review security in auth.js"

# Enters interactive mode with prompt pre-executed
# Continue conversation after
```

## Core Commands

### Session Management
```bash
# Continue last session
copilot --continue

# Resume specific session
copilot --resume

# Resume with session picker
copilot --resume [sessionId]
```

### Help and Information
```bash
# General help
copilot --help
copilot help

# Specific help topics
copilot help config
copilot help commands
copilot help permissions
copilot help environment
copilot help logging

# Version information
copilot version
```

### Utility Commands
```bash
# Update CLI
copilot update

# Initialize repository instructions
copilot init

# Plugin management
copilot plugin
```

## Permission System

### Permission Levels

**1. Allow All (--allow-all or --yolo)**
```bash
copilot --allow-all
copilot --yolo  # alias
# Enables all tools, paths, and URLs
# Required for non-interactive mode
```

**2. Specific Tools**
```bash
# Allow specific tools
copilot --allow-tool view --allow-tool edit

# Allow tool patterns
copilot --allow-tool 'shell(git:*)' --deny-tool 'shell(git push)'

# Allow all file editing
copilot --allow-tool write
```

**3. Path Restrictions**
```bash
# Allow specific directories
copilot --add-dir ~/workspace
copilot --add-dir /tmp

# Allow all paths
copilot --allow-all-paths

# By default: restricted to cwd
```

**4. URL Access**
```bash
# Allow specific URLs
copilot --allow-url github.com
copilot --allow-url https://api.example.com

# Deny specific URLs
copilot --deny-url malicious-site.com

# Allow all URLs
copilot --allow-all-urls
```

## Model Selection

### Set Model
```bash
# Start with specific model
copilot --model claude-opus-4.5
copilot --model gpt-5.1-codex
copilot --model claude-haiku-4.5
```

### Available Models
```
Standard (0x premium):
- claude-sonnet-4.5
- claude-sonnet-4
- claude-haiku-4.5
- gpt-5.2-codex, gpt-5.2
- gpt-5.1-codex-max, gpt-5.1-codex, gpt-5.1
- gpt-5, gpt-5.1-codex-mini, gpt-5-mini
- gpt-4.1

Premium (1x):
- claude-opus-4.6, claude-opus-4.6-fast
- claude-opus-4.5
- gemini-3-pro-preview
```

## Custom Agents

### Using Custom Agents
```bash
# Specify agent from AGENTS.md
copilot --agent security-reviewer

# Agent with model override
copilot --agent test-generator --model gpt-5.1-codex

# Agent in single prompt mode
copilot --agent docs-writer -p "Document API endpoints" --allow-all
```

## Configuration

### Config Directory
```bash
# Default: ~/.copilot/
# Custom location:
copilot --config-dir ~/my-copilot-config
```

### Configuration Files
```
~/.copilot/
├── config.json                # CLI settings
├── mcp-config.json           # MCP servers
├── logs/                     # Log files
├── sessions/                 # Session history
└── session-state/            # Active session data
```

### Config File Example
```json
{
  "model": "claude-sonnet-4.5",
  "allowedTools": ["view", "edit", "grep"],
  "allowedDirs": ["/Users/user/projects"],
  "stream": true,
  "autoUpdate": true
}
```

## Logging

### Log Levels
```bash
copilot --log-level none      # No logging
copilot --log-level error     # Errors only
copilot --log-level warning   # Warnings and errors
copilot --log-level info      # Info, warnings, errors
copilot --log-level debug     # All output
copilot --log-level all       # Everything (verbose)
```

### Log Directory
```bash
# Default: ~/.copilot/logs/
# Custom:
copilot --log-dir ~/my-logs/

# View logs
cat ~/.copilot/logs/latest.log
tail -f ~/.copilot/logs/latest.log
```

## Advanced Features

### MCP Server Management
```bash
# Disable built-in MCPs
copilot --disable-builtin-mcps

# Disable specific MCP
copilot --disable-mcp-server github-mcp-server

# Add temporary MCP config
copilot --additional-mcp-config @./mcp-config.json

# Enable all GitHub MCP tools
copilot --enable-all-github-mcp-tools
```

### Tool Management
```bash
# Limit available tools
copilot --available-tools view,edit,grep

# Exclude specific tools
copilot --excluded-tools bash,web_fetch

# Allow specific MCP tools
copilot --add-github-mcp-tool get_workflow_run
copilot --add-github-mcp-toolset actions
```

### Parallel Execution
```bash
# Disable parallel tool execution
copilot --disable-parallel-tools-execution

# Default: enabled (tools run in parallel when possible)
```

### Output Control
```bash
# Silent mode (response only, no stats)
copilot -p "question" --silent

# No color output
copilot --no-color

# Plain diff (no syntax highlighting)
copilot --plain-diff

# Screen reader optimizations
copilot --screen-reader
```

### Session Sharing
```bash
# Share to markdown file after completion
copilot -p "task" --share ./session-output.md

# Share to GitHub gist
copilot -p "task" --share-gist
```

### Streaming
```bash
# Enable/disable streaming
copilot --stream on
copilot --stream off
```

### Ask User Control
```bash
# Disable ask_user tool (autonomous mode)
copilot --no-ask-user

# Agent won't ask clarifying questions
# Makes all decisions independently
```

### Custom Instructions
```bash
# Disable loading custom instructions
copilot --no-custom-instructions

# Ignores .github/copilot-instructions.md
```

## Interactive Commands

### In-Session Commands
```
/help          - Show available commands
/clear         - Clear conversation
/new           - Start new conversation
/model [name]  - Switch model
/agent [name]  - Switch agent
/explain       - Explain code
/fix           - Suggest fixes
/tests         - Generate tests
/doc           - Write documentation
```

### Context References
```
@workspace                  - Search entire workspace
@vscode                    - VS Code API help
#file:path/to/file.js      - Specific file
#selection                 - Current selection
#terminalLastCommand       - Last terminal command
#terminalSelection         - Selected terminal text
```

## Scripting and Automation

### Shell Scripts
```bash
#!/bin/bash
# auto-review.sh

# Run multiple reviews
copilot --allow-all -p "Security review of auth.js" > security.txt
copilot --allow-all -p "Performance review of api.js" > perf.txt
copilot --allow-all -p "Test coverage analysis" > tests.txt

# Combine results
cat security.txt perf.txt tests.txt > full-review.md
```

### CI/CD Integration
```yaml
# .github/workflows/copilot-review.yml
name: Copilot Code Review

on: [pull_request]

jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Install Copilot CLI
        run: npm install -g @github/copilot-cli
      - name: Authenticate
        run: echo "${{ secrets.COPILOT_TOKEN }}" | copilot login
      - name: Review changes
        run: copilot --allow-all -p "Review PR changes" > review.md
      - name: Comment on PR
        uses: actions/github-script@v6
        with:
          script: |
            const fs = require('fs');
            const review = fs.readFileSync('review.md', 'utf8');
            github.rest.issues.createComment({
              issue_number: context.issue.number,
              owner: context.repo.owner,
              repo: context.repo.repo,
              body: review
            });
```

### Git Hooks
```bash
# .git/hooks/pre-commit

#!/bin/bash
echo "Running Copilot pre-commit review..."

# Get staged files
STAGED=$(git diff --cached --name-only)

# Review with Copilot
copilot --allow-all --silent -p "Quick review of staged changes: $STAGED"

# Exit code determines if commit proceeds
exit 0
```

## Troubleshooting

### Common Issues

**Authentication Failed**
```bash
# Re-authenticate
copilot login

# Check token
cat ~/.copilot/config.json | grep token
```

**Permission Denied**
```bash
# Add required directories
copilot --add-dir /path/to/project

# Or use allow-all for testing
copilot --allow-all
```

**Model Unavailable**
```bash
# Check available models
copilot --help | grep model

# Try different model
copilot --model claude-sonnet-4.5
```

**Slow Performance**
```bash
# Use faster model
copilot --model claude-haiku-4.5

# Reduce context
# (reference fewer files)
```

### Debug Mode
```bash
# Enable debug logging
copilot --log-level debug

# Check logs
tail -f ~/.copilot/logs/latest.log
```

## Best Practices

### Development Workflow
```bash
# Start interactive session with permissions
copilot --allow-all-tools --allow-all-paths

# Use for:
# - Code reviews
# - Bug investigation
# - Feature implementation
# - Documentation
# - Testing
```

### Scripting Workflow
```bash
# Single-purpose scripts with silent mode
copilot -p "Generate test for user.js" --silent --allow-all > test.js

# Chain commands
copilot -p "Analyze" --silent --allow-all | copilot -p "Implement" --silent --allow-all
```

### Team Workflow
```bash
# Shared configuration
# Commit .github/copilot-instructions.md
# Commit AGENTS.md
# Share best practices in wiki

# Individual customization
# Local ~/.copilot/config.json
# Personal agents in AGENTS.md
```

## Updates

### Check for Updates
```bash
copilot version
# Shows current version and checks for updates
```

### Update CLI
```bash
# Automatic update
copilot update

# Or via npm
npm update -g @github/copilot-cli

# Disable auto-update
copilot --no-auto-update
```

## Resources

### Official Documentation
- GitHub Copilot Docs: https://docs.github.com/copilot
- CLI Repository: https://github.com/github/copilot-cli
- MCP Protocol: https://modelcontextprotocol.io

### Community
- GitHub Discussions
- Stack Overflow (tag: github-copilot)
- GitHub Community Forum

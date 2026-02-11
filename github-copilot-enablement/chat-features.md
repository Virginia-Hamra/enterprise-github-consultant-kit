# Chat Features and Settings

## Chat Instructions

### Overview
Instructions that apply specifically to Copilot Chat interactions, separate from general coding instructions.

### Location
`.github/copilot-chat-instructions.md` or include in `.github/copilot-instructions.md`

### Purpose
- Define chat response style and format
- Set interaction preferences
- Specify output requirements
- Control verbosity and detail level

### Example Chat Instructions
```markdown
# Chat Instructions

## Response Style
- Be concise and direct
- Use bullet points for lists
- Include code examples when relevant
- Cite file references when making claims

## Code Examples
- Always include working, runnable code
- Add comments for complex logic
- Follow project style guide
- Include error handling

## Explanations
- Start with high-level overview
- Provide step-by-step breakdown
- Explain "why" not just "what"
- Link to relevant documentation

## Restrictions
- Don't suggest deprecated APIs
- Only recommend production-ready solutions
- Prioritize security over convenience
- Always validate user input in examples
```

## Chat Diagnostics

### Accessing Diagnostics
**VS Code:**
1. View → Output
2. Select "GitHub Copilot" from dropdown
3. Or Command Palette: "Copilot: Show Diagnostics"

**CLI:**
```bash
# View logs
cat ~/.copilot/logs/latest.log

# Follow logs in real-time
tail -f ~/.copilot/logs/latest.log
```

### Diagnostic Information

#### Request Details
```
Request ID: req_abc123
Model: claude-sonnet-4.5
Timestamp: 2026-02-11T13:52:00Z
Token count: 15234
Context files: 3
Response time: 2.3s
```

#### Context Analysis
```
Files referenced: 
- src/auth/jwt.ts (1234 tokens)
- src/models/user.ts (890 tokens)
- .github/copilot-instructions.md (456 tokens)

Total context: 15234 tokens
Available: 184766 tokens remaining
```

#### Errors and Warnings
```
Warning: Large file referenced (>10K lines)
Warning: Context window 75% full
Error: Rate limit approaching
Error: Model unavailable, fallback used
```

### Diagnostic Use Cases

**Performance Issues:**
```
Check: Response time > 10s
Investigate: Context size, model selection
Solution: Reduce referenced files, use faster model
```

**Quality Issues:**
```
Check: Token count in diagnostics
Investigate: Context completeness
Solution: Add missing context, clarify prompt
```

**Context Problems:**
```
Check: Files actually included in request
Investigate: Path resolution, file access
Solution: Use absolute paths, verify file exists
```

## Chat Settings

### VS Code Settings

#### Location
- Settings → Extensions → GitHub Copilot
- Or edit `.vscode/settings.json`

#### Key Settings
```json
{
  // Enable/disable Copilot
  "github.copilot.enable": true,
  
  // Chat settings
  "github.copilot.chat.welcomeMessage": "firstUse",
  "github.copilot.chat.responseLength": "concise",
  "github.copilot.chat.codeExamples": true,
  
  // Model selection
  "github.copilot.chat.model": "claude-sonnet-4.5",
  
  // Context
  "github.copilot.chat.maxContextTokens": 100000,
  "github.copilot.chat.includeOpenFiles": true,
  
  // Features
  "github.copilot.chat.experimental": false,
  "github.copilot.chat.streaming": true
}
```

#### Response Length Options
```json
{
  "github.copilot.chat.responseLength": "concise",  // Short answers
  "github.copilot.chat.responseLength": "normal",   // Balanced
  "github.copilot.chat.responseLength": "verbose"   // Detailed
}
```

#### Language-Specific Settings
```json
{
  "github.copilot.enable": {
    "*": true,
    "yaml": false,
    "plaintext": false,
    "markdown": true
  }
}
```

### CLI Settings

#### Configuration File
Location: `~/.copilot/config.json`

```json
{
  "model": "claude-sonnet-4.5",
  "stream": true,
  "allowedTools": ["view", "edit", "bash"],
  "allowAllPaths": false,
  "autoUpdate": true
}
```

#### Command-Line Options
```bash
# Set default model
copilot --model claude-opus-4.5

# Streaming preference
copilot --stream on
copilot --stream off

# Silent mode (scripting)
copilot -p "question" --silent

# Log level
copilot --log-level debug
copilot --log-level error
```

### User Preferences

#### Custom Instructions Directory
```bash
# Set custom config directory
copilot --config-dir ~/my-copilot-config
```

#### Session Persistence
```bash
# Continue last session
copilot --continue

# Resume specific session
copilot --resume abc123

# List sessions
ls ~/.copilot/sessions/
```

## Show Chat Debug View

### VS Code Debug View

#### Enabling
```json
{
  "github.copilot.advanced": {
    "debug.showDebugPanel": true,
    "debug.showRequestDetails": true
  }
}
```

#### Accessing
1. View → Output → "GitHub Copilot Debug"
2. Or Command Palette: "Copilot: Show Debug View"

#### Debug Information

**Request Details:**
```
=== Request ===
Prompt: "Explain this function"
Model: claude-sonnet-4.5
Temperature: 0.0
Context files: 2
  - src/auth.ts (1500 tokens)
  - src/user.ts (800 tokens)
System prompt: 450 tokens
Total tokens: 2750
```

**Response Details:**
```
=== Response ===
Status: 200 OK
Latency: 2.3s
Tokens used: 1200
Finish reason: stop
Cache hit: false
```

**Errors:**
```
=== Error ===
Type: RateLimitError
Message: Rate limit exceeded
Retry after: 60s
Fallback: claude-haiku-4.5
```

### CLI Debug Mode

#### Enable Debug Logging
```bash
copilot --log-level debug
```

#### Debug Output Example
```
[DEBUG] Loading config from ~/.copilot/config.json
[DEBUG] Starting session: abc123
[DEBUG] Loading MCP servers...
[DEBUG] MCP server 'github-mcp-server' loaded
[DEBUG] Processing request...
[DEBUG] Model: claude-sonnet-4.5
[DEBUG] Context: 3 files, 5000 tokens
[DEBUG] Sending request...
[DEBUG] Response received: 200 OK (2.3s)
[DEBUG] Tokens: 1200 used, 184K remaining
```

#### Log File Location
```bash
# Latest log
~/.copilot/logs/latest.log

# Historical logs
~/.copilot/logs/2026-02-11.log
```

### Debugging Common Issues

#### Issue: Slow Responses
```
Check in Debug View:
- Token count (context too large?)
- Model selection (Opus slower than Haiku)
- Network latency
- Rate limiting

Solutions:
- Reduce context size
- Switch to faster model
- Check internet connection
- Wait for rate limit reset
```

#### Issue: Poor Quality Responses
```
Check in Debug View:
- Context files included
- Token distribution
- System prompt
- Model used

Solutions:
- Add relevant files to context
- Clarify prompt
- Use higher quality model
- Check instructions file
```

#### Issue: Context Not Included
```
Check in Debug View:
- Files resolved correctly
- Token limits not exceeded
- Permissions for file access

Solutions:
- Use absolute paths
- Verify file exists
- Check file size
- Grant file access permissions
```

## Advanced Settings

### Token Limits
```json
{
  "github.copilot.chat.maxContextTokens": 100000,
  "github.copilot.chat.maxResponseTokens": 4000,
  "github.copilot.chat.reserveTokens": 10000
}
```

### Caching
```json
{
  "github.copilot.chat.enableCaching": true,
  "github.copilot.chat.cacheDuration": 3600
}
```

### Experimental Features
```json
{
  "github.copilot.experimental": true,
  "github.copilot.chat.experimental.multifile": true,
  "github.copilot.chat.experimental.agents": true
}
```

### Privacy Settings
```json
{
  "github.copilot.telemetry": true,
  "github.copilot.chat.history": true,
  "github.copilot.chat.shareHistory": false
}
```

## Keyboard Shortcuts

### Chat Actions
| Action | Shortcut | Description |
|--------|----------|-------------|
| Open Chat | Cmd/Ctrl + I | Inline chat |
| Chat Panel | Cmd/Ctrl + Shift + I | Sidebar chat |
| Clear Chat | Cmd/Ctrl + K | Clear history |
| New Chat | Cmd/Ctrl + N | New conversation |
| Toggle Diagnostics | Cmd/Ctrl + Shift + D | Show/hide debug |

### Custom Shortcuts
```json
{
  "keybindings": [
    {
      "key": "cmd+shift+c",
      "command": "github.copilot.chat.clear"
    },
    {
      "key": "cmd+shift+m",
      "command": "github.copilot.chat.switchModel"
    }
  ]
}
```

## Monitoring and Telemetry

### Usage Statistics
```bash
# CLI usage stats
cat ~/.copilot/stats.json

# VS Code stats
Settings → Copilot → View Usage
```

### Performance Metrics
```
Average response time: 2.5s
Token usage per day: 150K
Premium requests used: 5/100
Most used model: claude-sonnet-4.5
Context window avg: 15K tokens
```

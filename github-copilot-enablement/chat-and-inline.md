# Chat and Inline Copilot

## Chat Interface

### Access
- **VS Code**: Cmd/Ctrl + I or click Copilot icon in sidebar
- **JetBrains**: Alt + Enter on selection
- **GitHub.com**: Click Copilot icon in navigation

### Chat Features

#### Context Selection
```
@workspace - Search entire workspace
@vscode - VS Code API help
#file:path/to/file.js - Specific file
#selection - Current editor selection
#terminalLastCommand - Last terminal command
#terminalSelection - Selected terminal text
```

#### Slash Commands
```
/help - Show available commands
/clear - Clear conversation
/new - Start new conversation
/model - Switch model
/agent - Switch agent
/explain - Explain code
/fix - Suggest fixes
/tests - Generate tests
/doc - Write documentation
```

#### Multi-File Context
```
What does #file:auth.js and #file:user.js do together?
Compare #file:old-api.ts with #file:new-api.ts
```

### Chat in GitHub.com

#### Access Points
- Repository page: Click Copilot icon
- Pull request: Copilot tab in PR sidebar
- File view: Ask about specific file

#### Features
- Ask about repository code
- Explain PRs and issues
- Search across repository
- Navigate code relationships
- Generate implementation suggestions

#### Example Queries
```
"Explain the authentication flow in this repo"
"What does the CI/CD pipeline do?"
"Find all database queries"
"How is error handling implemented?"
```

## Inline Copilot

### Ghost Text Suggestions
Appears as gray text while typing - accept with Tab/Enter

#### Triggering
- Automatic: Start typing function/comment
- Manual: Alt + \ (trigger suggestion)

#### Controls
- **Accept**: Tab or Enter
- **Accept word**: Cmd/Ctrl + →
- **Next suggestion**: Alt + ]
- **Previous suggestion**: Alt + [
- **Dismiss**: Esc

### Inline Chat (Cmd/Ctrl + I)

#### Quick Actions
```
Extract this to a function
Add error handling
Add type annotations
Convert to async/await
Optimize this loop
```

#### Code Transformation
```
1. Select code
2. Press Cmd/Ctrl + I
3. Type instruction: "Refactor using promises"
4. Review diff
5. Accept or Reject
```

### Inline vs Chat Comparison

| Feature | Inline | Chat |
|---------|--------|------|
| **Trigger** | Automatic while typing | Manual, Cmd+I |
| **Context** | Current file, nearby code | Multi-file, workspace |
| **Output** | Direct code insertion | Explanation + code |
| **Interaction** | Accept/Reject | Conversational |
| **Use Case** | Quick completions | Complex questions |

## Workflow Integration

### Development Flow
```
1. Inline: Write function structure
2. Inline: Accept completions for implementation  
3. Chat: "Does this handle edge cases?"
4. Inline: Accept suggested fixes
5. Chat: "/tests generate unit tests"
```

### Code Review Flow
```
1. Select problematic code
2. Cmd/Ctrl + I: "Identify issues"
3. Review suggestions in diff view
4. Chat: "Explain why this is better"
5. Accept changes
```

### Refactoring Flow
```
1. Chat: "Analyze architecture of #file:services/"
2. Chat: "Suggest refactoring approach"
3. Inline: Apply specific changes file-by-file
4. Chat: "/tests update tests for refactored code"
```

## Best Practices

### Inline
- Write clear function names and comments to guide suggestions
- Review suggestions before accepting
- Use Cmd/Ctrl + → to accept word-by-word for partial acceptance
- Dismiss (Esc) unhelpful suggestions to improve learning

### Chat
- Provide specific context with @ and # symbols
- Break complex tasks into smaller questions
- Use slash commands for common tasks
- Keep conversations focused on single topic
- Start new conversation (/new) for different context

### Context Management
- Add relevant files to chat with #file:
- Use @workspace for cross-file questions
- Reference #selection for specific code blocks
- Combine context: "@workspace #file:config.js #selection"

## Keyboard Shortcuts Summary

| Action | Shortcut | Notes |
|--------|----------|-------|
| Open Chat | Cmd/Ctrl + I | Opens inline chat |
| Chat Panel | Cmd/Ctrl + Shift + I | Opens sidebar chat |
| Accept Suggestion | Tab or Enter | Inline completion |
| Accept Word | Cmd/Ctrl + → | Partial acceptance |
| Next Suggestion | Alt + ] | Cycle suggestions |
| Previous Suggestion | Alt + [ | Cycle backward |
| Trigger Suggestion | Alt + \ | Manual trigger |
| Dismiss | Esc | Close inline chat |

## Settings Configuration

### VS Code Settings
```json
{
  "github.copilot.enable": true,
  "github.copilot.inlineSuggest.enable": true,
  "github.copilot.chat.welcomeMessage": "firstUse",
  "github.copilot.editor.enableAutoCompletions": true
}
```

### Disabling for Specific Files
```json
{
  "github.copilot.enable": {
    "*": true,
    "yaml": false,
    "plaintext": false,
    "markdown": false
  }
}
```

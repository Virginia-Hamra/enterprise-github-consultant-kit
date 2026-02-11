# Context Windows

## Overview
Context window is the amount of text (code, conversation, files) a model can process in a single request.

## Context Window Sizes by Model

| Model | Context Window | Practical Use |
|-------|----------------|---------------|
| Claude Opus 4.6 | ~200K tokens | ~150K lines of code |
| Claude Opus 4.5 | ~200K tokens | ~150K lines of code |
| Claude Sonnet 4.5 | ~200K tokens | ~150K lines of code |
| Claude Haiku 4.5 | ~200K tokens | ~150K lines of code |
| GPT-5.2 | ~128K tokens | ~96K lines of code |
| GPT-5.1 | ~128K tokens | ~96K lines of code |
| GPT-5 | ~32K tokens | ~24K lines of code |

**Note**: ~1 token ≈ 0.75 words or ~4 characters

## What Fills Context Window

### In a Request
1. **System prompt** (~2K tokens)
2. **Copilot instructions** from `.github/copilot-instructions.md`
3. **Conversation history** (all previous messages)
4. **Referenced files** via `#file:`, `@workspace`
5. **Code selections** via `#selection`
6. **User prompt** (current question)

### Example Context Usage
```
System prompt:              2,000 tokens
Copilot instructions:       1,000 tokens
Conversation history:       5,000 tokens
Referenced files (3 files): 8,000 tokens
Current prompt:             500 tokens
------------------------
Total:                      16,500 tokens
Remaining:                  ~183K tokens (Claude)
```

## Optimizing Context Usage

### 1. Reference Specific Files
```
❌ Inefficient:
@workspace "Find the authentication code"
(Scans entire workspace)

✓ Efficient:
#file:src/auth/jwt.ts "Review this auth implementation"
(Only includes one file)
```

### 2. Start New Conversations
```
When to use /new:
- Switching to unrelated task
- Context window getting full
- Conversation lost focus
- Starting new feature
```

### 3. Use Targeted Context
```
Instead of large files:
#file:large-file.ts:100-150
(Specific line range)

Or use #selection:
1. Select relevant code section
2. Reference with #selection in prompt
```

### 4. Summarize Long Conversations
```
After long conversation:
"Summarize the key decisions and save to 
#file:docs/feature-decisions.md"

Start new conversation:
"Continue from #file:docs/feature-decisions.md"
```

## Context Strategies by Task

### Small Tasks (< 5K tokens)
Single file changes, simple questions
```
#file:utils/format.ts
"Add error handling to this function"
```

### Medium Tasks (5K-50K tokens)
Feature implementation, multiple files
```
#file:src/auth/login.ts
#file:src/auth/session.ts
#file:src/models/user.ts
"Implement password reset feature"
```

### Large Tasks (50K-100K tokens)
Architecture review, system-wide changes
```
@workspace
#file:docs/architecture.md
"Review authentication system across codebase"
```

### Very Large Tasks (> 100K tokens)
Complex analysis, large codebases
```
Strategy: Break into smaller requests
1. "Analyze auth modules: #file:src/auth/"
2. "Analyze API layer: #file:src/api/"
3. "Synthesize findings from above analyses"
```

## Context Window Exhaustion

### Signs You're Hitting Limits
- Responses ignore earlier conversation
- Model "forgets" previous code
- Errors about request size
- Incomplete responses

### Solutions
```
1. Start new conversation (/new)
2. Reduce file references
3. Use summaries instead of full history
4. Switch to model with larger context (if available)
```

## Advanced: Context Preservation

### Strategy: Context Documents
Create summary documents for complex features:

```markdown
# Feature: User Authentication

## Architecture
- JWT-based tokens
- Refresh token rotation
- Redis session store

## Files
- src/auth/jwt.ts - Token generation
- src/auth/middleware.ts - Auth middleware
- src/auth/session.ts - Session management

## Key Decisions
- Access tokens: 15min expiry
- Refresh tokens: 7day expiry
- Hash algorithm: bcrypt rounds=10
```

Reference in future conversations:
```
#file:docs/auth-summary.md
"Add password reset feature"
```

### Strategy: Layered Context
Start broad, then narrow:

```
Request 1: @workspace "Find auth-related files"
Request 2: #file:auth/login.ts "Explain this file"
Request 3: #selection "Optimize this specific function"
```

## Context in Different Interfaces

### VS Code Chat
- Full conversation history preserved
- Files auto-referenced from open tabs
- `@workspace` indexes entire project

### GitHub.com Chat
- Repository code automatically available
- PR context included in PR tab
- File browsing integrated

### CLI
- Minimal automatic context
- Explicitly reference files
- Session state maintained with `--continue`

## Best Practices

### Do's
- ✓ Start new conversations for new topics
- ✓ Reference specific files when possible
- ✓ Use line ranges for large files
- ✓ Summarize long conversations
- ✓ Create context documents for complex features

### Don'ts
- ✗ Include unnecessary files
- ✗ Let conversations run indefinitely
- ✗ Reference entire workspace for small tasks
- ✗ Repeat information already in context
- ✗ Ignore context exhaustion warnings

## Monitoring Context Usage

### Check Token Count (Approximate)
```
VS Code:
- View → Command Palette
- "Copilot: Show Token Count"
(Feature availability varies by version)
```

### Manual Estimation
```
1 line of code ≈ 5-15 tokens
1 paragraph ≈ 50-100 tokens
1 file (500 lines) ≈ 2500-7500 tokens
```

### Context Budget Planning
```
Available: 200K tokens (Claude Sonnet 4.5)
Reserved: 50K tokens (response generation)
Usable: 150K tokens

Allocation:
- System/Instructions: 3K
- Conversation history: 20K
- Referenced files: 100K
- Current prompt: 2K
Remaining: 25K buffer
```

## Future Considerations

### Growing Context Windows
Models are expanding context windows - stay updated:
- Check model documentation periodically
- New models may support larger contexts
- Adjust strategies as limits increase

### Context Caching
Some providers cache repeated context:
- Reduces latency for common files
- Lowers cost for repeated references
- Automatic in many implementations

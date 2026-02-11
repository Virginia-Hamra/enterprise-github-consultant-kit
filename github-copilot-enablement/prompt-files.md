# Prompt Files

## Overview
Reusable prompt templates stored as files in `.github/prompts/` directory.

## Structure
```
.github/
└── prompts/
    ├── review-security.md
    ├── generate-tests.md
    └── refactor-component.md
```

## Creating Prompt Files

### Basic Format
```markdown
# Prompt Title

Instructions for Copilot to follow...

## Context
Additional context or requirements...

## Example
Optional example of desired output...
```

### Example: `review-security.md`
```markdown
# Security Review

Review this code for security vulnerabilities:
- SQL injection risks
- XSS vulnerabilities
- Authentication/authorization issues
- Sensitive data exposure
- Input validation gaps

Provide specific recommendations with code examples.
```

## Using Prompt Files

### In Chat
- Reference with `#file:.github/prompts/review-security.md`
- Or use `/prompt review-security` (if configured)

### Benefits
- Consistent review processes
- Team-shared best practices
- Reduced prompt engineering time
- Version-controlled prompts

## Best Practices
- Name files descriptively
- Keep prompts focused on single tasks
- Include examples of desired output
- Update based on team feedback

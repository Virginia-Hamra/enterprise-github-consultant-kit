# Custom Agents Configuration

## Agent.md File

Define custom agents with specialized instructions, tools, and capabilities.

## Location
Repository root: `AGENTS.md` or `.github/AGENTS.md`

## Basic Structure

```markdown
# Custom Agents

## @security-reviewer
Role: Security-focused code reviewer
Model: claude-opus-4.5
Instructions:
- Review code for OWASP Top 10 vulnerabilities
- Check authentication and authorization
- Validate input sanitization
- Flag sensitive data exposure
Tools: grep, view, web_search

## @test-generator
Role: Comprehensive test suite creator
Model: gpt-5.1-codex
Instructions:
- Generate unit tests with >90% coverage
- Include edge cases and error scenarios
- Use project testing framework
- Add descriptive test names
Tools: view, edit, bash

## @docs-writer
Role: Technical documentation specialist
Model: claude-sonnet-4.5
Instructions:
- Write clear, concise documentation
- Include code examples
- Follow project doc standards
- Update existing docs when relevant
Tools: view, edit, grep
```

## Agent Configuration Options

### Required Fields
- **Name**: `@agent-name` (must start with @)
- **Role**: Brief description of agent's purpose
- **Instructions**: Specific guidelines for the agent

### Optional Fields
- **Model**: Specific model to use (default: claude-sonnet-4.5)
- **Tools**: Comma-separated list of allowed tools
- **Temperature**: Creativity level (0.0-1.0)
- **MaxTokens**: Response length limit

## Advanced Example

```markdown
## @api-architect
Role: REST API design specialist
Model: claude-opus-4.5
Temperature: 0.3
Instructions:
You are an API design expert. Follow these principles:
1. Use RESTful conventions (GET, POST, PUT, DELETE)
2. Version all endpoints (/api/v1/...)
3. Return consistent error formats
4. Include pagination for list endpoints
5. Document with OpenAPI/Swagger
6. Implement proper HTTP status codes
7. Use noun-based resource names

Review API designs for:
- Consistency with REST principles
- Security (authentication, rate limiting)
- Performance (caching, pagination)
- Developer experience (clear naming, docs)

Tools: view, edit, web_search, grep
MaxTokens: 4000
```

## Using Custom Agents

### In Chat
```
@security-reviewer check this authentication code
@test-generator create tests for UserService
@docs-writer document this API endpoint
```

### In CLI
```bash
copilot --agent security-reviewer -p "Review auth.js"
```

## Best Practices

### Naming
- Use descriptive, role-based names
- Prefix with @ for easy reference
- Keep names short and memorable

### Instructions
- Be specific about expected behavior
- Include examples when helpful
- List clear success criteria
- Define boundaries and limitations

### Model Selection
- **claude-opus-4.5**: Complex reasoning, critical reviews
- **claude-sonnet-4.5**: Balanced performance, general tasks
- **gpt-5.1-codex**: Code generation, refactoring
- **claude-haiku-4.5**: Fast responses, simple tasks

### Tool Restrictions
- Limit tools to what agent needs
- Security-sensitive agents: read-only tools
- Testing agents: bash, view, edit
- Documentation agents: view, edit, grep

## Repository-Specific Agents

```markdown
## @migration-helper
Role: Database migration specialist for our PostgreSQL stack
Model: gpt-5.1-codex
Instructions:
- Use Knex.js for migrations
- Follow naming: YYYYMMDDHHMMSS_description.js
- Include both up() and down() methods
- Add transaction wrappers
- Document breaking changes
- Test rollback scenarios
Tools: view, edit, bash
```

## Team Collaboration
- Version control AGENTS.md
- Review agent changes in PRs
- Share effective agent configurations
- Document agent usage in team wiki

# Premium Requests and Model Usage

## Request Types

### Standard Requests (0x) - Included
Models that don't consume premium request quota:
- Claude Sonnet 4.5 ✓
- Claude Sonnet 4 ✓
- Claude Haiku 4.5 ✓
- GPT-5.2 Codex ✓
- GPT-5.2 ✓
- GPT-5.1 Codex Max ✓
- GPT-5.1 Codex ✓
- GPT-5.1 ✓
- GPT-5 ✓
- GPT-5.1 Codex Mini ✓
- GPT-5 Mini ✓
- GPT-4.1 ✓

### Premium Requests (1x) - Limited Quota
Models that consume premium quota:
- Claude Opus 4.6 ⭐
- Claude Opus 4.6 Fast ⭐
- Claude Opus 4.5 ⭐
- Gemini 3 Pro Preview ⭐

## Premium Request Quotas by Subscription

### Individual Subscriptions

| Subscription | Premium Requests | Resets |
|--------------|------------------|--------|
| Free | 0 | N/A |
| Copilot Individual | 50/month | Monthly |
| Copilot Pro | 100/month | Monthly |

### Enterprise Subscriptions

| Subscription | Premium Requests | Resets |
|--------------|------------------|--------|
| Copilot Business | 50/user/month | Monthly |
| Copilot Enterprise | 150/user/month | Monthly |

## Checking Your Quota

### VS Code
1. Open Copilot Chat
2. Click on settings gear icon
3. View "Premium Requests Remaining"

### CLI
```bash
copilot version
# Shows quota in output
```

### GitHub.com
1. Settings → Copilot
2. View usage dashboard

## Strategic Model Selection

### Default Choice: Claude Sonnet 4.5
Use for 80% of tasks:
- General coding questions
- Code generation
- Refactoring
- Documentation
- Standard reviews

**Why**: High quality, no premium cost, fast enough

### When to Use Premium (Opus 4.5/4.6)

#### Complex Architecture
```
✓ System design decisions
✓ Multi-service architecture review
✓ Database schema design
✓ API contract design
```

#### Critical Security
```
✓ Authentication system review
✓ Security vulnerability analysis
✓ Cryptography implementation
✓ Authorization logic validation
```

#### High-Stakes Refactoring
```
✓ Large-scale refactoring plans
✓ Breaking change analysis
✓ Migration strategies
✓ Legacy code modernization
```

#### Advanced Problem Solving
```
✓ Complex algorithm design
✓ Performance optimization strategies
✓ Distributed system challenges
✓ Debugging intricate issues
```

### When to Use Standard Models

#### Fast Models (Haiku, GPT Mini)
```
✓ Typo fixes
✓ Simple questions
✓ Quick code explanations
✓ Formatting changes
```

#### Coding Models (GPT Codex variants)
```
✓ CRUD operations
✓ Boilerplate generation
✓ Test generation
✓ API endpoint implementation
```

## Quota Conservation Strategies

### 1. Start with Standard, Escalate if Needed
```
Try: Claude Sonnet 4.5 (0x)
↓ If insufficient
Use: Claude Opus 4.5 (1x)
```

### 2. Batch Premium Requests
Consolidate questions:
```
❌ Bad: 5 separate Opus requests for related questions
✓ Good: 1 Opus request covering all aspects
```

### 3. Use Planning + Execution Pattern
```
Step 1: /model claude-opus-4.5
"Design architecture for user authentication system"
(1x premium used)

Step 2: /model gpt-5.1-codex  
"Implement the authentication design from above"
(0x premium used)
```

### 4. Delegate to Specialized Standard Models
```
Instead of Opus for everything:
- Code generation → GPT-5.1 Codex (0x)
- Documentation → Sonnet 4.5 (0x)
- Testing → GPT-5.1 Codex (0x)
- Architecture review → Opus 4.5 (1x)
```

### 5. Use Custom Agents Wisely
```markdown
## @quick-helper
Model: claude-haiku-4.5  # 0x
Instructions: Handle simple questions and fixes

## @architect
Model: claude-opus-4.5  # 1x
Instructions: Only for architectural decisions
```

## Monitoring Usage

### Track Monthly Usage
Create a simple tracker:
```
Premium requests used: 12/100
Current date: Feb 11
Days until reset: 19
Avg. per day: 0.63
Projected total: ~31
Remaining budget: ~69
```

### Best Practices
- Reserve 20% quota for month-end emergencies
- Front-load critical work early in month
- Use standard models for routine work
- Document when premium requests added value

## Cost-Benefit Analysis

### Premium Request ROI

**High ROI** (use premium):
- Prevents architectural mistakes (hours/days saved)
- Critical security review (potential breach prevented)
- Complex refactoring guidance (reduces risk)

**Low ROI** (use standard):
- Routine code generation (GPT Codex sufficient)
- Simple bug fixes (Sonnet/Haiku sufficient)
- Documentation writing (Sonnet sufficient)

## Team Quota Management (Enterprise)

### Shared Best Practices
```
1. Document premium use cases in team wiki
2. Share effective prompts to reduce retries
3. Review monthly usage in team meetings
4. Designate "premium approvers" for complex asks
5. Create standard agents for common tasks (0x)
```

### Team Guidelines Example
```
Premium Approval Required For:
- System architecture changes
- Security-critical code reviews
- Production incident analysis
- Database migration strategies

Standard Models Sufficient For:
- Feature implementation
- Unit test generation
- Code formatting
- Documentation updates
```

## Future-Proofing

### Model Performance Evolution
Standard models improve over time - periodically re-evaluate:
```
Quarterly Review:
- Can Sonnet 4.5 now handle tasks we used Opus for?
- Are new standard models available?
- Has our premium usage dropped naturally?
```

### Subscription Planning
```
If consistently hitting quota limits:
- Individual → Pro (50 → 100 requests)
- Business → Enterprise (50 → 150 requests)
- Document use cases to justify upgrade
```

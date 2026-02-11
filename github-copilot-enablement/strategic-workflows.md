# Strategic Workflows and Advice

## Pattern: Plan with Premium, Execute with Standard

### Strategy Overview
Use premium models (Opus) for high-level planning and architecture, then implement with standard models (Sonnet/Codex) to conserve quota.

### Workflow Example

#### Step 1: Architecture Planning (Premium)
```
Model: Claude Opus 4.5 (1x request)

Prompt:
"Design a scalable authentication system with:
- JWT token management
- Refresh token rotation
- Role-based access control
- Session management
- Password reset flow
- Multi-factor authentication

Provide:
1. Architecture diagram (text)
2. Component breakdown
3. Data models
4. Security considerations
5. Implementation order"
```

#### Step 2: Save Plan to File
```
Copy Opus response to: docs/auth-architecture.md
```

#### Step 3: Implementation (Standard)
```
Model: GPT-5.1 Codex (0x requests)

Prompt:
"Implement Step 1 from #file:docs/auth-architecture.md
Create JWT token service with generation and validation"
```

```
Model: GPT-5.1 Codex (0x requests)

Prompt:
"Implement Step 2 from #file:docs/auth-architecture.md
Create refresh token rotation logic"
```

### Cost Analysis
```
Traditional Approach:
- 5 Opus requests for implementation = 5x premium

Strategic Approach:
- 1 Opus request for planning = 1x premium
- 5 Codex requests for implementation = 0x premium
Total: 1x premium (80% savings)
```

## Pattern: Conversation to Instructions

### Strategy Overview
Let natural conversation capture requirements, then synthesize into formal instructions for consistent future use.

### Workflow Example

#### Step 1: Natural Conversation (Multiple turns)
```
You: "I need a REST API for managing users"
Copilot: "What operations do you need?"
You: "CRUD operations, plus password reset"
Copilot: "Authentication method?"
You: "JWT tokens"
You: "Also validate email format and strong passwords"
Copilot: "Pagination for list endpoint?"
You: "Yes, and filtering by role"
```

#### Step 2: Synthesize Instructions
```
Model: Claude Sonnet 4.5

Prompt:
"Review this entire conversation thread and create a formal
specification document that captures all requirements,
preferences, and decisions made. Format as a detailed
requirements doc that could guide implementation."
```

#### Step 3: Save as Project Instructions
```
Save output to: .github/copilot-instructions.md

Add project-specific patterns identified:
- JWT authentication standard
- Email/password validation rules
- Pagination approach (page, limit, filter)
- REST conventions used
```

#### Step 4: Use for Implementation
```
Model: GPT-5.1 Codex

Prompt:
"Implement the user management API based on
#file:.github/copilot-instructions.md"
```

### Benefits
- Captures informal knowledge formally
- Creates reusable patterns
- Ensures consistency across features
- Reduces future conversation rounds

## Pattern: Incremental Refinement

### Strategy Overview
Start with fast models for quick iterations, escalate to premium only when stuck or need deep analysis.

### Workflow Example

#### Iteration 1: Quick Draft (Fast)
```
Model: Claude Haiku 4.5

"Create a basic Express.js API endpoint for user registration"
```

#### Iteration 2: Add Features (Standard)
```
Model: GPT-5.1 Codex

"Add input validation and password hashing to registration endpoint"
```

#### Iteration 3: Security Review (Standard)
```
Model: Claude Sonnet 4.5

"Review this registration endpoint for security issues"
```

#### Iteration 4: Deep Analysis if Issues Found (Premium)
```
Model: Claude Opus 4.5

"Found timing attack vulnerability - design comprehensive fix
considering all attack vectors and edge cases"
```

## Pattern: Multi-Agent Assembly Line

### Strategy Overview
Route tasks through specialized agents with optimal models for each phase.

### Workflow Example

#### Assembly Line Setup (AGENTS.md)
```markdown
## @architect
Model: claude-opus-4.5
Instructions: High-level system design only

## @implementer  
Model: gpt-5.1-codex
Instructions: Code implementation from designs

## @tester
Model: gpt-5.1-codex
Instructions: Comprehensive test generation

## @documenter
Model: claude-sonnet-4.5
Instructions: Clear technical documentation
```

#### Execution Flow
```
1. @architect "Design payment processing system"
   → Output: docs/payment-design.md (1x premium)

2. @implementer "Implement #file:docs/payment-design.md"
   → Output: src/payment-service.ts (0x)

3. @tester "Generate tests for #file:src/payment-service.ts"
   → Output: tests/payment-service.test.ts (0x)

4. @documenter "Document #file:src/payment-service.ts"
   → Output: docs/payment-api.md (0x)
```

### Automation Script
```bash
#!/bin/bash
# assembly-line.sh

FEATURE=$1

echo "🏗️  Phase 1: Architecture"
copilot --agent architect -p "Design $FEATURE" > "docs/${FEATURE}-design.md"

echo "⚙️  Phase 2: Implementation"
copilot --agent implementer -p "Implement #file:docs/${FEATURE}-design.md"

echo "🧪 Phase 3: Testing"
copilot --agent tester -p "Test the $FEATURE implementation"

echo "📝 Phase 4: Documentation"
copilot --agent documenter -p "Document the $FEATURE"

echo "✅ Complete"
```

## Pattern: Context Building

### Strategy Overview
Build comprehensive context files that reduce need for repeated explanations.

### Workflow Example

#### Create Context Files
```
.github/
├── copilot-instructions.md      # General project rules
├── context/
│   ├── database-schema.md       # DB structure
│   ├── api-conventions.md       # REST patterns
│   ├── auth-flow.md             # Authentication
│   └── testing-standards.md     # Test requirements
```

#### Reference in Prompts
```
Before:
"Create a user endpoint with pagination, JWT auth, validation,
following REST conventions, using our PostgreSQL schema..."

After:
"Create user endpoint following #file:.github/context/api-conventions.md"
```

### Benefits
- Shorter prompts
- Consistent patterns
- Better context retention
- Faster responses

## Pattern: Batch Processing

### Strategy Overview
Consolidate similar tasks into single requests to reduce context switching and quota usage.

### Example: Multiple File Updates
```
❌ Inefficient:
5 separate requests to add error handling to 5 files

✓ Efficient:
"Add error handling to these 5 files following our pattern:
#file:services/user.js
#file:services/order.js
#file:services/payment.js
#file:services/notification.js
#file:services/analytics.js

Pattern from #file:.github/context/error-handling.md"
```

## Advanced Strategies

### Progressive Enhancement
```
1. Haiku: "Is this approach correct?" (Quick validation)
2. Sonnet: "Implement basic version"
3. Codex: "Add advanced features"
4. Opus: "Review for production readiness" (if critical)
```

### Context Layering
```
Generic → Specific:
1. @workspace (broad context)
2. #file:path/to/module (module context)
3. #selection (specific code)
```

### Parallel Processing
```
Open multiple VS Code windows:
- Window 1: @implementer working on backend
- Window 2: @implementer working on frontend  
- Window 3: @tester generating tests
- Window 4: @documenter writing docs
```

## Time-Based Strategies

### Early Month (Premium Available)
- Front-load architecture reviews
- Complex system designs
- Critical security audits
- Establish patterns for month

### Mid Month (Conserve Premium)
- Use standard models
- Leverage established patterns
- Incremental improvements
- Routine development

### Late Month (Preserve Remaining)
- Emergency-only premium use
- Rely on context files
- Use fastest standard models
- Defer non-critical deep analysis

## ROI Maximization

### High-Value Premium Use
```
✓ Initial architecture (1x) → Guides weeks of work
✓ Security audit (1x) → Prevents vulnerabilities
✓ Complex refactor plan (1x) → Reduces risk
✓ Performance strategy (1x) → Systematic optimization
```

### Low-Value Premium Use
```
✗ Routine CRUD (use Codex)
✗ Test generation (use Codex)
✗ Documentation (use Sonnet)
✗ Simple bug fixes (use Sonnet/Haiku)
```

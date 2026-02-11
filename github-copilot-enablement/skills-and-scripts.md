# Skills and Scripts

## Skills Overview
Skills are reusable capabilities that extend Copilot's functionality using custom scripts.

## Skill Directory Structure
```
.github/
└── skills/
    ├── analyze-performance/
    │   ├── skill.json
    │   └── analyze.sh
    └── generate-api-docs/
        ├── skill.json
        └── generate.py
```

## Creating a Skill

### 1. Skill Definition (`skill.json`)
```json
{
  "name": "analyze-performance",
  "description": "Analyze code performance and suggest optimizations",
  "script": "./analyze.sh",
  "inputs": [
    {
      "name": "file",
      "description": "File to analyze",
      "required": true
    }
  ]
}
```

### 2. Implementation Script (`analyze.sh`)
```bash
#!/bin/bash
# Script receives inputs as environment variables
FILE="$1"

# Your analysis logic
echo "Analyzing $FILE..."
# Output results in structured format
```

## Script Best Practices

### Input Handling
- Access inputs via command arguments or environment variables
- Validate all inputs before processing
- Provide clear error messages

### Output Format
- Use structured output (JSON, Markdown)
- Include actionable recommendations
- Keep output concise and scannable

### Error Handling
```bash
#!/bin/bash
set -e  # Exit on error

if [ ! -f "$1" ]; then
    echo "Error: File not found: $1" >&2
    exit 1
fi
```

## Using Skills

### In Copilot Chat
- Copilot automatically discovers skills in `.github/skills/`
- Reference by name: "Use the analyze-performance skill on main.js"
- Skills appear in command suggestions

## Example Skills

### Performance Analyzer
```json
{
  "name": "perf-check",
  "description": "Check for performance anti-patterns",
  "script": "./check.sh"
}
```

### Test Coverage Report
```json
{
  "name": "coverage-report",
  "description": "Generate test coverage report",
  "script": "./coverage.py"
}
```

## Advanced Features
- Chain multiple skills together
- Integrate with CI/CD tools
- Access repository context
- Use external APIs or services

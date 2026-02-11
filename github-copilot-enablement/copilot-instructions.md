# Copilot Instructions

## `.github/copilot-instructions.md`

Repository-level instructions that guide Copilot's behavior across all interactions.

### Purpose
- Define project-specific coding standards and conventions
- Set architectural patterns and preferences
- Establish testing and documentation requirements
- Apply to all Copilot features (Chat, Inline, CLI)

### Location
Place in repository root: `.github/copilot-instructions.md`

### Example Structure
```markdown
# Project Instructions

## Code Style
- Use TypeScript strict mode
- Prefer functional components with hooks
- Follow Airbnb style guide

## Architecture
- Use feature-based folder structure
- Separate business logic from UI components
- Implement repository pattern for data access

## Testing
- Write unit tests for all business logic
- Use Jest and React Testing Library
- Maintain >80% code coverage

## Documentation
- Add JSDoc comments for public APIs
- Update README for new features
- Include inline comments for complex logic
```

### Best Practices
- Keep instructions concise and actionable
- Update as project conventions evolve
- Review with team for consistency
- Avoid contradictory guidelines

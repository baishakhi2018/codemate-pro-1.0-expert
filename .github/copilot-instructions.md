# CodeMate Pro - AI Assistant Instructions

## Project Overview

CodeMate Pro is a multi-language component generator supporting React, Angular, Python, Node.js, and Java. This project uses AI-assisted development to maintain consistent, high-quality code.

## Technology Stack

**Frontend:**
- React 18+ with TypeScript
- Angular 14+ with TypeScript
- Tailwind CSS for styling

**Backend:**
- Python 3.8+ with FastAPI
- Node.js 16+ with Express
- Java 11+ with Spring Boot

## General Coding Standards

### Code Quality Principles
- Write clean, readable, and maintainable code
- Follow SOLID principles
- Keep functions focused (single responsibility)
- Use meaningful names for variables, functions, and classes
- Limit function length to 30 lines where practical
- Limit file length to 300 lines where practical

### Documentation
- Write self-documenting code
- Add comments only for complex logic
- Use JSDoc/TypeDoc for TypeScript/JavaScript
- Use docstrings for Python
- Use Javadoc for Java

### Error Handling
- Always handle potential errors gracefully
- Use try-catch for async operations
- Provide meaningful error messages
- Log errors with appropriate severity levels

### Testing
- Write unit tests for all business logic
- Aim for 80%+ code coverage
- Test edge cases and error conditions
- Use descriptive test names

## Language-Specific Guidelines

### TypeScript/JavaScript
- Use explicit types, avoid `any`
- Prefer `interface` over `type` for object shapes
- Use async/await over raw promises
- Enable strict mode in tsconfig

### React
- Use functional components with hooks
- One component per file
- Extract reusable logic into custom hooks
- Use proper prop typing with interfaces
- Implement proper cleanup in useEffect

### Angular
- Follow Angular style guide
- Use reactive forms over template forms
- Implement proper dependency injection
- Use async pipe for observables
- Implement OnDestroy for cleanup

### Python
- Follow PEP 8 style guide
- Use type hints for all functions
- Use dataclasses for data structures
- Implement context managers where appropriate
- Use async/await for I/O operations

### Java
- Follow Java naming conventions
- Use proper access modifiers
- Implement interfaces for abstraction
- Use Builder pattern for complex objects
- Implement AutoCloseable for resources

## File Naming Conventions

| Language | Convention | Example |
|----------|------------|---------|
| React | PascalCase.tsx | `UserCard.tsx` |
| Angular | kebab-case.component.ts | `user-card.component.ts` |
| Python | snake_case.py | `user_service.py` |
| Node.js | camelCase.js | `userService.js` |
| Java | PascalCase.java | `UserService.java` |

## Component Generation

When generating components, use the CLI:
```bash
codemate generate <framework> <component-name>
```

Supported frameworks: react, angular, python, node, java

## Code Review Checklist

Before committing code, ensure:
- [ ] Code follows style guidelines
- [ ] All tests pass
- [ ] No console.log or print statements (except logging)
- [ ] Error handling is implemented
- [ ] Documentation is updated
- [ ] No hardcoded values (use config/env)

## Security Guidelines

- Never commit secrets or API keys
- Validate all user inputs
- Use parameterized queries for databases
- Implement proper authentication/authorization
- Follow OWASP security guidelines

## Performance Guidelines

- Optimize database queries
- Implement caching where appropriate
- Use lazy loading for modules
- Minimize bundle sizes
- Profile and optimize hot paths

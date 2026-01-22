# Usage Guide

Learn how to use CodeMate Pro effectively for multi-language component generation.

## Basic Usage

### Command Line Generation

The basic syntax for generating components:

```bash
codemate generate <framework> <component-name> [output-directory]
```

**Short form:**
```bash
codemate g <framework> <component-name>
```

### Supported Frameworks

| Framework | Alias | Output Extension |
|-----------|-------|------------------|
| react | react | .tsx |
| angular | angular | .component.ts |
| python | python | .py |
| node | node | .js |
| java | java | .java |

## Examples by Framework

### React Components

```bash
# Basic component
codemate g react UserCard

# Custom output directory
codemate g react UserCard ./my-components/
```

**Output:** `src/components/react/UserCard.tsx`

Generated code includes:
- TypeScript interface for props
- Functional component with hooks
- useState and useEffect setup
- Tailwind CSS class structure

### Angular Components

```bash
codemate g angular user-profile
codemate g angular settings-panel
```

**Output:** `src/components/angular/user-profile.component.ts`

Generated code includes:
- @Component decorator
- @Input() and @Output() properties
- OnInit and OnDestroy lifecycle hooks
- Template and styles

### Python Modules

```bash
codemate g python data_processor
codemate g python auth_service
```

**Output:** `src/components/python/data_processor.py`

Generated code includes:
- Type hints
- Dataclass configuration
- Context manager support
- FastAPI integration example

### Node.js Services

```bash
codemate g node emailService
codemate g node cacheManager
```

**Output:** `src/components/node/emailService.js`

Generated code includes:
- JSDoc documentation
- EventEmitter extension
- Async/await patterns
- Express middleware factory

### Java Classes

```bash
codemate g java OrderManager
codemate g java PaymentProcessor
```

**Output:** `src/components/java/OrderManager.java`

Generated code includes:
- Javadoc comments
- Configuration inner class
- AutoCloseable implementation
- Spring Boot annotations (commented)

## Interactive Mode

For guided component creation:

```bash
codemate interactive
# or
codemate i
```

You'll be prompted for:
1. Component name
2. Framework selection
3. Additional options

## AI-Assisted Development

### Using with VS Code and Copilot

1. Open your project in VS Code
2. Create or open a file in the appropriate language
3. Type a comment describing what you need:

```typescript
// Create a user authentication hook with login, logout, and session management
```

4. Copilot will suggest code following project standards

### Using Agent Commands

In VS Code Copilot Chat:

```
@component-generator Create a ProductList component for React with filtering

@code-reviewer Review this component for performance issues

@architect Suggest architecture for a microservices e-commerce system
```

## Workflow Examples

### Creating a Complete Feature

**Step 1: Generate the component**
```bash
codemate g react ProductCard
```

**Step 2: Open in VS Code**
```bash
code src/components/react/ProductCard.tsx
```

**Step 3: Customize with AI assistance**
- Add specific props for your use case
- Implement business logic
- Add styling

**Step 4: Generate tests**
```
@testing-specialist Generate tests for this ProductCard component
```

### Multi-Language Project

Generate related components across languages:

```bash
# Frontend component
codemate g react UserDashboard

# Backend API endpoint
codemate g python user_api

# Node.js middleware
codemate g node authMiddleware

# Java service
codemate g java UserService
```

## Custom Output Directories

Specify custom output location:

```bash
# Output to specific directory
codemate g react Header ./src/layout/

# Output to current directory
codemate g python utils ./
```

## Batch Generation

Use shell scripting for batch generation:

```bash
# generate-components.sh
#!/bin/bash

components=("Header" "Footer" "Sidebar" "Navbar")

for comp in "${components[@]}"; do
  codemate g react "$comp"
done
```

## Integration with Build Tools

### npm Scripts

The `package.json` includes helpful scripts:

```bash
# Generate component
npm run generate -- react MyComponent

# Interactive mode
npm run interactive

# Development server
npm run dev

# Build
npm run build
```

### Custom npm Scripts

Add to your `package.json`:

```json
{
  "scripts": {
    "new:react": "codemate g react",
    "new:python": "codemate g python",
    "new:feature": "codemate interactive"
  }
}
```

Usage:
```bash
npm run new:react UserCard
```

## Best Practices

1. **Use descriptive names** - `UserProfileCard` not `Card1`
2. **Follow naming conventions** - PascalCase for React, snake_case for Python
3. **Generate in appropriate directories** - Keep components organized
4. **Customize after generation** - Templates are starting points
5. **Use AI agents** - For code review and optimization
6. **Document your components** - Add comments for complex logic

## Troubleshooting

### Component Not Generating

```bash
# Check if directory exists
ls src/components/react/

# Check permissions
ls -la src/components/
```

### Wrong Output Location

Specify the full path:
```bash
codemate g react MyComponent /absolute/path/to/output/
```

### Framework Not Recognized

Check spelling and use lowercase:
```bash
codemate g react MyComponent  # correct
codemate g React MyComponent  # incorrect
```

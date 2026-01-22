# Commands Reference

Complete reference for all CodeMate Pro CLI commands.

## CLI Syntax

```bash
codemate <command> [options] [arguments]
```

## Commands

### generate (g)

Generate a new component for a specific framework.

**Syntax:**
```bash
codemate generate <framework> <name> [output-dir]
codemate g <framework> <name> [output-dir]
```

**Arguments:**
| Argument | Required | Description |
|----------|----------|-------------|
| framework | Yes | Target framework (react, angular, python, node, java) |
| name | Yes | Component name |
| output-dir | No | Custom output directory |

**Examples:**
```bash
# Generate React component
codemate generate react UserCard
codemate g react UserCard

# Generate Angular component
codemate g angular user-settings

# Generate Python module
codemate g python data_handler

# Generate Node.js service
codemate g node cacheService

# Generate Java class
codemate g java OrderProcessor

# Custom output directory
codemate g react Header ./src/layout/
```

### list (l)

List all supported frameworks.

**Syntax:**
```bash
codemate list
codemate l
```

**Output:**
```
Supported Frameworks:
  • react (.tsx)
  • angular (.component.ts)
  • python (.py)
  • node (.js)
  • java (.java)
```

### interactive (i)

Launch interactive mode for guided component creation.

**Syntax:**
```bash
codemate interactive
codemate i
```

**Interactive Flow:**
1. Enter component name (or "exit" to quit)
2. Select framework from list
3. Component is generated
4. Repeat or exit

### help (h)

Display help information.

**Syntax:**
```bash
codemate help
codemate h
codemate --help
```

## npm Scripts

Available npm scripts in `package.json`:

```bash
# Generate component
npm run generate -- <framework> <name>

# Interactive mode
npm run interactive

# Development server
npm run dev

# Build project
npm run build

# Run tests
npm run test

# Lint code
npm run lint

# Format code
npm run format
```

## Framework Reference

### React

| Convention | Value |
|------------|-------|
| Naming | PascalCase |
| Extension | .tsx |
| Output | src/components/react/ |

**Example:**
```bash
codemate g react UserProfile
# Creates: src/components/react/UserProfile.tsx
```

### Angular

| Convention | Value |
|------------|-------|
| Naming | kebab-case |
| Extension | .component.ts |
| Output | src/components/angular/ |

**Example:**
```bash
codemate g angular user-profile
# Creates: src/components/angular/user-profile.component.ts
```

### Python

| Convention | Value |
|------------|-------|
| Naming | snake_case |
| Extension | .py |
| Output | src/components/python/ |

**Example:**
```bash
codemate g python user_service
# Creates: src/components/python/user_service.py
```

### Node.js

| Convention | Value |
|------------|-------|
| Naming | camelCase |
| Extension | .js |
| Output | src/components/node/ |

**Example:**
```bash
codemate g node userService
# Creates: src/components/node/userService.js
```

### Java

| Convention | Value |
|------------|-------|
| Naming | PascalCase |
| Extension | .java |
| Output | src/components/java/ |

**Example:**
```bash
codemate g java UserService
# Creates: src/components/java/UserService.java
```

## Exit Codes

| Code | Meaning |
|------|---------|
| 0 | Success |
| 1 | Error (invalid arguments, generation failed) |

## Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| CODEMATE_OUTPUT_DIR | Default output directory | ./src/components |
| CODEMATE_VERBOSE | Enable verbose logging | false |

## Common Patterns

### Generate Multiple Components

```bash
# Shell loop
for comp in Header Footer Sidebar; do
  codemate g react "$comp"
done
```

### Generate to Current Directory

```bash
codemate g react MyComponent ./
```

### Generate with Specific Path

```bash
codemate g react Button ./src/ui/buttons/
```

## Troubleshooting

### "Framework not supported"

Check spelling (must be lowercase):
```bash
codemate g react MyComponent  # ✓ Correct
codemate g React MyComponent  # ✗ Incorrect
```

### "Cannot create directory"

Check permissions:
```bash
chmod 755 src/components/
```

### "Command not found"

Use npm script or node directly:
```bash
npm run generate -- react MyComponent
# or
node src/generator/cli.js generate react MyComponent
```

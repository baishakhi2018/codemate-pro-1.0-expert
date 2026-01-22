# Quick Start Guide

Get started with CodeMate Pro in 5 minutes.

## Prerequisites

- Node.js 16 or higher
- npm or yarn
- VS Code (recommended) with GitHub Copilot extension

## Installation

```bash
# Clone the repository
git clone https://github.com/yourusername/codemate-pro.git
cd codemate-pro

# Install dependencies
npm install

# (Optional) Link CLI globally
npm link
```

## Generate Your First Component

### Option 1: Command Line

```bash
# React component
npm run generate react UserCard

# Angular component  
npm run generate angular user-profile

# Python module
npm run generate python data_handler

# Node.js service
npm run generate node apiClient

# Java class
npm run generate java OrderProcessor
```

### Option 2: Interactive Mode

```bash
npm run interactive
```

Follow the prompts to select framework and component name.

### Option 3: Direct CLI (if globally linked)

```bash
codemate generate react Dashboard
codemate g angular settings-panel
codemate g python auth_service
```

## Output Location

Generated components are saved to:
```
src/components/
├── react/         # React components (.tsx)
├── angular/       # Angular components (.component.ts)
├── python/        # Python modules (.py)
├── node/          # Node.js modules (.js)
└── java/          # Java classes (.java)
```

## Using AI Agents

If using VS Code with GitHub Copilot or another AI assistant:

```
@architect Review my system design
@code-reviewer Check this component for issues
@component-generator Create a LoginForm for React
@testing-specialist What tests should I write?
```

## Example Workflow

1. **Generate a component:**
   ```bash
   codemate g react ProductList
   ```

2. **Open in VS Code:**
   ```bash
   code src/components/react/ProductList.tsx
   ```

3. **Enhance with AI:**
   - Type comments describing what you need
   - Use agent commands for specialized help
   - AI will suggest code following project standards

4. **Run your app:**
   ```bash
   npm run dev
   ```

## Next Steps

- Read the full [README](README.md)
- Explore [AI Agents](AGENTS_GUIDE.md)
- Check [Documentation](docs/)
- Customize templates in `src/templates/`

## Troubleshooting

**Command not found:**
```bash
# Use npm script instead
npm run generate react MyComponent
```

**Permission denied:**
```bash
chmod +x src/generator/cli.js
```

**Node version error:**
```bash
# Check Node version (need 16+)
node --version
```

---

Happy coding! 🚀

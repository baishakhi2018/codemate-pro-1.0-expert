# Setup Guide

Complete installation and setup instructions for CodeMate Pro.

## Requirements

- **Node.js**: Version 16.0.0 or higher
- **npm**: Version 8.0.0 or higher (comes with Node.js)
- **Git**: For cloning the repository
- **VS Code**: Recommended IDE with GitHub Copilot extension

## Installation Steps

### 1. Clone the Repository

```bash
git clone https://github.com/yourusername/codemate-pro.git
cd codemate-pro
```

### 2. Install Dependencies

```bash
npm install
```

### 3. Verify Installation

```bash
# Test the CLI
node src/generator/cli.js help

# Or use npm script
npm run generate -- react TestComponent
```

### 4. Global CLI (Optional)

To use `codemate` command globally:

```bash
npm link
```

Now you can use:
```bash
codemate generate react MyComponent
```

## VS Code Configuration

### Recommended Extensions

1. **GitHub Copilot** - AI pair programming
2. **ESLint** - JavaScript/TypeScript linting
3. **Prettier** - Code formatting
4. **Tailwind CSS IntelliSense** - Tailwind autocomplete

### Settings

Add to your VS Code settings (`.vscode/settings.json`):

```json
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "typescript.preferences.importModuleSpecifier": "relative",
  "github.copilot.enable": {
    "*": true
  }
}
```

## Project Structure After Setup

```
codemate-pro/
├── .github/
│   ├── agents/           # AI agent definitions
│   ├── instructions/     # Language-specific instructions
│   ├── prompts/          # Reusable prompts
│   └── copilot-instructions.md
├── src/
│   ├── generator/        # CLI source code
│   │   └── cli.js
│   ├── components/       # Generated components
│   │   ├── react/
│   │   ├── angular/
│   │   ├── python/
│   │   ├── node/
│   │   └── java/
│   └── templates/        # Customizable templates
├── docs/                 # Documentation
├── package.json
└── README.md
```

## Verifying Setup

### Test Component Generation

```bash
# Generate a test component
codemate generate react TestButton

# Check the output
cat src/components/react/TestButton.tsx
```

### Test Interactive Mode

```bash
npm run interactive
```

## Environment Variables (Optional)

Create a `.env` file for custom configuration:

```env
# Default output directory
CODEMATE_OUTPUT_DIR=./src/components

# Enable verbose logging
CODEMATE_VERBOSE=true
```

## Troubleshooting

### "Command not found" Error

If `codemate` command isn't recognized:

```bash
# Use npm script instead
npm run generate -- react MyComponent

# Or run directly
node src/generator/cli.js generate react MyComponent
```

### Permission Denied

```bash
chmod +x src/generator/cli.js
```

### Node Version Error

```bash
# Check your Node version
node --version

# If below 16, upgrade Node.js
# Using nvm:
nvm install 18
nvm use 18
```

### Dependencies Not Installing

```bash
# Clear npm cache
npm cache clean --force

# Remove node_modules and reinstall
rm -rf node_modules package-lock.json
npm install
```

## Next Steps

1. Read the [Usage Guide](USAGE.md)
2. Try [generating components](COMMANDS.md)
3. Explore [AI agents](../AGENTS_GUIDE.md)
4. [Customize templates](CUSTOMIZATION.md)

## Updating

To update to the latest version:

```bash
git pull origin main
npm install
```

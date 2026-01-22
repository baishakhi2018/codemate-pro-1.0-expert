# CodeMate Pro 🚀

**A Multi-Language Component Generator with AI-Assisted Development Support**

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Node.js](https://img.shields.io/badge/Node.js-16+-green.svg)](https://nodejs.org)

---

## Overview

CodeMate Pro is a comprehensive toolkit for generating components across multiple programming languages and frameworks. It integrates with AI assistants (GitHub Copilot, Claude, etc.) to provide context-aware code suggestions and includes expert agents for specialized tasks.

**Supported Languages/Frameworks:**
- ⚛️ **React** (TypeScript/JSX)
- 🅰️ **Angular** (TypeScript)
- 🐍 **Python** (FastAPI/Pydantic)
- 📗 **Node.js** (Express/CommonJS)
- ☕ **Java** (Spring Boot)

---

## Features

### 🎯 Component Generator CLI
Generate production-ready components from the command line or in interactive mode.

```bash
# Generate a React component
codemate generate react UserCard

# Generate an Angular component
codemate generate angular user-profile

# Generate a Python module
codemate generate python data_processor

# Generate a Node.js service
codemate generate node authService

# Generate a Java class
codemate generate java OrderManager
```

### 🤖 AI Agent Modes
Expert AI agents for specialized tasks:
- **@code-reviewer** - Automated code review
- **@architect** - System design guidance
- **@testing-specialist** - Test strategy recommendations
- **@devops-engineer** - CI/CD and infrastructure
- **@security-expert** - Security audit assistance

### 📝 Custom Instructions
Framework-specific coding standards and best practices automatically applied to your projects.

### 💬 Chat Modes
Specialized conversation modes for different development contexts.

---

## Quick Start

### Installation

```bash
# Clone the repository
git clone https://github.com/yourusername/codemate-pro.git
cd codemate-pro

# Install dependencies
npm install

# Link CLI globally (optional)
npm link
```

### Generate Components

**Command Line:**
```bash
# Using npm script
npm run generate react MyComponent

# Or directly with node
node src/generator/cli.js generate react MyComponent

# If globally linked
codemate generate react MyComponent
```

**Interactive Mode:**
```bash
npm run interactive
# or
codemate interactive
```

### With AI Assistants

1. Open your project in VS Code
2. The `.github/` folder contains instructions that AI assistants automatically detect
3. Use agent commands like `@architect` for specialized help
4. Use prompts like `/add-feature` for guided workflows

---

## Project Structure

```
codemate-pro/
├── .github/
│   ├── agents/              # AI agent definitions
│   ├── instructions/        # Language-specific instructions
│   ├── prompts/             # Reusable prompt templates
│   ├── chatmodes/           # Chat mode configurations
│   └── collections/         # Bundled workflows
├── src/
│   ├── generator/           # Component generator CLI
│   ├── components/          # Generated components output
│   │   ├── react/
│   │   ├── angular/
│   │   ├── python/
│   │   ├── node/
│   │   └── java/
│   └── templates/           # Component templates
├── docs/                    # Documentation
├── package.json
└── README.md
```

---

## CLI Commands

| Command | Description |
|---------|-------------|
| `codemate generate <framework> <name>` | Generate a component |
| `codemate g <framework> <name>` | Short form of generate |
| `codemate list` | List supported frameworks |
| `codemate interactive` | Launch interactive mode |
| `codemate help` | Show help information |

### Examples

```bash
# React TypeScript component
codemate g react UserProfile
# Output: src/components/react/UserProfile.tsx

# Angular component
codemate g angular user-settings
# Output: src/components/angular/user-settings.component.ts

# Python FastAPI service
codemate g python payment_processor
# Output: src/components/python/payment_processor.py

# Node.js Express module
codemate g node emailService
# Output: src/components/node/emailService.js

# Java Spring component
codemate g java NotificationManager
# Output: src/components/java/NotificationManager.java
```

---

## AI Agents

### Available Agents

| Agent | Purpose |
|-------|---------|
| `@code-reviewer` | Code quality analysis and suggestions |
| `@architect` | System design and architecture guidance |
| `@testing-specialist` | Test strategies and coverage |
| `@devops-engineer` | CI/CD and deployment |
| `@security-expert` | Security audits and recommendations |

### Usage

In your AI assistant chat:
```
@architect Review the architecture for a microservices e-commerce system

@code-reviewer Check this component for performance issues

@testing-specialist What tests should I write for this authentication module?
```

---

## Custom Instructions

Language-specific instructions are in `.github/instructions/`:

- `react.instructions.md` - React/TypeScript best practices
- `angular.instructions.md` - Angular patterns and standards
- `python.instructions.md` - Python/FastAPI conventions
- `java.instructions.md` - Java/Spring Boot guidelines
- `typescript.instructions.md` - TypeScript configuration

These are automatically detected by AI assistants when working in your project.

---

## Generated Component Examples

### React Component
```tsx
import React, { useState, useEffect } from 'react';

interface UserCardProps {
  className?: string;
  children?: React.ReactNode;
}

export const UserCard: React.FC<UserCardProps> = ({ className = '', children }) => {
  const [isLoading, setIsLoading] = useState(false);
  // ... component logic
};
```

### Python Module
```python
from dataclasses import dataclass
from typing import Dict, Any

@dataclass
class DataProcessorConfig:
    enabled: bool = True
    timeout: int = 30

class DataProcessor:
    def __init__(self, config: DataProcessorConfig = None):
        self.config = config or DataProcessorConfig()
    
    def execute(self, data: Dict[str, Any]) -> Dict[str, Any]:
        # ... processing logic
```

### Java Class
```java
public class OrderManager implements AutoCloseable {
    private final OrderManagerConfig config;
    
    public OrderManager(OrderManagerConfig config) {
        this.config = config;
    }
    
    public OrderManagerResult execute(Map<String, Object> data) {
        // ... execution logic
    }
}
```

---

## Configuration

### VS Code Settings

Add to `.vscode/settings.json`:
```json
{
  "github.copilot.enable": {
    "*": true
  }
}
```

### Customization

Edit templates in `src/templates/` to customize generated code for your project's style.

---

## Contributing

Contributions are welcome! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

---

## License

MIT License - see [LICENSE](LICENSE) for details.

---

## Acknowledgments

This project is inspired by community efforts to enhance AI-assisted development, including the concept of customizable instructions and agents pioneered by projects like [awesome-copilot](https://github.com/github/awesome-copilot).

**Note:** This is an independent project. All code is original and not copied from any source.

---

## Support

- 📧 Issues: [GitHub Issues](https://github.com/yourusername/codemate-pro/issues)
- 💬 Discussions: [GitHub Discussions](https://github.com/yourusername/codemate-pro/discussions)

---

**Built for developers who want consistent, high-quality code generation across multiple languages.**

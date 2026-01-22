# AI Agents Guide

CodeMate Pro includes specialized AI agents for different development tasks. These agents work with AI assistants like GitHub Copilot to provide context-aware assistance.

## Available Agents

### @component-generator
**Purpose:** Generate components for any supported framework

**Usage:**
```
@component-generator Create a UserCard component for React with avatar and stats
@component-generator Generate an authentication service for Python
```

**Capabilities:**
- React TypeScript components
- Angular components with services
- Python modules and FastAPI endpoints
- Node.js services and middleware
- Java classes and Spring components

---

### @code-reviewer
**Purpose:** Comprehensive code review and quality analysis

**Usage:**
```
@code-reviewer Review this component for best practices
@code-reviewer Check for security vulnerabilities in this API
```

**Focuses on:**
- Code quality and readability
- Performance optimizations
- Security concerns
- Best practice adherence
- Error handling

---

### @architect
**Purpose:** System design and architecture guidance

**Usage:**
```
@architect Design a microservices architecture for e-commerce
@architect Review our database schema design
```

**Expertise:**
- System design patterns
- Microservices architecture
- Database design
- Scalability strategies
- Cloud architecture (AWS, Azure, GCP)

---

### @testing-specialist
**Purpose:** Test strategy and implementation guidance

**Usage:**
```
@testing-specialist What tests should I write for this authentication module?
@testing-specialist Help me set up integration tests
```

**Covers:**
- Unit testing strategies
- Integration testing
- End-to-end testing
- Test coverage analysis
- Mocking and fixtures

---

### @devops-engineer
**Purpose:** CI/CD and infrastructure guidance

**Usage:**
```
@devops-engineer Set up GitHub Actions for this project
@devops-engineer Review our Docker configuration
```

**Expertise:**
- CI/CD pipelines
- Docker and Kubernetes
- Infrastructure as Code
- Monitoring and logging
- Deployment strategies

---

### @security-expert
**Purpose:** Security analysis and recommendations

**Usage:**
```
@security-expert Audit this API for security vulnerabilities
@security-expert Review authentication implementation
```

**Focuses on:**
- OWASP Top 10
- Authentication/Authorization
- Input validation
- Encryption practices
- Security headers

## How Agents Work

Agents are defined in `.github/agents/` directory. Each agent has:

1. **Name and description** - What the agent specializes in
2. **Expertise areas** - Specific knowledge domains
3. **Response format** - How the agent structures responses
4. **Guidelines** - Best practices the agent follows

## Using Multiple Agents

You can chain agents for complex tasks:

```
1. @architect Design the system architecture
2. @component-generator Create the main components
3. @code-reviewer Review the generated code
4. @testing-specialist Add appropriate tests
5. @security-expert Security audit
```

## Customizing Agents

### Adding a New Agent

Create a file in `.github/agents/` with the pattern:
`your-agent.agent.md`

**Template:**
```markdown
---
name: Your Agent Name
description: Brief description of the agent's purpose
---

# Your Agent Name

You are a specialized agent for [purpose].

## Expertise
- Area 1
- Area 2
- Area 3

## Response Format
[How to structure responses]

## Guidelines
[Best practices and rules]
```

### Modifying Existing Agents

Edit the agent files in `.github/agents/` to:
- Add new expertise areas
- Modify response formats
- Update guidelines

## Best Practices

1. **Be specific** - Give agents clear, detailed requests
2. **Provide context** - Share relevant code or requirements
3. **Use the right agent** - Match agents to tasks
4. **Review outputs** - Agents assist, humans verify
5. **Iterate** - Refine requests based on responses

## Agent Files Location

```
.github/
└── agents/
    ├── architect.agent.md
    ├── code-reviewer.agent.md
    ├── component-generator.agent.md
    ├── devops-engineer.agent.md
    ├── security-expert.agent.md
    └── testing-specialist.agent.md
```

# CodeMate Pro - Complete Command Reference

> All CLI commands, Agent commands (@), and Prompt commands (/) in one place.

---

## Table of Contents

- [CLI Commands](#cli-commands)
- [Agent Commands (@)](#agent-commands-)
- [Prompt Commands (/)](#prompt-commands-)
- [Quick Reference Card](#quick-reference-card)

---

## CLI Commands

### Component Generation

| Command | Short | Description |
|---------|-------|-------------|
| `codemate generate <framework> <name>` | `codemate g` | Generate a component |
| `codemate test <framework> <name>` | `codemate t` | Generate a test file |
| `codemate both <framework> <name>` | `codemate b` | Generate component + test |
| `codemate list` | `codemate l` | List supported frameworks |
| `codemate interactive` | `codemate i` | Interactive mode |
| `codemate help` | `codemate h` | Show help |

### Supported Frameworks

| Framework | Extension | Naming Convention |
|-----------|-----------|-------------------|
| `react` | `.tsx` | PascalCase |
| `angular` | `.component.ts` | kebab-case |
| `python` | `.py` | snake_case |
| `node` | `.js` | camelCase |
| `java` | `.java` | PascalCase |

### Component Generation Examples

```bash
# React
codemate g react UserCard
codemate g react ProductList
codemate g react LoginForm

# Angular
codemate g angular user-profile
codemate g angular settings-panel
codemate g angular data-table

# Python
codemate g python user_service
codemate g python payment_handler
codemate g python auth_middleware

# Node.js
codemate g node authService
codemate g node cacheManager
codemate g node emailHandler

# Java
codemate g java UserController
codemate g java OrderService
codemate g java PaymentProcessor
```

### Test Generation Examples

```bash
# React Tests
codemate t react UserCard           # → UserCard.test.tsx
codemate t react ProductList        # → ProductList.test.tsx

# Angular Tests
codemate t angular user-profile     # → user-profile.component.spec.ts
codemate t angular settings-panel   # → settings-panel.component.spec.ts

# Python Tests
codemate t python user_service      # → test_user_service.py
codemate t python auth_handler      # → test_auth_handler.py

# Node.js Tests
codemate t node authService         # → authService.test.js
codemate t node cacheManager        # → cacheManager.test.js

# Java Tests
codemate t java UserController      # → UserControllerTest.java
codemate t java OrderService        # → OrderServiceTest.java
```

### Generate Component + Test Together

```bash
# Creates both component and test file
codemate b react UserCard
codemate b angular user-profile
codemate b python data_processor
codemate b node paymentService
codemate b java CustomerManager
```

### Output Locations

```
src/components/
├── react/
│   ├── UserCard.tsx
│   └── __tests__/
│       └── UserCard.test.tsx
├── angular/
│   ├── user-profile.component.ts
│   └── __tests__/
│       └── user-profile.component.spec.ts
├── python/
│   ├── user_service.py
│   └── __tests__/
│       └── test_user_service.py
├── node/
│   ├── authService.js
│   └── __tests__/
│       └── authService.test.js
└── java/
    ├── UserController.java
    └── __tests__/
        └── UserControllerTest.java
```

---

## Agent Commands (@)

### Available Agents

| Agent | Command | Purpose |
|-------|---------|---------|
| Component Generator | `@component-generator` | Generate components for any framework |
| Code Reviewer | `@code-reviewer` | Code quality, security, best practices |
| Architect | `@architect` | System design, microservices, cloud |
| Testing Specialist | `@testing-specialist` | Test strategy, TDD, BDD, automation |
| DevOps Engineer | `@devops-engineer` | CI/CD, Docker, Kubernetes, infrastructure |
| Security Expert | `@security-expert` | Security audits, OWASP, vulnerabilities |

---

### @component-generator

Generate production-ready components for multiple frameworks.

**Examples:**
```
@component-generator Create a UserCard component for React with avatar, name, and email

@component-generator Generate a PaymentService for Python with validation

@component-generator Create a REST API controller for Java Spring Boot

@component-generator Build an Angular form component with validation

@component-generator Generate a Node.js authentication middleware

@component-generator Create a dashboard component for React with charts and stats

@component-generator Generate a file upload service for Node.js

@component-generator Create a data access layer for Python with SQLAlchemy
```

---

### @code-reviewer

Expert code review focusing on best practices, security, and maintainability.

**Examples:**
```
@code-reviewer Review this component for best practices

@code-reviewer Check this function for security vulnerabilities

@code-reviewer Analyze this code for performance issues

@code-reviewer Review error handling in this service

@code-reviewer Check if this follows SOLID principles

@code-reviewer Review this PR for code quality

@code-reviewer Check for memory leaks in this component

@code-reviewer Analyze this API endpoint for potential issues
```

---

### @architect

System design and architecture expert for scalable solutions.

**Examples:**
```
@architect Design a microservices architecture for e-commerce

@architect Review our database schema for scalability

@architect Suggest architecture for a real-time chat application

@architect How should we structure our API gateway?

@architect Evaluate monolith vs microservices for our use case

@architect Design a caching strategy for high-traffic API

@architect Review our event-driven architecture

@architect Suggest a data pipeline architecture for analytics
```

---

### @testing-specialist

Expert in test strategy, test automation, and quality assurance.

**Examples:**
```
@testing-specialist What tests should I write for this auth module?

@testing-specialist Help me set up integration tests for this API

@testing-specialist Generate unit tests for this service class

@testing-specialist What's the best testing strategy for React components?

@testing-specialist How to mock external API calls in tests?

@testing-specialist Review my test coverage and suggest improvements

@testing-specialist Help me write E2E tests for checkout flow

@testing-specialist What's the testing pyramid for this project?
```

---

### @devops-engineer

Expert in CI/CD, infrastructure, containerization, and cloud deployment.

**Examples:**
```
@devops-engineer Set up GitHub Actions CI/CD for this project

@devops-engineer Create a Dockerfile for this Node.js app

@devops-engineer Design a Kubernetes deployment strategy

@devops-engineer Set up monitoring with Prometheus and Grafana

@devops-engineer How to implement blue-green deployment?

@devops-engineer Create a Terraform script for AWS infrastructure

@devops-engineer Set up automatic scaling for our services

@devops-engineer Design a disaster recovery strategy
```

---

### @security-expert

Application security specialist for vulnerabilities and compliance.

**Examples:**
```
@security-expert Audit this API for security vulnerabilities

@security-expert Review authentication implementation

@security-expert Check for SQL injection vulnerabilities

@security-expert How to properly store passwords?

@security-expert Review this code for OWASP Top 10 issues

@security-expert Implement rate limiting for this API

@security-expert Review our JWT token implementation

@security-expert Check for XSS vulnerabilities in this component
```

---

## Prompt Commands (/)

### Code & Feature Prompts

| Prompt | Usage | Description |
|--------|-------|-------------|
| `/add-feature` | `/add-feature user authentication` | Guide for adding new features |
| `/code-review` | `/code-review` | Comprehensive code review checklist |

### Test Generation Prompts

| Prompt | Usage | Description |
|--------|-------|-------------|
| `/generate-unit-test` | `/generate-unit-test UserCard` | Generate unit tests |
| `/generate-integration-test` | `/generate-integration-test POST /api/users` | Generate integration tests |
| `/generate-e2e-test` | `/generate-e2e-test login flow` | Generate E2E tests |
| `/generate-api-test` | `/generate-api-test GET /api/products` | Generate API tests |
| `/generate-mock-data` | `/generate-mock-data User` | Generate mock data & fixtures |
| `/analyze-coverage` | `/analyze-coverage UserService` | Analyze test coverage gaps |

---

### /add-feature

Guide for adding new features to the project.

**Examples:**
```
/add-feature Add user authentication with JWT

/add-feature Add shopping cart functionality

/add-feature Add file upload feature with drag-and-drop

/add-feature Add real-time notifications

/add-feature Add search with filters and pagination
```

---

### /code-review

Comprehensive code review checklist.

**Examples:**
```
/code-review

/code-review Check this component for React best practices

/code-review Review this API endpoint
```

---

### /generate-unit-test

Generate unit tests for components and functions.

**Examples:**
```
/generate-unit-test UserCard component with props: name, email, avatar

/generate-unit-test calculateTotal function

/generate-unit-test AuthService class

/generate-unit-test validateEmail utility function

/generate-unit-test useCart custom hook
```

---

### /generate-integration-test

Generate integration tests for APIs and services.

**Examples:**
```
/generate-integration-test POST /api/users create user

/generate-integration-test GET /api/products with pagination

/generate-integration-test authentication flow

/generate-integration-test database operations for UserRepository

/generate-integration-test payment processing service
```

---

### /generate-e2e-test

Generate end-to-end tests for user workflows.

**Examples:**
```
/generate-e2e-test user login flow

/generate-e2e-test shopping cart checkout

/generate-e2e-test user registration with email verification

/generate-e2e-test product search and filter

/generate-e2e-test form submission with validation
```

---

### /generate-api-test

Generate REST/GraphQL API tests.

**Examples:**
```
/generate-api-test GET /api/users list with pagination

/generate-api-test POST /api/orders create order

/generate-api-test PUT /api/users/:id update user

/generate-api-test DELETE /api/products/:id

/generate-api-test GraphQL query for user profile
```

---

### /generate-mock-data

Generate mock data, factories, and fixtures.

**Examples:**
```
/generate-mock-data User with id, name, email, role

/generate-factory Product with price, category, stock

/generate-fixture orders for e-commerce

/generate-mock-data API responses for user endpoints

/generate-factory Comment with author, content, timestamp
```

---

### /analyze-coverage

Analyze test coverage and suggest improvements.

**Examples:**
```
/analyze-coverage UserService

/analyze-coverage src/components/

/analyze-coverage --plan

/analyze-coverage PaymentController identify gaps

/analyze-coverage suggest missing tests
```

---

## Quick Reference Card

```
╔═══════════════════════════════════════════════════════════════════════════════╗
║                    CODEMATE PRO - QUICK REFERENCE                             ║
╠═══════════════════════════════════════════════════════════════════════════════╣
║                                                                               ║
║  CLI COMMANDS                                                                 ║
║  ─────────────────────────────────────────────────────────────────────────── ║
║  codemate g <framework> <name>     Generate component                         ║
║  codemate t <framework> <name>     Generate test file                         ║
║  codemate b <framework> <name>     Generate component + test                  ║
║  codemate l                        List frameworks                            ║
║  codemate i                        Interactive mode                           ║
║                                                                               ║
║  Frameworks: react | angular | python | node | java                           ║
║                                                                               ║
╠═══════════════════════════════════════════════════════════════════════════════╣
║                                                                               ║
║  AGENT COMMANDS (@)                                                           ║
║  ─────────────────────────────────────────────────────────────────────────── ║
║  @component-generator    Generate code components                             ║
║  @code-reviewer          Review code quality & security                       ║
║  @architect              System design & architecture                         ║
║  @testing-specialist     Test strategy & automation                           ║
║  @devops-engineer        CI/CD, Docker, Kubernetes                            ║
║  @security-expert        Security audits & vulnerabilities                    ║
║                                                                               ║
╠═══════════════════════════════════════════════════════════════════════════════╣
║                                                                               ║
║  PROMPT COMMANDS (/)                                                          ║
║  ─────────────────────────────────────────────────────────────────────────── ║
║  /add-feature                 Add new feature guide                           ║
║  /code-review                 Code review checklist                           ║
║  /generate-unit-test          Generate unit tests                             ║
║  /generate-integration-test   Generate integration tests                      ║
║  /generate-e2e-test           Generate E2E tests                              ║
║  /generate-api-test           Generate API tests                              ║
║  /generate-mock-data          Generate mock data & fixtures                   ║
║  /analyze-coverage            Analyze test coverage                           ║
║                                                                               ║
╚═══════════════════════════════════════════════════════════════════════════════╝
```

---

## Workflow Examples

### Example 1: Create a New Feature

```bash
# 1. Generate component
codemate g react UserProfile

# 2. Ask architect for design guidance
@architect How should UserProfile integrate with our auth system?

# 3. Generate tests
codemate t react UserProfile

# 4. Review code
@code-reviewer Review UserProfile component

# 5. Security check
@security-expert Check UserProfile for vulnerabilities
```

### Example 2: Full-Stack Feature

```bash
# Backend
codemate g python user_api
codemate g node userController
codemate g java UserService

# Frontend
codemate g react UserDashboard
codemate g angular user-settings

# Tests for all
codemate t python user_api
codemate t node userController
codemate t java UserService
codemate t react UserDashboard
```

### Example 3: Testing Workflow

```bash
# Generate component with tests
codemate b react PaymentForm

# Ask for more test scenarios
@testing-specialist What edge cases should I test for PaymentForm?

# Generate integration tests
/generate-integration-test payment processing

# Generate E2E tests
/generate-e2e-test checkout flow

# Analyze coverage
/analyze-coverage PaymentForm
```

### Example 4: DevOps Setup

```bash
# Ask for CI/CD setup
@devops-engineer Set up GitHub Actions for this React + Node project

# Ask for Docker setup
@devops-engineer Create Dockerfile for production

# Ask for deployment strategy
@devops-engineer Design Kubernetes deployment with auto-scaling
```

---

## File Locations

```
.github/
├── agents/
│   ├── architect.agent.md
│   ├── code-reviewer.agent.md
│   ├── component-generator.agent.md
│   ├── devops-engineer.agent.md
│   ├── security-expert.agent.md
│   └── testing-specialist.agent.md
├── prompts/
│   ├── add-feature.prompt.md
│   ├── analyze-coverage.prompt.md
│   ├── code-review.prompt.md
│   ├── generate-api-test.prompt.md
│   ├── generate-e2e-test.prompt.md
│   ├── generate-integration-test.prompt.md
│   ├── generate-mock-data.prompt.md
│   └── generate-unit-test.prompt.md
├── instructions/
│   ├── react.instructions.md
│   ├── angular.instructions.md
│   ├── python.instructions.md
│   ├── node.instructions.md
│   ├── java.instructions.md
│   └── typescript.instructions.md
├── chatmodes/
│   └── react-expert.chatmode.md
├── collections/
│   ├── devsecops.collection.yml
│   ├── fullstack-development.collection.yml
│   └── quality-assurance.collection.yml
└── copilot-instructions.md
```

---

**Generated by CodeMate Pro** | MIT License

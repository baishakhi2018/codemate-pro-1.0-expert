# Collections Guide

Collections are bundles of related prompts, instructions, and configurations organized around specific workflows or themes.

## What are Collections?

Collections group together:
- Related prompts for specific tasks
- Coding instructions for a domain
- Recommended agents for workflows
- Best practices documentation

## Available Collections

### DevSecOps Collection
**Location:** `.github/collections/devsecops.collection.yml`

Includes resources for:
- Security-focused development
- CI/CD pipeline security
- Infrastructure security
- Code security scanning

### Quality Assurance Collection
**Location:** `.github/collections/quality-assurance.collection.yml`

Includes resources for:
- Testing strategies
- Code quality metrics
- Test automation
- Review processes

### Full-Stack Development Collection
**Location:** `.github/collections/fullstack-development.collection.yml`

Includes resources for:
- Frontend development (React, Angular)
- Backend development (Python, Node, Java)
- Database design
- API development

## Using Collections

Collections are automatically available when using AI assistants in your project. Reference them by asking about specific workflows:

```
"Help me set up a DevSecOps pipeline"
"Guide me through full-stack development best practices"
"What's the recommended QA process?"
```

## Collection Structure

Each collection file (`.collection.yml`) contains:

```yaml
name: Collection Name
description: What this collection provides
items:
  - type: prompt
    path: prompts/relevant-prompt.md
  - type: instruction
    path: instructions/relevant-instruction.md
  - type: agent
    path: agents/relevant-agent.md
tags:
  - tag1
  - tag2
```

## Creating Custom Collections

1. Create a new `.collection.yml` file in `.github/collections/`
2. Define the collection name and description
3. List related items (prompts, instructions, agents)
4. Add relevant tags

**Example:**
```yaml
name: API Development
description: Resources for building REST APIs
items:
  - type: instruction
    path: instructions/python.instructions.md
  - type: agent
    path: agents/architect.agent.md
  - type: prompt
    path: prompts/api-design.prompt.md
tags:
  - api
  - rest
  - backend
```

## Best Practices

1. **Keep collections focused** - One theme per collection
2. **Include documentation** - Explain what's included
3. **Use meaningful tags** - Help with discovery
4. **Update regularly** - Keep collections current

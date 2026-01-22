# Security Policy

## Reporting a Vulnerability

If you discover a security vulnerability, please report it responsibly.

**Do NOT open a public issue for security vulnerabilities.**

Instead:
1. Send details to the maintainer privately
2. Allow time for a fix before public disclosure
3. Provide steps to reproduce if possible

## What to Report

- Security vulnerabilities in the code
- Unsafe dependencies
- Injection vulnerabilities
- Path traversal issues
- Any issue that could be exploited

## Response Timeline

- Initial response: within 48 hours
- Status update: within 1 week
- Fix timeline: depends on severity

## Security Best Practices

When using CodeMate Pro:

1. **Review generated code** before using in production
2. **Keep dependencies updated** - run `npm audit` regularly
3. **Don't commit secrets** - use environment variables
4. **Validate inputs** - especially in generated API endpoints

## Scope

This policy applies to the CodeMate Pro repository and its official packages.

Third-party integrations and dependencies have their own security policies.

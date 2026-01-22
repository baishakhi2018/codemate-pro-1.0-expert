---
title: Code Review
description: Comprehensive code review checklist for pull requests
---

# Code Review Prompt

Please review the following code changes and provide feedback on:

## Code Quality
- [ ] Code follows project TypeScript guidelines
- [ ] No use of `any` type
- [ ] Functions are small and focused (< 50 lines)
- [ ] Meaningful variable and function names
- [ ] No commented-out code
- [ ] No console.log statements (except intentional logging)

## React Best Practices
- [ ] Using functional components
- [ ] Proper use of hooks (no violations of rules of hooks)
- [ ] Components are under 200 lines
- [ ] Props are properly typed
- [ ] No prop drilling (consider context if needed)
- [ ] Memoization used appropriately (useMemo, useCallback)

## Type Safety
- [ ] All functions have explicit return types
- [ ] Proper error handling with types
- [ ] Type guards used where appropriate
- [ ] No type assertions unless necessary

## UI/UX
- [ ] Responsive design implemented
- [ ] Proper use of Tailwind CSS utilities
- [ ] Semantic HTML elements used
- [ ] Accessible (keyboard navigation, ARIA labels)
- [ ] Loading and error states handled

## Performance
- [ ] No unnecessary re-renders
- [ ] Heavy computations are memoized
- [ ] Proper cleanup in useEffect
- [ ] No memory leaks

## Security
- [ ] User input is validated
- [ ] XSS vulnerabilities addressed
- [ ] No sensitive data in localStorage

## Testing
- [ ] Edge cases considered
- [ ] Error scenarios handled
- [ ] Manual testing completed

## Suggested Improvements
[Copilot will list specific improvements here]

## Potential Issues
[Copilot will flag any problems here]

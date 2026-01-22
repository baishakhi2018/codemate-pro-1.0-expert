---
title: Analyze Test Coverage
description: Analyze code and suggest missing tests for better coverage
---

# Analyze Test Coverage Prompt

Use this prompt to identify gaps in test coverage and get suggestions for missing tests.

## Usage

```
/analyze-coverage [file or component name]
```

## Coverage Analysis Process

### Step 1: Identify Untested Code

**Request:**
```
/analyze-coverage UserService
```

**Output:**
```
## Coverage Analysis: UserService

### Current Coverage Summary
- Statements: 65%
- Branches: 50%
- Functions: 70%
- Lines: 65%

### Untested Functions
1. `validateEmail()` - No tests found
2. `resetPassword()` - No tests found
3. `updateProfile()` - Partially tested (missing error cases)

### Untested Branches
1. Line 45: `if (user.role === 'admin')` - Admin branch not tested
2. Line 78: `catch (error)` - Error handling not tested
3. Line 92: `if (!email)` - Null check not tested

### Missing Test Scenarios
```

### Step 2: Generate Missing Tests

**Suggested Tests:**
```typescript
describe('UserService - Missing Tests', () => {
  // Untested Function: validateEmail
  describe('validateEmail', () => {
    it('should return true for valid email', () => {
      expect(userService.validateEmail('test@example.com')).toBe(true);
    });

    it('should return false for invalid email', () => {
      expect(userService.validateEmail('invalid')).toBe(false);
    });

    it('should return false for empty email', () => {
      expect(userService.validateEmail('')).toBe(false);
    });

    it('should return false for null email', () => {
      expect(userService.validateEmail(null)).toBe(false);
    });
  });

  // Untested Function: resetPassword
  describe('resetPassword', () => {
    it('should send reset email for existing user', async () => {
      const result = await userService.resetPassword('user@example.com');
      expect(result.success).toBe(true);
      expect(emailService.send).toHaveBeenCalled();
    });

    it('should throw error for non-existent user', async () => {
      await expect(userService.resetPassword('unknown@example.com'))
        .rejects.toThrow('User not found');
    });

    it('should handle email service failure', async () => {
      emailService.send.mockRejectedValue(new Error('SMTP error'));
      await expect(userService.resetPassword('user@example.com'))
        .rejects.toThrow('Failed to send reset email');
    });
  });

  // Untested Branch: Admin role check
  describe('updateProfile - Admin branch', () => {
    it('should allow admin to update any user', async () => {
      const admin = createMockUser({ role: 'admin' });
      const targetUser = createMockUser();
      
      const result = await userService.updateProfile(
        targetUser.id,
        { name: 'Updated' },
        admin
      );
      
      expect(result.name).toBe('Updated');
    });
  });

  // Untested Branch: Error handling
  describe('Error handling', () => {
    it('should handle database connection error', async () => {
      database.query.mockRejectedValue(new Error('Connection lost'));
      
      await expect(userService.getUser('123'))
        .rejects.toThrow('Database error');
    });
  });
});
```

## Coverage Report Templates

### Jest Coverage Report
```bash
npm test -- --coverage --coverageReporters=text-summary

# Output:
# =============================== Coverage summary ===============================
# Statements   : 85.5% ( 171/200 )
# Branches     : 75.2% ( 79/105 )
# Functions    : 90.0% ( 45/50 )
# Lines        : 85.5% ( 171/200 )
# ================================================================================
```

### Python Coverage Report
```bash
pytest --cov=src --cov-report=term-missing

# Output:
# Name                    Stmts   Miss  Cover   Missing
# -----------------------------------------------------
# src/services/user.py      100     15    85%   45-50, 78-82, 92
# src/services/auth.py       80      5    94%   23, 67-70
```

### Coverage Improvement Checklist

**For Each Uncovered Line:**
- [ ] Determine if line is testable
- [ ] Identify the condition/branch
- [ ] Write test that exercises the code path
- [ ] Verify coverage increased

**For Each Uncovered Function:**
- [ ] Write happy path test
- [ ] Write error case tests
- [ ] Write edge case tests
- [ ] Test all parameter combinations

**For Each Uncovered Branch:**
- [ ] Identify all conditions
- [ ] Write test for true branch
- [ ] Write test for false branch
- [ ] Test boundary conditions

## Priority Matrix

| Code Type | Priority | Target Coverage |
|-----------|----------|-----------------|
| Business Logic | High | 90%+ |
| API Endpoints | High | 85%+ |
| Utilities | Medium | 80%+ |
| UI Components | Medium | 75%+ |
| Configuration | Low | 50%+ |

## Coverage Goals by Test Type

```
┌─────────────────────────────────────────────┐
│           Coverage Pyramid                   │
├─────────────────────────────────────────────┤
│                  E2E                        │
│               (Critical                     │
│                paths)                       │
│            ┌───────────┐                    │
│            │    10%    │                    │
│         ┌──┴───────────┴──┐                 │
│         │  Integration    │                 │
│         │     20-30%      │                 │
│      ┌──┴─────────────────┴──┐              │
│      │      Unit Tests       │              │
│      │        70-80%         │              │
│      └───────────────────────┘              │
└─────────────────────────────────────────────┘
```

## Generating Coverage Improvement Plan

**Request:**
```
/analyze-coverage --plan
```

**Output:**
```markdown
## Coverage Improvement Plan

### Week 1: Critical Business Logic (Target: +10%)
- [ ] Add tests for PaymentService.processPayment()
- [ ] Add tests for OrderService.calculateTotal()
- [ ] Add error handling tests for AuthService

### Week 2: API Endpoints (Target: +8%)
- [ ] POST /api/orders endpoint tests
- [ ] PUT /api/users/:id endpoint tests
- [ ] Error response tests for all endpoints

### Week 3: Edge Cases (Target: +5%)
- [ ] Null/undefined input handling
- [ ] Empty array/object handling
- [ ] Boundary value tests

### Week 4: Integration Tests (Target: +7%)
- [ ] Database transaction tests
- [ ] Third-party API integration tests
- [ ] End-to-end user flows
```

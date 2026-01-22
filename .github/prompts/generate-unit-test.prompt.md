---
title: Generate Unit Tests
description: Generate comprehensive unit tests for components and functions
---

# Generate Unit Tests Prompt

Use this prompt to generate unit tests for your code.

## Usage

```
/generate-unit-test [component/function name]
```

## Test Generation Guidelines

### For React Components

Generate tests that cover:
1. **Rendering** - Component renders without crashing
2. **Props** - All props are handled correctly
3. **State** - State changes work as expected
4. **Events** - User interactions trigger correct handlers
5. **Edge Cases** - Empty data, null values, errors

**Example Request:**
```
/generate-unit-test UserCard component with props: name, email, avatar, onEdit, onDelete
```

**Expected Output:**
```tsx
import { render, screen, fireEvent } from '@testing-library/react';
import { UserCard } from './UserCard';

describe('UserCard', () => {
  const defaultProps = {
    name: 'John Doe',
    email: 'john@example.com',
    avatar: '/avatar.jpg',
    onEdit: jest.fn(),
    onDelete: jest.fn()
  };

  it('renders user information', () => {
    render(<UserCard {...defaultProps} />);
    expect(screen.getByText('John Doe')).toBeInTheDocument();
    expect(screen.getByText('john@example.com')).toBeInTheDocument();
  });

  it('calls onEdit when edit button clicked', () => {
    render(<UserCard {...defaultProps} />);
    fireEvent.click(screen.getByRole('button', { name: /edit/i }));
    expect(defaultProps.onEdit).toHaveBeenCalled();
  });

  it('calls onDelete when delete button clicked', () => {
    render(<UserCard {...defaultProps} />);
    fireEvent.click(screen.getByRole('button', { name: /delete/i }));
    expect(defaultProps.onDelete).toHaveBeenCalled();
  });
});
```

### For Python Functions

Generate tests that cover:
1. **Happy Path** - Normal input returns expected output
2. **Edge Cases** - Empty input, boundary values
3. **Error Cases** - Invalid input raises appropriate exceptions
4. **Types** - Input validation works correctly

**Example Request:**
```
/generate-unit-test calculate_discount function that takes price and discount_percent
```

### For Java Methods

Generate tests that cover:
1. **Normal execution** - Valid inputs produce correct results
2. **Boundary conditions** - Min/max values
3. **Exception handling** - Invalid inputs throw correct exceptions
4. **Null handling** - Null parameters handled properly

### For Node.js Functions

Generate tests that cover:
1. **Async operations** - Promises resolve/reject correctly
2. **Callbacks** - Callbacks invoked with correct arguments
3. **Error handling** - Errors propagated correctly
4. **Edge cases** - Empty arrays, null values

## Test Coverage Targets

| Type | Coverage |
|------|----------|
| Statements | 80% |
| Branches | 75% |
| Functions | 90% |
| Lines | 80% |

## Output Format

Tests should include:
- Descriptive test names
- Arrange-Act-Assert pattern
- Proper mocking of dependencies
- Comments for complex assertions

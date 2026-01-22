---
applyTo: "**/*.jsx,**/*.tsx"
---

# React Development Instructions

## Component Standards

### Functional Components Only
- Use functional components with hooks
- No class components
- Export components as named exports

### Component Structure
```tsx
// 1. Imports (grouped: React, third-party, local)
// 2. Type definitions
// 3. Component definition
// 4. Styled components (if any)
// 5. Export
```

## Hooks Best Practices

### useState
- Initialize with appropriate default values
- Use descriptive state variable names
- Group related state when possible

### useEffect
- Always specify dependency array
- Clean up side effects (return cleanup function)
- Separate concerns into multiple useEffect hooks
- Avoid infinite loops

### useCallback & useMemo
- Use for expensive computations
- Use for functions passed to child components
- Don't over-optimize - only when needed

### Custom Hooks
- Prefix with 'use' (e.g., useTasks, useAuth)
- Extract reusable logic
- Return objects or arrays, not single values
- Include proper TypeScript types

## Props & TypeScript

### Props Interface
```tsx
interface ComponentProps {
  // Required props first
  title: string;
  onSubmit: (data: FormData) => void;
  
  // Optional props with ?
  className?: string;
  disabled?: boolean;
  
  // Children if needed
  children?: React.ReactNode;
}
```

### Prop Destructuring
```tsx
export function Component({ 
  title, 
  onSubmit, 
  className = '', 
  disabled = false 
}: ComponentProps): JSX.Element {
  // Component logic
}
```

## State Management

### Local State
- Use useState for component-specific state
- Lift state up when needed by multiple components
- Keep state as close to where it's used as possible

### Context
- Use for truly global state (theme, auth, language)
- Don't overuse - not for every shared state
- Provide default values

### Complex State
```tsx
// Use useReducer for complex state logic
const [state, dispatch] = useReducer(reducer, initialState);
```

## Performance

### Avoid Unnecessary Re-renders
- Use React.memo for expensive components
- Use useCallback for functions passed as props
- Use useMemo for expensive calculations

### Lazy Loading
```tsx
const LazyComponent = React.lazy(() => import('./Component'));
```

## Event Handlers

### Naming Convention
- Prefix with 'handle' (handleClick, handleSubmit)
- Be specific about what they do

### Type Safety
```tsx
const handleClick = (event: React.MouseEvent<HTMLButtonElement>): void => {
  // Handler logic
};

const handleChange = (event: React.ChangeEvent<HTMLInputElement>): void => {
  // Handler logic
};
```

## Conditional Rendering

### Preferred Patterns
```tsx
// Ternary for if-else
{isLoading ? <Spinner /> : <Content />}

// && for if only
{error && <ErrorMessage error={error} />}

// Early return for complex conditions
if (isLoading) return <Spinner />;
if (error) return <ErrorMessage error={error} />;
return <Content />;
```

## Lists & Keys

### Always Use Keys
```tsx
{items.map((item) => (
  <Item key={item.id} item={item} />
))}
```

### Key Requirements
- Use stable, unique IDs
- Never use array index as key (unless list never changes)
- Don't generate keys randomly

## Forms

### Controlled Components
```tsx
const [value, setValue] = useState('');

<input 
  value={value} 
  onChange={(e) => setValue(e.target.value)} 
/>
```

### Form Submission
```tsx
const handleSubmit = (e: React.FormEvent<HTMLFormElement>): void => {
  e.preventDefault();
  // Handle form data
};
```

## Error Boundaries

### Use for Production
- Wrap components that might error
- Provide fallback UI
- Log errors appropriately

## Accessibility

### ARIA Labels
```tsx
<button aria-label="Close dialog" onClick={handleClose}>
  <CloseIcon />
</button>
```

### Semantic HTML
- Use proper HTML elements (button, nav, main, etc.)
- Don't use div for everything
- Use form elements appropriately

### Keyboard Navigation
- Ensure all interactive elements are keyboard accessible
- Use proper tab order
- Handle Enter and Escape keys for modals

## File Organization

### Component Files
```
components/
├── Button/
│   ├── Button.tsx
│   ├── Button.test.tsx
│   └── index.ts (re-export)
```

### Barrel Exports
```tsx
// components/index.ts
export { Button } from './Button';
export { Input } from './Input';
```

## Common Patterns

### Render Props
```tsx
<DataProvider>
  {(data) => <Display data={data} />}
</DataProvider>
```

### Compound Components
```tsx
<Card>
  <Card.Header>Title</Card.Header>
  <Card.Body>Content</Card.Body>
</Card>
```

## Avoid These

❌ Mutating state directly
❌ Using index as key
❌ Binding in render
❌ Inline function definitions in JSX (if passing to children)
❌ Missing dependency arrays
❌ Unnecessary useEffect
❌ Props drilling more than 2 levels

## Testing Checklist

- [ ] Component renders without crashing
- [ ] Props are handled correctly
- [ ] Event handlers work as expected
- [ ] Edge cases are covered
- [ ] Accessibility is maintained

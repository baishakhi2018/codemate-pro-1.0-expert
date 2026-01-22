---
applyTo: "**/*.ts,**/*.tsx"
---

# TypeScript Coding Instructions

## Type Safety
- Never use `any` type - use `unknown` if type is truly unknown
- Enable strict mode in tsconfig.json
- Use type assertions sparingly and only when necessary
- Prefer union types over optional properties when appropriate

## Interface Design
- Use `interface` for object shapes that might be extended
- Use `type` for unions, intersections, and mapped types
- Always export types that are used across multiple files
- Group related types in a single file

## Function Signatures
- Always specify return types explicitly
- Use generics for reusable type-safe functions
- Prefer readonly arrays when data shouldn't be mutated
- Use optional parameters with default values

## Best Practices
- Use enums for fixed sets of related constants
- Leverage discriminated unions for complex state
- Use utility types (Partial, Pick, Omit, etc.) effectively
- Avoid type casting with `as` unless absolutely necessary

## Example Patterns

### Component Props
```typescript
interface TaskItemProps {
  task: Task;
  onToggle: (id: string) => void;
  onDelete: (id: string) => void;
}
```

### Custom Hooks
```typescript
function useTasks(): {
  tasks: Task[];
  addTask: (title: string) => void;
  toggleTask: (id: string) => void;
  deleteTask: (id: string) => void;
} {
  // implementation
}
```

### Type Guards
```typescript
function isTask(obj: unknown): obj is Task {
  return (
    typeof obj === 'object' &&
    obj !== null &&
    'id' in obj &&
    'title' in obj &&
    'completed' in obj
  );
}
```

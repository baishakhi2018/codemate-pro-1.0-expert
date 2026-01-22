---
applyTo: "**/*.js,**/*.mjs,**/*.cjs"
---

# Node.js Development Instructions

## Code Style

### Module System
- Use CommonJS (`require`/`module.exports`) for Node.js modules
- Use ES Modules (`import`/`export`) for modern projects with `"type": "module"`
- Be consistent within a project

### Async Patterns
- Prefer `async/await` over callbacks
- Use `Promise.all()` for parallel operations
- Handle all promise rejections

### Error Handling
```javascript
// Good - specific error handling
try {
  const result = await operation();
} catch (error) {
  if (error.code === 'ENOENT') {
    // Handle file not found
  } else {
    throw error;
  }
}
```

## Express.js Guidelines

### Route Organization
- Group related routes in separate files
- Use Router for modular routing
- Apply middleware at appropriate levels

### Middleware Pattern
```javascript
const middleware = async (req, res, next) => {
  try {
    // Logic here
    next();
  } catch (error) {
    next(error);
  }
};
```

### Error Middleware
```javascript
const errorHandler = (err, req, res, next) => {
  console.error(err.stack);
  res.status(err.status || 500).json({
    error: err.message
  });
};
```

## File Structure

```
src/
├── routes/       # Route definitions
├── controllers/  # Request handlers
├── services/     # Business logic
├── middleware/   # Express middleware
├── models/       # Data models
├── utils/        # Utility functions
└── config/       # Configuration
```

## Best Practices

1. **Environment Variables** - Use `dotenv` for configuration
2. **Logging** - Use structured logging (winston, pino)
3. **Validation** - Validate all inputs (joi, express-validator)
4. **Security** - Use helmet, rate limiting, CORS
5. **Testing** - Use Jest or Mocha for testing

## Documentation

Use JSDoc for all exported functions:

```javascript
/**
 * Process user data
 * @param {Object} user - User object
 * @param {string} user.id - User ID
 * @param {string} user.name - User name
 * @returns {Promise<Object>} Processed user data
 * @throws {Error} If user is invalid
 */
async function processUser(user) {
  // Implementation
}
```

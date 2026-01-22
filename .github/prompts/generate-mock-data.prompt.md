---
title: Generate Mock Data & Fixtures
description: Generate test fixtures, mock data, and factory functions
---

# Generate Mock Data & Fixtures Prompt

Use this prompt to generate test data, fixtures, and factory functions.

## Usage

```
/generate-mock-data [entity name] [fields]
/generate-fixture [entity name]
/generate-factory [entity name]
```

## Mock Data Generators

### React/TypeScript Factory

**Request:**
```
/generate-factory User with id, name, email, role, createdAt
```

**Output:**
```typescript
// factories/userFactory.ts
import { faker } from '@faker-js/faker';

export interface User {
  id: string;
  name: string;
  email: string;
  role: 'admin' | 'user' | 'guest';
  createdAt: Date;
}

export const createMockUser = (overrides: Partial<User> = {}): User => ({
  id: faker.string.uuid(),
  name: faker.person.fullName(),
  email: faker.internet.email(),
  role: faker.helpers.arrayElement(['admin', 'user', 'guest']),
  createdAt: faker.date.past(),
  ...overrides
});

export const createMockUsers = (count: number, overrides: Partial<User> = {}): User[] => 
  Array.from({ length: count }, () => createMockUser(overrides));

// Builder pattern
export class UserBuilder {
  private user: Partial<User> = {};

  withId(id: string): this {
    this.user.id = id;
    return this;
  }

  withName(name: string): this {
    this.user.name = name;
    return this;
  }

  withEmail(email: string): this {
    this.user.email = email;
    return this;
  }

  asAdmin(): this {
    this.user.role = 'admin';
    return this;
  }

  asGuest(): this {
    this.user.role = 'guest';
    return this;
  }

  build(): User {
    return createMockUser(this.user);
  }
}

// Usage examples:
// const user = createMockUser();
// const admin = createMockUser({ role: 'admin' });
// const users = createMockUsers(10);
// const customUser = new UserBuilder().withName('John').asAdmin().build();
```

### Python Factory (Factory Boy)

**Output:**
```python
# factories/user_factory.py
import factory
from factory import fuzzy
from datetime import datetime, timedelta
from models import User

class UserFactory(factory.Factory):
    class Meta:
        model = User

    id = factory.Faker('uuid4')
    name = factory.Faker('name')
    email = factory.Faker('email')
    role = fuzzy.FuzzyChoice(['admin', 'user', 'guest'])
    created_at = factory.LazyFunction(datetime.now)
    is_active = True

    class Params:
        admin = factory.Trait(
            role='admin',
            is_active=True
        )
        inactive = factory.Trait(
            is_active=False
        )

# Usage:
# user = UserFactory()
# admin = UserFactory(admin=True)
# inactive_user = UserFactory(inactive=True)
# users = UserFactory.create_batch(10)
```

### Java Factory

**Output:**
```java
// factories/UserFactory.java
public class UserFactory {
    private static final Faker faker = new Faker();
    
    public static User createUser() {
        return User.builder()
            .id(UUID.randomUUID().toString())
            .name(faker.name().fullName())
            .email(faker.internet().emailAddress())
            .role(faker.options().option("admin", "user", "guest"))
            .createdAt(LocalDateTime.now())
            .build();
    }
    
    public static User createAdmin() {
        return User.builder()
            .id(UUID.randomUUID().toString())
            .name(faker.name().fullName())
            .email(faker.internet().emailAddress())
            .role("admin")
            .createdAt(LocalDateTime.now())
            .build();
    }
    
    public static List<User> createUsers(int count) {
        return IntStream.range(0, count)
            .mapToObj(i -> createUser())
            .collect(Collectors.toList());
    }
}
```

### Node.js Factory

**Output:**
```javascript
// factories/userFactory.js
const { faker } = require('@faker-js/faker');

const createUser = (overrides = {}) => ({
  id: faker.string.uuid(),
  name: faker.person.fullName(),
  email: faker.internet.email(),
  role: faker.helpers.arrayElement(['admin', 'user', 'guest']),
  createdAt: faker.date.past(),
  ...overrides
});

const createUsers = (count, overrides = {}) => 
  Array.from({ length: count }, () => createUser(overrides));

const createAdmin = (overrides = {}) => 
  createUser({ role: 'admin', ...overrides });

module.exports = { createUser, createUsers, createAdmin };
```

## Test Fixtures

### JSON Fixtures

**Request:**
```
/generate-fixture products for e-commerce
```

**Output:**
```json
// fixtures/products.json
{
  "products": [
    {
      "id": "prod-001",
      "name": "Wireless Headphones",
      "price": 99.99,
      "category": "electronics",
      "stock": 50,
      "rating": 4.5,
      "tags": ["audio", "wireless", "bluetooth"]
    },
    {
      "id": "prod-002",
      "name": "Running Shoes",
      "price": 129.99,
      "category": "sports",
      "stock": 100,
      "rating": 4.8,
      "tags": ["running", "fitness", "outdoor"]
    },
    {
      "id": "prod-003",
      "name": "Coffee Maker",
      "price": 79.99,
      "category": "home",
      "stock": 30,
      "rating": 4.2,
      "tags": ["kitchen", "appliance", "coffee"]
    }
  ]
}
```

### Fixture Loader

```typescript
// utils/fixtures.ts
import productsFixture from '../fixtures/products.json';
import usersFixture from '../fixtures/users.json';

export const fixtures = {
  products: productsFixture.products,
  users: usersFixture.users,
};

export const loadFixture = <T>(name: keyof typeof fixtures): T[] => {
  return fixtures[name] as T[];
};

export const findFixture = <T extends { id: string }>(
  name: keyof typeof fixtures,
  id: string
): T | undefined => {
  return (fixtures[name] as T[]).find(item => item.id === id);
};
```

## API Mock Responses

```typescript
// mocks/api/users.ts
export const mockUserResponses = {
  getUsers: {
    success: {
      status: 200,
      data: {
        users: createMockUsers(5),
        pagination: { page: 1, limit: 10, total: 5 }
      }
    },
    empty: {
      status: 200,
      data: { users: [], pagination: { page: 1, limit: 10, total: 0 } }
    },
    unauthorized: {
      status: 401,
      error: { message: 'Unauthorized' }
    }
  },
  
  createUser: {
    success: {
      status: 201,
      data: createMockUser()
    },
    validationError: {
      status: 400,
      error: {
        message: 'Validation failed',
        errors: [{ field: 'email', message: 'Invalid email format' }]
      }
    },
    conflict: {
      status: 409,
      error: { message: 'Email already exists' }
    }
  }
};
```

## MSW Handlers (Mock Service Worker)

```typescript
// mocks/handlers.ts
import { rest } from 'msw';
import { createMockUser, createMockUsers } from '../factories/userFactory';

export const handlers = [
  rest.get('/api/users', (req, res, ctx) => {
    return res(
      ctx.status(200),
      ctx.json({
        data: createMockUsers(10),
        pagination: { page: 1, limit: 10, total: 100 }
      })
    );
  }),

  rest.get('/api/users/:id', (req, res, ctx) => {
    const { id } = req.params;
    return res(
      ctx.status(200),
      ctx.json(createMockUser({ id: id as string }))
    );
  }),

  rest.post('/api/users', async (req, res, ctx) => {
    const body = await req.json();
    return res(
      ctx.status(201),
      ctx.json(createMockUser(body))
    );
  }),

  rest.delete('/api/users/:id', (req, res, ctx) => {
    return res(ctx.status(204));
  })
];
```

## Mock Data Checklist

- [ ] Cover all entity types
- [ ] Include edge cases (empty, null, max values)
- [ ] Use realistic data
- [ ] Support customization/overrides
- [ ] Include relationships between entities
- [ ] Add helper functions for common scenarios

---
title: Generate API Tests
description: Generate comprehensive API tests for REST and GraphQL endpoints
---

# Generate API Tests Prompt

Use this prompt to generate API tests for your backend endpoints.

## Usage

```
/generate-api-test [HTTP method] [endpoint] [description]
```

## REST API Tests

### GET Endpoint

**Request:**
```
/generate-api-test GET /api/users list all users with pagination
```

**Output:**
```javascript
describe('GET /api/users', () => {
  describe('Success Cases', () => {
    it('should return paginated users list', async () => {
      const response = await request(app)
        .get('/api/users')
        .query({ page: 1, limit: 10 })
        .expect(200);

      expect(response.body).toHaveProperty('data');
      expect(response.body).toHaveProperty('pagination');
      expect(response.body.data).toBeInstanceOf(Array);
      expect(response.body.pagination).toMatchObject({
        page: 1,
        limit: 10,
        total: expect.any(Number)
      });
    });

    it('should filter users by status', async () => {
      const response = await request(app)
        .get('/api/users')
        .query({ status: 'active' })
        .expect(200);

      response.body.data.forEach(user => {
        expect(user.status).toBe('active');
      });
    });

    it('should search users by name', async () => {
      const response = await request(app)
        .get('/api/users')
        .query({ search: 'john' })
        .expect(200);

      response.body.data.forEach(user => {
        expect(user.name.toLowerCase()).toContain('john');
      });
    });
  });

  describe('Error Cases', () => {
    it('should return 401 without authentication', async () => {
      const response = await request(app)
        .get('/api/users')
        .expect(401);

      expect(response.body.error).toBe('Unauthorized');
    });

    it('should return 400 for invalid pagination', async () => {
      const response = await request(app)
        .get('/api/users')
        .set('Authorization', `Bearer ${token}`)
        .query({ page: -1 })
        .expect(400);

      expect(response.body.error).toContain('page');
    });
  });
});
```

### POST Endpoint

**Request:**
```
/generate-api-test POST /api/users create new user
```

**Output:**
```javascript
describe('POST /api/users', () => {
  const validUser = {
    name: 'John Doe',
    email: 'john@example.com',
    password: 'SecurePass123!',
    role: 'user'
  };

  describe('Success Cases', () => {
    it('should create user with valid data', async () => {
      const response = await request(app)
        .post('/api/users')
        .set('Authorization', `Bearer ${adminToken}`)
        .send(validUser)
        .expect(201);

      expect(response.body).toMatchObject({
        id: expect.any(String),
        name: validUser.name,
        email: validUser.email,
        role: validUser.role
      });
      expect(response.body).not.toHaveProperty('password');
    });

    it('should hash password before storing', async () => {
      await request(app)
        .post('/api/users')
        .set('Authorization', `Bearer ${adminToken}`)
        .send(validUser);

      const user = await User.findOne({ email: validUser.email });
      expect(user.password).not.toBe(validUser.password);
    });
  });

  describe('Validation Errors', () => {
    it('should reject invalid email format', async () => {
      const response = await request(app)
        .post('/api/users')
        .set('Authorization', `Bearer ${adminToken}`)
        .send({ ...validUser, email: 'invalid-email' })
        .expect(400);

      expect(response.body.errors).toContainEqual(
        expect.objectContaining({ field: 'email' })
      );
    });

    it('should reject weak password', async () => {
      const response = await request(app)
        .post('/api/users')
        .set('Authorization', `Bearer ${adminToken}`)
        .send({ ...validUser, password: '123' })
        .expect(400);

      expect(response.body.errors).toContainEqual(
        expect.objectContaining({ field: 'password' })
      );
    });

    it('should reject missing required fields', async () => {
      const response = await request(app)
        .post('/api/users')
        .set('Authorization', `Bearer ${adminToken}`)
        .send({})
        .expect(400);

      expect(response.body.errors.length).toBeGreaterThan(0);
    });
  });

  describe('Business Rules', () => {
    it('should reject duplicate email', async () => {
      await request(app)
        .post('/api/users')
        .set('Authorization', `Bearer ${adminToken}`)
        .send(validUser);

      const response = await request(app)
        .post('/api/users')
        .set('Authorization', `Bearer ${adminToken}`)
        .send(validUser)
        .expect(409);

      expect(response.body.error).toContain('already exists');
    });
  });
});
```

### PUT/PATCH Endpoint

**Output:**
```javascript
describe('PATCH /api/users/:id', () => {
  let userId;

  beforeEach(async () => {
    const user = await createTestUser();
    userId = user.id;
  });

  it('should update user partially', async () => {
    const response = await request(app)
      .patch(`/api/users/${userId}`)
      .set('Authorization', `Bearer ${token}`)
      .send({ name: 'Updated Name' })
      .expect(200);

    expect(response.body.name).toBe('Updated Name');
  });

  it('should return 404 for non-existent user', async () => {
    await request(app)
      .patch('/api/users/non-existent-id')
      .set('Authorization', `Bearer ${token}`)
      .send({ name: 'Test' })
      .expect(404);
  });

  it('should return 403 when updating other user', async () => {
    await request(app)
      .patch(`/api/users/${userId}`)
      .set('Authorization', `Bearer ${otherUserToken}`)
      .send({ name: 'Hacked' })
      .expect(403);
  });
});
```

### DELETE Endpoint

**Output:**
```javascript
describe('DELETE /api/users/:id', () => {
  it('should delete user successfully', async () => {
    const user = await createTestUser();

    await request(app)
      .delete(`/api/users/${user.id}`)
      .set('Authorization', `Bearer ${adminToken}`)
      .expect(204);

    const deletedUser = await User.findById(user.id);
    expect(deletedUser).toBeNull();
  });

  it('should return 404 for non-existent user', async () => {
    await request(app)
      .delete('/api/users/non-existent-id')
      .set('Authorization', `Bearer ${adminToken}`)
      .expect(404);
  });

  it('should return 403 for non-admin users', async () => {
    const user = await createTestUser();

    await request(app)
      .delete(`/api/users/${user.id}`)
      .set('Authorization', `Bearer ${userToken}`)
      .expect(403);
  });
});
```

## GraphQL API Tests

```javascript
describe('GraphQL User Queries', () => {
  it('should fetch user by ID', async () => {
    const query = `
      query GetUser($id: ID!) {
        user(id: $id) {
          id
          name
          email
        }
      }
    `;

    const response = await request(app)
      .post('/graphql')
      .send({ query, variables: { id: userId } })
      .expect(200);

    expect(response.body.data.user).toMatchObject({
      id: userId,
      name: expect.any(String),
      email: expect.any(String)
    });
  });

  it('should create user mutation', async () => {
    const mutation = `
      mutation CreateUser($input: CreateUserInput!) {
        createUser(input: $input) {
          id
          name
          email
        }
      }
    `;

    const response = await request(app)
      .post('/graphql')
      .send({
        query: mutation,
        variables: {
          input: { name: 'Test', email: 'test@example.com', password: 'pass123' }
        }
      })
      .expect(200);

    expect(response.body.data.createUser.id).toBeDefined();
  });
});
```

## API Test Checklist

- [ ] Test all HTTP methods (GET, POST, PUT, PATCH, DELETE)
- [ ] Test authentication and authorization
- [ ] Test input validation
- [ ] Test pagination and filtering
- [ ] Test error responses
- [ ] Test rate limiting
- [ ] Test CORS headers
- [ ] Test response formats

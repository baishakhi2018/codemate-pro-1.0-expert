---
title: Generate Integration Tests
description: Generate integration tests for API endpoints and service interactions
---

# Generate Integration Tests Prompt

Use this prompt to generate integration tests that verify component interactions.

## Usage

```
/generate-integration-test [endpoint/service name]
```

## Integration Test Guidelines

### For REST APIs

**Example Request:**
```
/generate-integration-test POST /api/users endpoint
```

**Expected Output (Node.js/Express):**
```javascript
const request = require('supertest');
const app = require('../app');
const db = require('../db');

describe('POST /api/users', () => {
  beforeAll(async () => {
    await db.connect();
  });

  afterAll(async () => {
    await db.disconnect();
  });

  beforeEach(async () => {
    await db.clear('users');
  });

  it('should create a new user with valid data', async () => {
    const userData = {
      name: 'John Doe',
      email: 'john@example.com',
      password: 'securePass123'
    };

    const response = await request(app)
      .post('/api/users')
      .send(userData)
      .expect(201);

    expect(response.body).toMatchObject({
      id: expect.any(String),
      name: 'John Doe',
      email: 'john@example.com'
    });
    expect(response.body.password).toBeUndefined();
  });

  it('should return 400 for invalid email', async () => {
    const response = await request(app)
      .post('/api/users')
      .send({ name: 'John', email: 'invalid', password: 'pass123' })
      .expect(400);

    expect(response.body.error).toContain('email');
  });

  it('should return 409 for duplicate email', async () => {
    const userData = { name: 'John', email: 'john@example.com', password: 'pass123' };
    
    await request(app).post('/api/users').send(userData);
    
    const response = await request(app)
      .post('/api/users')
      .send(userData)
      .expect(409);

    expect(response.body.error).toContain('already exists');
  });
});
```

### For Python FastAPI

**Example Output:**
```python
import pytest
from httpx import AsyncClient
from app.main import app
from app.database import get_db, Base, engine

@pytest.fixture(autouse=True)
async def setup_db():
    Base.metadata.create_all(bind=engine)
    yield
    Base.metadata.drop_all(bind=engine)

@pytest.mark.asyncio
async def test_create_user_success():
    async with AsyncClient(app=app, base_url="http://test") as client:
        response = await client.post("/api/users", json={
            "name": "John Doe",
            "email": "john@example.com",
            "password": "securePass123"
        })
    
    assert response.status_code == 201
    data = response.json()
    assert data["name"] == "John Doe"
    assert data["email"] == "john@example.com"
    assert "password" not in data

@pytest.mark.asyncio
async def test_create_user_invalid_email():
    async with AsyncClient(app=app, base_url="http://test") as client:
        response = await client.post("/api/users", json={
            "name": "John",
            "email": "invalid-email",
            "password": "pass123"
        })
    
    assert response.status_code == 422

@pytest.mark.asyncio
async def test_create_user_duplicate_email():
    user_data = {"name": "John", "email": "john@example.com", "password": "pass123"}
    
    async with AsyncClient(app=app, base_url="http://test") as client:
        await client.post("/api/users", json=user_data)
        response = await client.post("/api/users", json=user_data)
    
    assert response.status_code == 409
```

### For Java Spring Boot

**Example Output:**
```java
@SpringBootTest(webEnvironment = WebEnvironment.RANDOM_PORT)
@AutoConfigureMockMvc
class UserControllerIntegrationTest {

    @Autowired
    private MockMvc mockMvc;

    @Autowired
    private UserRepository userRepository;

    @BeforeEach
    void setUp() {
        userRepository.deleteAll();
    }

    @Test
    void createUser_WithValidData_ReturnsCreated() throws Exception {
        String userJson = """
            {
                "name": "John Doe",
                "email": "john@example.com",
                "password": "securePass123"
            }
            """;

        mockMvc.perform(post("/api/users")
                .contentType(MediaType.APPLICATION_JSON)
                .content(userJson))
            .andExpect(status().isCreated())
            .andExpect(jsonPath("$.name").value("John Doe"))
            .andExpect(jsonPath("$.email").value("john@example.com"))
            .andExpect(jsonPath("$.password").doesNotExist());
    }

    @Test
    void createUser_WithInvalidEmail_ReturnsBadRequest() throws Exception {
        String userJson = """
            {"name": "John", "email": "invalid", "password": "pass123"}
            """;

        mockMvc.perform(post("/api/users")
                .contentType(MediaType.APPLICATION_JSON)
                .content(userJson))
            .andExpect(status().isBadRequest());
    }
}
```

## Test Database Setup

### MongoDB (Node.js)
```javascript
const { MongoMemoryServer } = require('mongodb-memory-server');

let mongoServer;

beforeAll(async () => {
  mongoServer = await MongoMemoryServer.create();
  await mongoose.connect(mongoServer.getUri());
});

afterAll(async () => {
  await mongoose.disconnect();
  await mongoServer.stop();
});
```

### PostgreSQL (Python)
```python
@pytest.fixture(scope="session")
def database():
    # Use testcontainers for real PostgreSQL
    with PostgresContainer("postgres:15") as postgres:
        yield postgres.get_connection_url()
```

## What Integration Tests Should Cover

- [ ] API endpoint responses (status codes, body)
- [ ] Database operations (CRUD)
- [ ] Authentication/Authorization
- [ ] Error handling
- [ ] Data validation
- [ ] Service-to-service communication

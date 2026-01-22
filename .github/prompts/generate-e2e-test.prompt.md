---
title: Generate E2E Tests
description: Generate end-to-end tests for user workflows and critical paths
---

# Generate E2E Tests Prompt

Use this prompt to generate end-to-end tests that simulate real user interactions.

## Usage

```
/generate-e2e-test [user workflow description]
```

## E2E Test Guidelines

### For React Apps (Cypress)

**Example Request:**
```
/generate-e2e-test user login flow with email and password
```

**Expected Output:**
```javascript
// cypress/e2e/auth/login.cy.js

describe('User Login Flow', () => {
  beforeEach(() => {
    cy.visit('/login');
  });

  it('should login successfully with valid credentials', () => {
    // Fill in login form
    cy.get('[data-testid="email-input"]').type('user@example.com');
    cy.get('[data-testid="password-input"]').type('correctPassword123');
    cy.get('[data-testid="login-button"]').click();

    // Verify successful login
    cy.url().should('include', '/dashboard');
    cy.get('[data-testid="welcome-message"]').should('contain', 'Welcome');
    cy.get('[data-testid="user-menu"]').should('be.visible');
  });

  it('should show error for invalid credentials', () => {
    cy.get('[data-testid="email-input"]').type('user@example.com');
    cy.get('[data-testid="password-input"]').type('wrongPassword');
    cy.get('[data-testid="login-button"]').click();

    cy.get('[data-testid="error-message"]')
      .should('be.visible')
      .and('contain', 'Invalid credentials');
    cy.url().should('include', '/login');
  });

  it('should show validation errors for empty fields', () => {
    cy.get('[data-testid="login-button"]').click();

    cy.get('[data-testid="email-error"]').should('contain', 'Email is required');
    cy.get('[data-testid="password-error"]').should('contain', 'Password is required');
  });

  it('should navigate to forgot password page', () => {
    cy.get('[data-testid="forgot-password-link"]').click();
    cy.url().should('include', '/forgot-password');
  });

  it('should navigate to register page', () => {
    cy.get('[data-testid="register-link"]').click();
    cy.url().should('include', '/register');
  });
});
```

### For React Apps (Playwright)

**Example Output:**
```typescript
// tests/auth/login.spec.ts
import { test, expect } from '@playwright/test';

test.describe('User Login Flow', () => {
  test.beforeEach(async ({ page }) => {
    await page.goto('/login');
  });

  test('should login successfully with valid credentials', async ({ page }) => {
    await page.fill('[data-testid="email-input"]', 'user@example.com');
    await page.fill('[data-testid="password-input"]', 'correctPassword123');
    await page.click('[data-testid="login-button"]');

    await expect(page).toHaveURL(/.*dashboard/);
    await expect(page.locator('[data-testid="welcome-message"]')).toContainText('Welcome');
  });

  test('should show error for invalid credentials', async ({ page }) => {
    await page.fill('[data-testid="email-input"]', 'user@example.com');
    await page.fill('[data-testid="password-input"]', 'wrongPassword');
    await page.click('[data-testid="login-button"]');

    await expect(page.locator('[data-testid="error-message"]')).toBeVisible();
    await expect(page).toHaveURL(/.*login/);
  });

  test('should persist session after page refresh', async ({ page }) => {
    // Login first
    await page.fill('[data-testid="email-input"]', 'user@example.com');
    await page.fill('[data-testid="password-input"]', 'correctPassword123');
    await page.click('[data-testid="login-button"]');
    await expect(page).toHaveURL(/.*dashboard/);

    // Refresh and verify still logged in
    await page.reload();
    await expect(page.locator('[data-testid="user-menu"]')).toBeVisible();
  });
});
```

### For Angular Apps (Protractor/Cypress)

**Example Output:**
```typescript
// e2e/src/login.e2e-spec.ts
describe('Login Page', () => {
  beforeEach(() => {
    cy.visit('/login');
  });

  it('should display login form', () => {
    cy.get('app-login').should('exist');
    cy.get('input[formControlName="email"]').should('be.visible');
    cy.get('input[formControlName="password"]').should('be.visible');
    cy.get('button[type="submit"]').should('be.visible');
  });

  it('should login and redirect to dashboard', () => {
    cy.get('input[formControlName="email"]').type('test@example.com');
    cy.get('input[formControlName="password"]').type('password123');
    cy.get('button[type="submit"]').click();

    cy.url().should('include', '/dashboard');
    cy.get('app-dashboard').should('exist');
  });
});
```

## Common E2E Test Scenarios

### Shopping Cart Flow
```javascript
describe('Shopping Cart', () => {
  it('should complete purchase flow', () => {
    // Browse products
    cy.visit('/products');
    cy.get('[data-testid="product-card"]').first().click();
    
    // Add to cart
    cy.get('[data-testid="add-to-cart"]').click();
    cy.get('[data-testid="cart-count"]').should('contain', '1');
    
    // Go to checkout
    cy.get('[data-testid="cart-icon"]').click();
    cy.get('[data-testid="checkout-button"]').click();
    
    // Fill shipping info
    cy.get('[data-testid="shipping-form"]').within(() => {
      cy.get('input[name="address"]').type('123 Main St');
      cy.get('input[name="city"]').type('New York');
      cy.get('input[name="zip"]').type('10001');
    });
    
    // Complete order
    cy.get('[data-testid="place-order"]').click();
    cy.get('[data-testid="order-confirmation"]').should('be.visible');
  });
});
```

### Form Submission Flow
```javascript
describe('Contact Form', () => {
  it('should submit form successfully', () => {
    cy.visit('/contact');
    
    cy.get('input[name="name"]').type('John Doe');
    cy.get('input[name="email"]').type('john@example.com');
    cy.get('textarea[name="message"]').type('Hello, this is a test message.');
    cy.get('button[type="submit"]').click();
    
    cy.get('[data-testid="success-toast"]')
      .should('be.visible')
      .and('contain', 'Message sent successfully');
  });
});
```

## E2E Test Best Practices

- [ ] Use data-testid attributes for selectors
- [ ] Reset database state before each test
- [ ] Test critical user journeys
- [ ] Keep tests independent
- [ ] Use page objects for reusability
- [ ] Test both happy and unhappy paths
- [ ] Include accessibility checks
- [ ] Test responsive behavior

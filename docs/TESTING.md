# Testing Guide

## 1. Testing Strategy

We use a multi-layered testing approach to ensure code quality and reliability:

### 1.1 Testing Layers

| Layer | Tool | Purpose |
|-------|------|---------|
| **Unit Tests** | Jest + React Testing Library | Test individual functions and components |
| **Integration Tests** | Supertest + tRPC | Test API endpoints and database interactions |
| **End-to-End Tests** | Playwright | Test user workflows and UI interactions |
| **Performance Tests** | k6 | Test application performance under load |

### 1.2 Test Coverage

We aim for:
- **Unit Tests:** 80%+ coverage
- **Integration Tests:** 70%+ coverage
- **End-to-End Tests:** 50%+ coverage

## 2. Setting Up Testing

### 2.1 Install Dependencies

```bash
pnpm install
```

### 2.2 Install Testing Tools

```bash
pnpm add -D jest @testing-library/react @testing-library/jest-dom supertest @types/supertest playwright @playwright/test
```

### 2.3 Configure Jest

Create `jest.config.js`:

```javascript
module.exports = {
  testEnvironment: 'jsdom',
  setupFilesAfterEnv: ['<rootDir>/jest.setup.js'],
  moduleNameMapping: {
    '^@/(.*)$': '<rootDir>/src/$1',
  },
  testMatch: [
    '<rootDir>/src/**/__tests__/**/*.{js,jsx,ts,tsx}',
    '<rootDir>/src/**/*.{test,spec}.{js,jsx,ts,tsx}',
  ],
  collectCoverageFrom: [
    'src/**/*.{js,jsx,ts,tsx}',
    '!src/**/*.d.ts',
    '!src/**/*.stories.{js,jsx,ts,tsx}',
  ],
};
```

### 2.4 Setup Jest Environment

Create `jest.setup.js`:

```javascript
import '@testing-library/jest-dom';
import { server } from './src/server/mocks/server.js';

// Establish API mocking before all tests
beforeAll(() => server.listen());

// Reset any request handlers that we may add during the tests,
// so they don't interfere with tests.
afterEach(() => server.resetHandlers());

// Clean up after the all tests are done, including the mocking.
afterAll(() => server.close());
```

## 3. Unit Testing

### 3.1 Testing Functions

Example: Testing a utility function

```javascript
// src/lib/utils.js
export function formatDate(date) {
  return new Date(date).toLocaleDateString();
}

// tests/lib/utils.test.js
import { formatDate } from '@/lib/utils';

describe('formatDate', () => {
  it('should format date correctly', () => {
    const date = '2026-12-01';
    const formatted = formatDate(date);
    expect(formatted).toBe('12/1/2026');
  });
});
```

### 3.2 Testing Components

Example: Testing a React component

```javascript
// src/components/ui/Button.jsx
import React from 'react';

export function Button({ children, onClick }) {
  return (
    <button onClick={onClick} className="btn">
      {children}
    </button>
  );
}

// tests/components/ui/Button.test.jsx
import { render, screen } from '@testing-library/react';
import { Button } from '@/components/ui/Button';

describe('Button', () => {
  it('should render children', () => {
    render(<Button>Click me</Button>);
    expect(screen.getByText('Click me')).toBeInTheDocument();
  });

  it('should call onClick when clicked', () => {
    const handleClick = jest.fn();
    render(<Button onClick={handleClick}>Click me</Button>);
    screen.getByText('Click me').click();
    expect(handleClick).toHaveBeenCalledTimes(1);
  });
});
```

### 3.3 Testing Hooks

Example: Testing a custom hook

```javascript
// src/hooks/useAuth.js
import { useState, useEffect } from 'react';

export function useAuth() {
  const [user, setUser] = useState(null);

  useEffect(() => {
    const token = localStorage.getItem('token');
    if (token) {
      // Fetch user from API
      setUser({ id: 'user123', name: 'John Doe' });
    }
  }, []);

  return { user };
}

// tests/hooks/useAuth.test.js
import { renderHook, act } from '@testing-library/react';
import { useAuth } from '@/hooks/useAuth';

describe('useAuth', () => {
  it('should initialize with no user', () => {
    const { result } = renderHook(() => useAuth());
    expect(result.current.user).toBeNull();
  });

  it('should set user when token exists', () => {
    localStorage.setItem('token', 'test-token');
    const { result } = renderHook(() => useAuth());
    expect(result.current.user).toEqual({ id: 'user123', name: 'John Doe' });
  });
});
```

## 4. Integration Testing

### 4.1 Testing API Endpoints

Example: Testing a tRPC endpoint

```javascript
// src/server/routers/tournamentRouter.js
import { z } from 'zod';
import { publicProcedure, router } from '../trpc.js';

export const tournamentRouter = router({
  create: publicProcedure
    .input(z.object({
      name: z.string(),
      game: z.string(),
    }))
    .mutation(async ({ input, ctx }) => {
      const tournament = await ctx.db.tournament.create({
        data: input,
      });
      return tournament;
    }),
});

// tests/api/tournament.test.js
import { createTRPCContext } from '@/server/trpc.js';
import { tournamentRouter } from '@/server/routers/tournamentRouter';
import { initTRPC } from '@trpc/server';

describe('Tournament API', () => {
  it('should create tournament', async () => {
    const t = initTRPC.create();
    const app = t.router({
      tournament: tournamentRouter,
    });

    const ctx = await createTRPCContext();
    const caller = app.createCaller(ctx);

    const tournament = await caller.tournament.create({
      name: 'Test Tournament',
      game: 'CS2',
    });

    expect(tournament.name).toBe('Test Tournament');
    expect(tournament.game).toBe('CS2');
  });
});
```

### 4.2 Testing Database Operations

Example: Testing database interactions

```javascript
// tests/database/tournament.test.js
import { db } from '@/lib/supabase.js';

describe('Tournament Database', () => {
  it('should create tournament', async () => {
    const tournament = await db.tournament.create({
      data: {
        name: 'Test Tournament',
        game: 'CS2',
      },
    });

    expect(tournament.name).toBe('Test Tournament');
    expect(tournament.game).toBe('CS2');
  });

  it('should find tournament by name', async () => {
    const tournament = await db.tournament.findUnique({
      where: { name: 'Test Tournament' },
    });

    expect(tournament).not.toBeNull();
    expect(tournament.name).toBe('Test Tournament');
  });
});
```

## 5. End-to-End Testing

### 5.1 Setting Up Playwright

Create `playwright.config.js`:

```javascript
import { defineConfig } from '@playwright/test';

export default defineConfig({
  testDir: './tests/e2e',
  fullyParallel: true,
  forbidOnly: !!process.env.CI,
  retries: process.env.CI ? 2 : 0,
  workers: process.env.CI ? 1 : undefined,
  reporter: 'html',
  use: {
    baseURL: 'http://localhost:3000',
    trace: 'on-first-retry',
  },
});
```

### 5.2 Testing User Workflows

Example: Testing tournament creation

```javascript
// tests/e2e/tournament.spec.js
import { test, expect } from '@playwright/test';

test.describe('Tournament Creation', () => {
  test('should create tournament', async ({ page }) => {
    // Login
    await page.goto('/login');
    await page.fill('[data-testid="email"]', 'user@example.com');
    await page.fill('[data-testid="password"]', 'password');
    await page.click('[data-testid="login-button"]');

    // Navigate to tournaments
    await page.goto('/tournaments');
    await page.click('[data-testid="create-tournament"]');

    // Fill form
    await page.fill('[data-testid="tournament-name"]', 'Test Tournament');
    await page.fill('[data-testid="tournament-game"]', 'CS2');
    await page.fill('[data-testid="tournament-entry-fee"]', '10');
    await page.click('[data-testid="submit-tournament"]');

    // Verify tournament was created
    await expect(page.locator('[data-testid="tournament-card"]')).toBeVisible();
    await expect(page.locator('text=Test Tournament')).toBeVisible();
  });
});
```

### 5.3 Testing API Endpoints

Example: Testing API with Playwright

```javascript
// tests/e2e/api.spec.js
import { test, expect } from '@playwright/test';

test.describe('API Endpoints', () => {
  test('should create tournament via API', async ({ request }) => {
    const response = await request.post('/api/tournaments', {
      data: {
        name: 'API Test Tournament',
        game: 'Valorant',
        entryFee: 15,
      },
    });

    expect(response.status()).toBe(201);
    const tournament = await response.json();
    expect(tournament.name).toBe('API Test Tournament');
  });
});
```

## 6. Performance Testing

### 6.1 Setting Up k6

Create `load-test.js`:

```javascript
import http from 'k6/http';
import { check, sleep } from 'k6';

export let options = {
  stages: [
    { duration: '30s', target: 10 },
    { duration: '1m', target: 50 },
    { duration: '30s', target: 100 },
    { duration: '30s', target: 0 },
  ],
};

export default function () {
  const response = http.get('http://localhost:3000/api/tournaments');
  check(response, {
    'status was 200': (r) => r.status == 200,
    'response time < 500ms': (r) => r.timings.duration < 500,
  });
  sleep(1);
}
```

### 6.2 Running Performance Tests

```bash
k6 run load-test.js
```

## 7. Test Scripts

Add these scripts to `package.json`:

```json
{
  "scripts": {
    "test": "jest",
    "test:watch": "jest --watch",
    "test:coverage": "jest --coverage",
    "test:e2e": "playwright test",
    "test:performance": "k6 run load-test.js",
    "test:ci": "jest --ci --coverage --watchAll=false"
  }
}
```

## 8. Continuous Integration

### 8.1 GitHub Actions

Create `.github/workflows/test.yml`:

```yaml
name: Tests

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: '18'
      - run: npm install -g pnpm
      - run: pnpm install
      - run: pnpm test:ci
      - run: pnpm test:e2e
      - run: pnpm test:performance
```

## 9. Debugging Tests

### 9.1 Debug Jest Tests

```bash
# Run tests with debugging
node --inspect-brk node_modules/.bin/jest --runInBand

# Run tests with coverage
pnpm test:coverage
```

### 9.2 Debug Playwright Tests

```bash
# Run tests in headed mode
pnpm test:e2e --headed

# Run tests with debug mode
pnpm test:e2e --debug
```

## 10. Best Practices

### 10.1 Test Organization

- **Group related tests** using `describe`
- **Use meaningful test names** that explain the test purpose
- **Test edge cases** and error scenarios
- **Mock external dependencies** to ensure test isolation

### 10.2 Test Maintenance

- **Update tests** when code changes
- **Remove obsolete tests** that no longer serve a purpose
- **Refactor tests** to improve readability
- **Add tests** for new features

### 10.3 Performance Considerations

- **Avoid slow tests** that take too long to run
- **Use mocking** for external APIs and databases
- **Run tests in parallel** to speed up CI
- **Cache dependencies** to reduce setup time

## 11. Resources

- **Jest Documentation:** [https://jestjs.io/](https://jestjs.io/)
- **React Testing Library:** [https://testing-library.com/](https://testing-library.com/)
- **Playwright:** [https://playwright.dev/](https://playwright.dev/)
- **k6 Documentation:** [https://k6.io/docs/](https://k6.io/docs/)
- **Testing Best Practices:** [https://testing-library.com/docs/guidelines/](https://testing-library.com/docs/guidelines/)
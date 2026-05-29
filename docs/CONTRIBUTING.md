# Contributing Guidelines

## 1. Code of Conduct

We expect all contributors to adhere to our [Code of Conduct](https://github.com/aequiarch-org/aequiarch-esports/blob/main/CODE_OF_CONDUCT.md). Please report any violations to [security@aequiarch.org](mailto:security@aequiarch.org).

## 2. Getting Started

### 2.1 Prerequisites

- **Node.js 18+**
- **pnpm 8+**
- **Git**
- **Supabase Project** (for local development)

### 2.2 Clone the Repository

```bash
git clone https://github.com/aequiarch-org/aequiarch-esports.git
cd aequiarch-esports
pnpm install
```

### 2.3 Set Up Environment Variables

Copy `.env.example` to `.env.local` and fill in your Supabase and Stripe keys.

```bash
cp .env.example apps/web/.env.local
```

## 3. Project Structure

```bash
aequiarch-esports/
├── apps/
│   └── web/                        # Next.js application
│       ├── src/
│       │   ├── app/                # Next.js App Router
│       │   ├── components/         # Reusable components
│       │   ├── lib/                # Utility libraries
│       │   ├── server/             # Server-side logic
│       │   └── styles/             # Global styles
├── packages/
│   ├── db/                         # Database layer
│   ├── types/                      # Shared TypeScript types
│   └── ui/                         # Shared UI components
├── public/                        # Static assets
├── .github/                      # GitHub workflows
├── docs/                          # Documentation
├── .env.example                   # Environment variables template
├── package.json                   # Project dependencies
└── README.md                     # Project overview
```

## 4. Commit Message Conventions

Use the following format for commit messages:

```bash
<type>(<scope>): <description>

<BLANK LINE>
<BODY>

<BLANK LINE>
<FOOTER>
```

### 4.1 Types

- **feat:** A new feature
- **fix:** A bug fix
- **docs:** Documentation only changes
- **style:** Formatting, missing semi-colons, etc.
- **refactor:** Code refactoring
- **perf:** Performance improvements
- **test:** Adding missing tests
- **chore:** Maintenance tasks

### 4.2 Example

```bash
feat(tournaments): add tournament creation API

- Add POST endpoint for creating tournaments
- Add validation for tournament fields

Fixes #123
```

## 5. Coding Standards

### 5.1 JavaScript/TypeScript

- Use **TypeScript** for type safety.
- Follow **ESLint** and **Prettier** configurations.
- Write modular, reusable functions.

### 5.2 Database

- Use **Supabase migrations** for schema changes.
- Write clear, descriptive SQL queries.
- Use transactions for critical operations.

### 5.3 Testing

- Write **unit tests** for critical functions.
- Use **React Testing Library** for component tests.
- Test API endpoints with **tRPC** or **supertest**.

## 6. Pull Request Guidelines

### 6.1 Before Submitting

- Ensure your changes are up-to-date with the latest `main` branch.
- Write a clear and descriptive title and description.
- Reference any related issues or pull requests.
- Include screenshots or GIFs if applicable.

### 6.2 Pull Request Template

```markdown
## Description

A clear description of the changes made.

## Related Issues

- Fixes #123
- Closes #456

## Screenshots/GIFs

If applicable, add screenshots or GIFs here.
```

## 7. Review Process

- Your pull request will be reviewed by the maintainers.
- Be open to feedback and willing to make changes.
- Address any comments or questions promptly.

## 8. Releasing

### 8.1 Versioning

We follow **Semantic Versioning (SemVer)** for releases.

### 8.2 Release Process

1. Update the `CHANGELOG.md` file.
2. Bump the version in `package.json`.
3. Commit and tag the release:
   ```bash
   git tag v1.0.0
   git push origin v1.0.0
   ```
4. Create a release on GitHub.

## 9. Security

- Do not commit sensitive information (e.g., API keys, passwords).
- Use environment variables for secrets.
- Report any security vulnerabilities to [security@aequiarch.org](mailto:security@aequiarch.org).

## 10. Contact

For questions or suggestions, please open an issue or contact us at [contact@aequiarch.org](mailto:contact@aequiarch.org).
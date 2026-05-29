# Deployment Guide

## 1. Prerequisites

- **Node.js 18+**
- **pnpm 8+**
- **Supabase Project** ([Sign up here](https://supabase.com))
- **Stripe Account** ([Sign up here](https://stripe.com))
- **GitHub Account** (for CI/CD)

## 2. Setup Environment Variables

Copy the `.env.example` file to `.env.local` and fill in the required values:

```bash
cp .env.example apps/web/.env.local
```

### 2.1 Supabase Configuration

```env
NEXT_PUBLIC_SUPABASE_URL=your-supabase-url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your-supabase-anon-key
SUPABASE_SERVICE_ROLE_KEY=your-supabase-service-role-key
```

### 2.2 Stripe Configuration

```env
STRIPE_SECRET_KEY=your-stripe-secret-key
STRIPE_WEBHOOK_SECRET=your-stripe-webhook-secret
NEXT_PUBLIC_APP_URL=http://localhost:3000
```

## 3. Database Setup

### 3.1 Run Migrations

```bash
pnpm db:migrate
```

### 3.2 Seed Initial Data

```bash
pnpm db:seed
```

## 4. Frontend Deployment (Vercel)

### 4.1 Install Vercel CLI

```bash
npm install -g vercel
```

### 4.2 Deploy to Vercel

```bash
cd apps/web
vercel --prod
```

### 4.3 Environment Variables on Vercel

- Add the following environment variables in the Vercel dashboard:
  - `NEXT_PUBLIC_SUPABASE_URL`
  - `NEXT_PUBLIC_SUPABASE_ANON_KEY`
  - `SUPABASE_SERVICE_ROLE_KEY`
  - `STRIPE_SECRET_KEY`
  - `STRIPE_WEBHOOK_SECRET`
  - `NEXT_PUBLIC_APP_URL`

## 5. Backend Deployment (Vercel)

### 5.1 Deploy API Routes

The Next.js API routes are automatically deployed with the frontend on Vercel.

## 6. CI/CD Pipeline (GitHub Actions)

### 6.1 Setup GitHub Actions

Create a `.github/workflows/deploy.yml` file:

```yaml
name: Deploy to Vercel

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: '18'
      - run: npm install -g pnpm
      - run: pnpm install
      - run: pnpm build
      - uses: amondnet/vercel-action@v25
        with:
          vercel-token: ${{ secrets.VERCEL_TOKEN }}
          vercel-org-id: ${{ secrets.VERCEL_ORG_ID }}
          vercel-project-id: ${{ secrets.VERCEL_PROJECT_ID }}
          args: --prod
```

### 6.2 Add Secrets to GitHub

- Go to **GitHub Repository → Settings → Secrets → Actions**
- Add the following secrets:
  - `VERCEL_TOKEN`
  - `VERCEL_ORG_ID`
  - `VERCEL_PROJECT_ID`

## 7. Database Backups

### 7.1 Schedule Regular Backups

Use Supabase's built-in backup feature or set up a cron job:

```bash
# Example cron job to backup Supabase database
0 3 * * * pg_dump -h your-supabase-host -U postgres -d your-database-name > backup_$(date +%Y-%m-%d).sql
```

### 7.2 Store Backups Securely

Upload backups to a secure location like AWS S3 or Google Cloud Storage.

## 8. Monitoring

### 8.1 Supabase Monitoring

- Enable Supabase's built-in monitoring and alerts.

### 8.2 Vercel Analytics

- Enable Vercel Analytics to track usage and performance.

## 9. Scaling

### 9.1 Horizontal Scaling

- Vercel automatically scales Next.js apps.
- Supabase handles PostgreSQL scaling.

### 9.2 Database Optimization

- Use Supabase's query caching and edge functions for performance.

## 10. Troubleshooting

### 10.1 Common Issues

- **Error: Database connection failed**
  - Ensure your `.env` variables are correct.
  - Check Supabase's connection settings.

- **Error: Stripe payment failed**
  - Verify your Stripe webhook secret.
  - Check Stripe's dashboard for errors.

- **Error: Deployment failed**
  - Review Vercel logs for detailed errors.
  - Ensure all environment variables are set.

## 11. Updating

### 11.1 Update Dependencies

```bash
pnpm update
```

### 11.2 Run Migrations

```bash
pnpm db:migrate
```

### 11.3 Redeploy

```bash
vercel --prod
```
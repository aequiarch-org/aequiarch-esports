# aequiarch-esports

A full-stack, game-agnostic esports platform for tournament organizing, team management, match tracking, and live leaderboards — built for everyone from casual communities to professional orgs.

---

## Overview

aequiarch-esports is a freemium esports platform that supports:

- 🏆 Tournament Management — Create and run brackets for any game
- 👥 Team Management — Build rosters, manage roles, invite players
- 📊 Match Tracking & Stats — Log results, view history, track performance
- 🔴 Live Features — Real-time scores, in-platform chat, push notifications
- 🥇 Leaderboards — Global and per-game rankings
- 💳 Freemium Tiers — Free access with paid upgrades for orgs and power users

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | Next.js 14, TypeScript, Tailwind CSS |
| Backend | Next.js API Routes, tRPC |
| Database | Supabase (PostgreSQL) |
| Auth | Supabase Auth |
| Real-time | Supabase Realtime |
| Payments | Stripe |
| Monorepo | Turborepo |

---

## Repo Structure

aequiarch-esports/
├── apps/
│   └── web/                        # Main Next.js application
│       └── src/
│           ├── app/                # Next.js App Router pages
│           │   ├── (auth)/         # Login & Register
│           │   ├── dashboard/      # User dashboard
│           │   ├── tournaments/    # Tournament pages
│           │   ├── teams/          # Team pages
│           │   ├── leaderboard/    # Leaderboards
│           │   └── admin/          # Admin panel
│           ├── components/         # Reusable UI components
│           │   ├── ui/             # Base UI primitives
│           │   ├── tournament/     # Tournament-specific components
│           │   ├── team/           # Team components
│           │   ├── match/          # Match components
│           │   ├── chat/           # Live chat
│           │   ├── notifications/  # Notification system
│           │   ├── leaderboard/    # Leaderboard components
│           │   └── layout/         # Navbar, sidebar, footer
│           ├── lib/                # Utility libraries
│           │   ├── supabase/       # Supabase client setup
│           │   ├── stripe/         # Stripe client + webhooks
│           │   ├── realtime/       # Realtime channel helpers
│           │   └── utils/          # General utilities
│           ├── server/             # Server-side logic
│           │   ├── routers/        # tRPC routers
│           │   └── trpc/           # tRPC context & init
│           ├── hooks/              # Custom React hooks
│           └── styles/             # Global styles
├── packages/
│   ├── db/                         # Database layer
│   │   ├── migrations/             # SQL migrations
│   │   ├── schema/                 # Schema definitions
│   │   └── seed/                   # Seed data
│   ├── types/                      # Shared TypeScript types
│   ├── ui/                         # Shared UI components
│   └── config/                     # Shared configs (ESLint, TS, Tailwind)
├── .github/
│   └── workflows/                  # CI/CD pipelines
├── .env.example
├── turbo.json
└── package.json

---

## Getting Started

### Prerequisites

- Node.js 18+
- pnpm 8+
- A [Supabase](https://supabase.com) project
- A [Stripe](https://stripe.com) account

### Installation

```bash
# Clone the repo
git clone https://github.com/your-org/aequiarch-esports.git
cd aequiarch-esports

# Install dependencies
pnpm install

# Set up environment variables
cp .env.example apps/web/.env.local
# Fill in your Supabase and Stripe keys

# Run database migrations
pnpm db:migrate

# Start the dev server
pnpm dev
```

---

## Environment Variables

```env
# Supabase
NEXT_PUBLIC_SUPABASE_URL=
NEXT_PUBLIC_SUPABASE_ANON_KEY=
SUPABASE_SERVICE_ROLE_KEY=

# Stripe
STRIPE_SECRET_KEY=NEXT_P...KEY=
STRIPE_WEBHOOK_SECRET=*** App
NEXT_PUBLIC_APP_URL=http://localhost:3000
```

---

## Freemium Tiers

| Feature | Free | Pro | Org |
|---|---|---|---|
| Join tournaments | ✅ | ✅ | ✅ |
| Create tournaments | Up to 3 | Unlimited | Unlimited |
| Team size | Up to 5 | Up to 20 | Unlimited |
| Live chat | ✅ | ✅ | ✅ |
| Advanced stats | ❌ | ✅ | ✅ |
| Custom branding | ❌ | ❌ | ✅ |
| Admin panel | ❌ | ❌ | ✅ |

---

## Roadmap

- [ ] OAuth login (Discord, Steam, Google)
- [ ] Bracket generator (single/double elimination, round robin)
- [ ] In-game API integrations (Riot, Steam)
- [ ] Mobile app (React Native)
- [ ] Spectator mode & VOD links
- [ ] Org sub-accounts and role permissions

---

## Contributing

Pull requests are welcome. For major changes, please open an issue first to discuss what you'd like to change.

---

## License

[MIT](LICENSE)
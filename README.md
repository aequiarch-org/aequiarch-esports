# aequiarch-esports

A full-stack, game-agnostic esports platform for tournament organizing, team management, match tracking, and live leaderboards — built for everyone from casual communities to professional orgs.

---

## Licenses

This repository includes multiple licenses to protect the intellectual property of aequiarch-org. **No copying, modification, or distribution is allowed without explicit permission.**

### Available Licenses

- **Proprietary License** (`licenses/PROPRIETARY.md`) — **Default license.** No copying, modification, or distribution allowed.
- **AGPL-3.0 License** (`licenses/AGPL-3.0.md`) — Requires source code distribution if modified.
- **Custom No-Copy License** (`licenses/CUSTOM-NO-COPY.md`) — Strictly prohibits copying or redistribution.

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

## Contributing

Pull requests are welcome. For major changes, please open an issue first to discuss what you'd like to change.

---

## License

This project is protected by multiple licenses. **No copying, modification, or distribution is allowed without explicit permission.**

See the `licenses` folder for details.
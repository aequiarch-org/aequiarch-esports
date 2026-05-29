# aequiarch-esports

A full-stack, game-agnostic esports platform for tournament organizing, team management, match tracking, and live leaderboards — built for everyone from casual communities to professional orgs.

---

## 🏆 Tournament System
Organizers can create tournaments for any game with full bracket logic — single elimination, double elimination, and round robin formats. The system auto-generates match slots, seeds teams, advances winners through rounds automatically, and closes registration when slots fill. Anyone can browse all active, upcoming, and completed tournaments in one place.

---

### 👥 Team Management
Players form teams, assign a captain, build a roster with defined roles, and manage membership over time. Teams have a public profile page showing their record, roster, achievements, and tournament history. Captains can invite players, remove members, and update team info at any time.

---

### ⚔️ Match Scheduling & Results
Every match in a tournament is automatically scheduled with a round label and time slot. After a match, results are submitted and the bracket updates instantly — no manual work. Full match history is logged permanently for every team and player.

---

### 📡 Live Match Experience
When a match goes live, spectators get a dedicated match page with real-time score updates, a live event feed that logs every key moment as it happens, side-by-side stat comparisons between the two teams, and an elapsed game timer. It feels like watching a broadcast.

---

### 💬 Real-Time Chat
Every live match has its own chat room. Players, fans, and organizers can communicate in real-time during a match. Messages appear instantly across all viewers with usernames and timestamps.

---

### 📊 Player & Team Statistics
Every action on the platform feeds into persistent stats. Players accumulate wins, losses, points, and rank across all tournaments they compete in. Teams build a complete competitive record over time. Stats are visible on every profile and feed into the global leaderboard.

---

### 🥇 Global Leaderboard
A ranked list of every team and player on the platform sorted by points, win rate, and total wins. Filterable by game. The top 3 get a podium moment. Updates after every match result is submitted — always current.

---

### 🔔 Notifications & Alerts
Players get notified about upcoming match times, tournament registration confirmations, bracket advancement, and match results. Everything time-sensitive surfaces in the dashboard so nothing is missed.

---

### 🧑‍💼 Personal Dashboard
Each user lands on a command center showing everything relevant to them — their teams, tournaments they're registered in, upcoming scheduled matches, recent results, and notifications. One screen, full picture.

---

### 🎮 Game-Agnostic by Design
The platform has zero dependency on any specific game's API or ecosystem. Any competitive game can be run here — you define the game name when setting up a team or tournament. FPS, MOBA, battle royale, fighting games, card games, anything. The structure works universally.

---

### 🌍 Built for Every Level
A solo player forming their first team has the same tools as a semi-pro org running a 64-team bracket. No feature is locked. No experience required. The platform scales from casual Friday night lobbies to serious competitive circuits.

---

### 🔓 Free & Open Source Forever
Every single feature is free. No subscription. No premium tier. No ads. The codebase is open source so communities can self-host, fork, and extend it however they need.

---

## 🎯 Advanced Features

### 🎫 Tournament Registration Flow
Teams request to join a tournament. The organizer can approve or reject applications. Once approved, the team gets seeded into the bracket. Registration has an open/closed state with a visible deadline and remaining slot counter.

---

### 🌱 Free Agent System
Players who don't have a team can mark themselves as a free agent. Team captains can browse the free agent pool, filter by game and role, and send recruitment invites directly. Players get notified and can accept or decline.

---

### 📣 Tournament Announcements
Organizers can post announcements inside a tournament — rule changes, schedule updates, prize breakdowns. All registered participants get notified. Announcement history is visible on the tournament page.

---

### 🗓️ Match Check-In System
Before a match starts, both teams must check in within a time window (e.g. 15 minutes before). If a team fails to check in, they forfeit automatically. This prevents ghost matches and wasted bracket slots.

---

### 🏅 Achievements & Badges
Players and teams earn permanent badges for milestones — first tournament win, 100 matches played, undefeated run, top 3 on leaderboard. Badges display on profiles and add a progression layer beyond just stats.

---

### 📝 Match Dispute System
If a team believes a result was reported incorrectly, they can open a dispute within a time window after the match. The organizer reviews both sides, checks submitted evidence, and issues a ruling. The match result updates accordingly.

---

### 🎙️ Player Reputation & Reviews
After a tournament, players can rate opponents on sportsmanship — separate from skill stats. High reputation players get a badge. Low reputation players get flagged. Organizers can filter flagged players from their tournaments.

---

### 📸 Team & Player Media Wall
Teams and players can attach VOD links, highlight clips, and screenshots to their profiles. Match history entries can link to recorded footage. Builds a portfolio of competitive history over time.

---

### 🔗 Public Shareable Profiles
Every team and player has a clean public URL — aequiarch.gg/teams/aether-vanguard. No login required to view. Shareable on social media, Discord, anywhere. Organizers can use it to verify team legitimacy before approving registration.

---

### 🗺️ Regional Brackets
Tournaments can be scoped to a region — North America, Europe, Southeast Asia, etc. Leaderboards filter by region. Players set their region on signup. Reduces ping issues and keeps competition local when needed.

---

### 📆 Recurring Tournaments
Organizers can schedule weekly, monthly, or seasonal tournament series — not just one-off events. The platform auto-creates the next instance when the current one closes. Teams can subscribe to a series and get auto-reminded each cycle.

---

### 🤝 Sponsorship & Prize Submission
Organizers can list a prize pool and describe what's being offered — cash, gift cards, in-game items, hardware. Players see exactly what they're competing for. After the tournament, the organizer marks prizes as distributed per winner.

---

### 🛡️ Anti-Smurfing & Eligibility Rules
Organizers can set eligibility requirements for a tournament — minimum rank, maximum win rate, account age. Players who don't meet the criteria are blocked from registering. Keeps competition fair and skill-appropriate.

---

### 📬 In-Platform Messaging
Direct messaging between players and team captains. Organizers can message all participants at once. Keeps all competition communication inside the platform rather than scattered across Discord and DMs.

---

### 📈 Tournament Analytics for Organizers
Organizers see a dashboard for their tournament — registration rate over time, average match duration, most active participants, dropout rate, peak viewer count on live matches. Helps improve future events.

---

### 🔁 Sub & Stand-In System
If a team member can't play a specific match, the captain can register a temporary stand-in from an approved pool. The stand-in is flagged on the match page. Prevents forfeits due to one unavailable player.

---

### 🏗️ Custom Tournament Rules Builder
Organizers define the ruleset inside the platform — map pool, game mode, scoring format, overtime rules, banned characters/loadouts. Rules are publicly visible on the tournament page and must be acknowledged on registration.

---

### 🌐 Multi-Language Support
The platform UI is available in multiple languages. Players set their preferred language on their profile. Tournament announcements can be posted in multiple languages simultaneously.

---

### 🔐 Role-Based Access Control
Platform roles: Player, Team Captain, Tournament Organizer, Platform Admin. Each role has specific permissions. Captains can't modify other teams. Organizers can only manage their own tournaments. Admins have full oversight.

---

## 🚀 Getting Started

### Prerequisites

- Node.js 18+
- pnpm 8+
- Supabase Project
- Stripe Account

### Installation

```bash
# Clone the repo
git clone https://github.com/aequiarch-org/aequiarch-esports.git
cd aequiarch-esports

# Install dependencies
pnpm install

# Set up environment variables
cp .env.example apps/web/.env.local

# Run database migrations
pnpm db:migrate

# Start the dev server
pnpm dev
```

---

## 📚 Documentation

- [📖 Features Overview](docs/FEATURES.md) - Complete feature documentation
- [🏗️ Architecture](docs/ARCHITECTURE.md) - System design and components
- [🔌 API Reference](docs/API.md) - REST/tRPC endpoints and examples
- [💾 Database Schema](docs/DATABASE.md) - Schema and migrations
- [🚀 Deployment](docs/DEPLOYMENT.md) - Production deployment guide
- [🤝 Contributing](docs/CONTRIBUTING.md) - How to contribute
- [📈 Roadmap](docs/ROADMAP.md) - Future development plans
- [🔒 Security](docs/SECURITY.md) - Security policies
- [🧪 Testing](docs/TESTING.md) - Testing strategy
- [❓ FAQ](docs/FAQ.md) - Frequently asked questions
- [📝 Changelog](docs/CHANGELOG.md) - Version history

---

## 🎯 Roadmap

### v0.1.0 (Current) - Core Foundation
- ✅ Tournament system with bracket logic
- ✅ Team management and profiles
- ✅ Match scheduling and results
- ✅ Live match experience
- ✅ Real-time chat
- ✅ Statistics and leaderboards
- ✅ Notifications and dashboard
- ✅ Game-agnostic design
- ✅ Free and open source

### v0.2.0 (Next) - Advanced Features
- 🔄 Tournament registration flow
- 🔄 Free agent system
- 🔄 Tournament announcements
- 🔄 Match check-in system
- 🔄 Achievements and badges
- 🔄 Match dispute system
- 🔄 Player reputation system
- 🔄 Media wall
- 🔄 Public profiles
- 🔄 Regional brackets
- 🔄 Recurring tournaments
- 🔄 Sponsorship and prizes
- 🔄 Anti-smurfing rules
- 🔄 In-platform messaging
- 🔄 Tournament analytics
- 🔄 Sub and stand-in system
- 🔄 Custom rules builder
- 🔄 Multi-language support
- 🔄 Role-based access control

### v0.3.0 - Enhanced Experience
- 📱 Mobile app
- 🤖 AI-powered features
- 📊 Advanced analytics
- 🔌 External integrations

### v1.0.0 - Global Expansion
- 🌍 Multi-region support
- 🏢 Enterprise features
- 🎯 Professional esports integration
- 💰 Advanced monetization

---

## 🤝 Contributing

We welcome contributions! Please read our [Contributing Guidelines](docs/CONTRIBUTING.md) for details on our code of conduct and the process for submitting pull requests.

---

## 📄 License

This project is protected by multiple licenses. **No copying, modification, or distribution is allowed without explicit permission.**

See the `licenses` folder for details.

---

## 🌐 Community

- **Discord:** [Join our community](https://discord.gg/aequiarch)
- **GitHub:** [Source code](https://github.com/aequiarch-org/aequiarch-esports)
- **Support:** [contact@aequiarch.org](mailto:contact@aequiarch.org)

---

## 🏆 Built for Esports

aequiarch-esports is designed to be the ultimate platform for competitive gaming — from casual communities to professional circuits. Every feature is built with the competitive player in mind, focusing on fairness, transparency, and community.

**No paywalls. No ads. No restrictions. Just pure esports.**
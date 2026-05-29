# Database Schema

## 1. Tables

### 1.1 `users`
Stores user profiles and authentication details.

```sql
CREATE TABLE users (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  name VARCHAR(255) NOT NULL,
  email VARCHAR(255) UNIQUE NOT NULL,
  discord_id VARCHAR(255),
  steam_id VARCHAR(255),
  role VARCHAR(50) DEFAULT 'player' CHECK (role IN ('player', 'admin', 'org')),
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);
```

### 1.2 `teams`
Stores team information and roster.

```sql
CREATE TABLE teams (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  name VARCHAR(255) NOT NULL,
  game VARCHAR(100) NOT NULL,
  owner_id UUID REFERENCES users(id) ON DELETE CASCADE,
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE TABLE team_members (
  team_id UUID REFERENCES teams(id) ON DELETE CASCADE,
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  role VARCHAR(50) DEFAULT 'member' CHECK (role IN ('member', 'captain')),
  joined_at TIMESTAMPTZ DEFAULT NOW(),
  PRIMARY KEY (team_id, user_id)
);
```

### 1.3 `tournaments`
Stores tournament details and brackets.

```sql
CREATE TABLE tournaments (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  name VARCHAR(255) NOT NULL,
  game VARCHAR(100) NOT NULL,
  start_date TIMESTAMPTZ NOT NULL,
  end_date TIMESTAMPTZ,
  entry_fee DECIMAL(10, 2) DEFAULT 0,
  max_teams INTEGER DEFAULT 16,
  status VARCHAR(50) DEFAULT 'draft' CHECK (status IN ('draft', 'active', 'completed')),
  bracket JSONB DEFAULT '[]',
  created_by UUID REFERENCES users(id) ON DELETE CASCADE,
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE TABLE tournament_teams (
  tournament_id UUID REFERENCES tournaments(id) ON DELETE CASCADE,
  team_id UUID REFERENCES teams(id) ON DELETE CASCADE,
  registered_at TIMESTAMPTZ DEFAULT NOW(),
  PRIMARY KEY (tournament_id, team_id)
);
```

### 1.4 `matches`
Stores match results and stats.

```sql
CREATE TABLE matches (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  tournament_id UUID REFERENCES tournaments(id) ON DELETE CASCADE,
  team1_id UUID REFERENCES teams(id) ON DELETE CASCADE,
  team2_id UUID REFERENCES teams(id) ON DELETE CASCADE,
  winner_id UUID REFERENCES teams(id) ON DELETE CASCADE,
  score VARCHAR(50) NOT NULL,
  round INTEGER DEFAULT 1,
  status VARCHAR(50) DEFAULT 'pending' CHECK (status IN ('pending', 'in_progress', 'completed')),
  scheduled_at TIMESTAMPTZ,
  completed_at TIMESTAMPTZ,
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);
```

### 1.5 `leaderboards`
Stores rankings and scores.

```sql
CREATE TABLE leaderboards (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  game VARCHAR(100) NOT NULL,
  team_id UUID REFERENCES teams(id) ON DELETE CASCADE,
  points INTEGER DEFAULT 0,
  wins INTEGER DEFAULT 0,
  losses INTEGER DEFAULT 0,
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);
```

### 1.6 `payments`
Stores payment transactions.

```sql
CREATE TABLE payments (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  tournament_id UUID REFERENCES tournaments(id) ON DELETE CASCADE,
  team_id UUID REFERENCES teams(id) ON DELETE CASCADE,
  amount DECIMAL(10, 2) NOT NULL,
  status VARCHAR(50) DEFAULT 'pending' CHECK (status IN ('pending', 'completed', 'failed')),
  stripe_payment_id VARCHAR(255),
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);
```

## 2. Indexes

```sql
CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_teams_game ON teams(game);
CREATE INDEX idx_tournaments_game ON tournaments(game);
CREATE INDEX idx_tournaments_status ON tournaments(status);
CREATE INDEX idx_matches_tournament ON matches(tournament_id);
CREATE INDEX idx_matches_status ON matches(status);
CREATE INDEX idx_leaderboards_game ON leaderboards(game);
```

## 3. Triggers

### 3.1 Update `updated_at`
```sql
CREATE OR REPLACE FUNCTION update_updated_at()
RETURNS TRIGGER AS $$
BEGIN
  NEW.updated_at = NOW();
  RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER update_users_updated_at BEFORE UPDATE ON users
  FOR EACH ROW EXECUTE FUNCTION update_updated_at();

CREATE TRIGGER update_teams_updated_at BEFORE UPDATE ON teams
  FOR EACH ROW EXECUTE FUNCTION update_updated_at();

CREATE TRIGGER update_tournaments_updated_at BEFORE UPDATE ON tournaments
  FOR EACH ROW EXECUTE FUNCTION update_updated_at();

CREATE TRIGGER update_matches_updated_at BEFORE UPDATE ON matches
  FOR EACH ROW EXECUTE FUNCTION update_updated_at();

CREATE TRIGGER update_leaderboards_updated_at BEFORE UPDATE ON leaderboards
  FOR EACH ROW EXECUTE FUNCTION update_updated_at();
```

## 4. Seed Data

```sql
-- Insert sample users
INSERT INTO users (name, email, role) VALUES
  ('John Doe', 'john@example.com', 'player'),
  ('Jane Smith', 'jane@example.com', 'admin'),
  ('Alice Johnson', 'alice@example.com', 'org');

-- Insert sample teams
INSERT INTO teams (name, game, owner_id) VALUES
  ('Team Phoenix', 'CS2', (SELECT id FROM users WHERE name = 'John Doe')),
  ('Team Nova', 'Valorant', (SELECT id FROM users WHERE name = 'Jane Smith'));

-- Insert team members
INSERT INTO team_members (team_id, user_id, role) VALUES
  ((SELECT id FROM teams WHERE name = 'Team Phoenix'), (SELECT id FROM users WHERE name = 'John Doe'), 'captain'),
  ((SELECT id FROM teams WHERE name = 'Team Nova'), (SELECT id FROM users WHERE name = 'Jane Smith'), 'captain');

-- Insert sample tournaments
INSERT INTO tournaments (name, game, start_date, end_date, entry_fee, max_teams, status, created_by) VALUES
  ('Summer Showdown', 'CS2', '2026-12-01', '2026-12-15', 10, 16, 'active', (SELECT id FROM users WHERE name = 'Alice Johnson'));

-- Insert tournament teams
INSERT INTO tournament_teams (tournament_id, team_id) VALUES
  ((SELECT id FROM tournaments WHERE name = 'Summer Showdown'), (SELECT id FROM teams WHERE name = 'Team Phoenix'));

-- Insert sample matches
INSERT INTO matches (tournament_id, team1_id, team2_id, winner_id, score, status, scheduled_at) VALUES
  ((SELECT id FROM tournaments WHERE name = 'Summer Showdown'),
   (SELECT id FROM teams WHERE name = 'Team Phoenix'),
   (SELECT id FROM teams WHERE name = 'Team Nova'),
   (SELECT id FROM teams WHERE name = 'Team Phoenix'),
   '2-1', 'completed', '2026-12-10');

-- Insert leaderboard entries
INSERT INTO leaderboards (game, team_id, points, wins, losses) VALUES
  ('CS2', (SELECT id FROM teams WHERE name = 'Team Phoenix'), 1500, 1, 0),
  ('Valorant', (SELECT id FROM teams WHERE name = 'Team Nova'), 1200, 0, 1);
```
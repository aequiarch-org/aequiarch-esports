# API Documentation

## 1. Authentication

### 1.1 Login
- **Endpoint:** `POST /api/auth/login`
- **Body:**
  ```json
  {
    "email": "user@example.com",
    "password": "securepassword"
  }
  ```
- **Response:**
  ```json
  {
    "token": "jwt.token.here",
    "user": {
      "id": "user123",
      "name": "John Doe",
      "role": "player"
    }
  }
  ```

### 1.2 Discord OAuth
- **Endpoint:** `GET /api/auth/discord`
- **Response:** Redirects to Discord OAuth.

## 2. Tournaments

### 2.1 Create Tournament
- **Endpoint:** `POST /api/tournaments`
- **Body:**
  ```json
  {
    "name": "Summer Showdown",
    "game": "Valorant",
    "startDate": "2026-12-01",
    "entryFee": 10
  }
  ```
- **Response:**
  ```json
  {
    "id": "tournament123",
    "name": "Summer Showdown",
    "bracket": []
  }
  ```

### 2.2 Join Tournament
- **Endpoint:** `POST /api/tournaments/:id/join`
- **Body:**
  ```json
  {
    "teamId": "team123"
  }
  ```
- **Response:**
  ```json
  {
    "success": true,
    "message": "Team joined tournament"
  }
  ```

## 3. Teams

### 3.1 Create Team
- **Endpoint:** `POST /api/teams`
- **Body:**
  ```json
  {
    "name": "Team Phoenix",
    "game": "CS2"
  }
  ```
- **Response:**
  ```json
  {
    "id": "team123",
    "name": "Team Phoenix"
  }
  ```

### 3.2 Invite Player
- **Endpoint:** `POST /api/teams/:id/invite`
- **Body:**
  ```json
  {
    "userId": "user456"
  }
  ```
- **Response:**
  ```json
  {
    "success": true,
    "message": "Invite sent"
  }
  ```

## 4. Matches

### 4.1 Submit Match Result
- **Endpoint:** `POST /api/matches`
- **Body:**
  ```json
  {
    "tournamentId": "tournament123",
    "winnerId": "team123",
    "score": "2-1"
  }
  ```
- **Response:**
  ```json
  {
    "id": "match123",
    "winner": "Team Phoenix"
  }
  ```

## 5. Leaderboards

### 5.1 Get Leaderboard
- **Endpoint:** `GET /api/leaderboard?game=Valorant`
- **Response:**
  ```json
  [
    {
      "teamId": "team123",
      "name": "Team Phoenix",
      "points": 1500
    }
  ]
  ```

## 6. Payments

### 6.1 Create Checkout Session
- **Endpoint:** `POST /api/payments/checkout`
- **Body:**
  ```json
  {
    "tournamentId": "tournament123",
    "teamId": "team123"
  }
  ```
- **Response:**
  ```json
  {
    "url": "https://checkout.stripe.com/..."
  }
  ```

## 7. Realtime

### 7.1 Subscribe to Match Updates
- **Endpoint:** `GET /api/realtime/match/:id`
- **Response:** Supabase Realtime channel token.

## 8. Error Handling

All errors return:
```json
{
  "error": {
    "code": "invalid_input",
    "message": "Missing required field: name"
  }
}
```
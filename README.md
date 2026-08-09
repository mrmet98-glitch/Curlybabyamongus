# IRL Among Us — Cloudflare Workers + D1

A phone-first party game controller for in-person Among Us-style games.

## Features
- 6-character lobby codes, up to 30 players
- One host, configurable impostor count
- Random role assignment
- Host-defined task pool and random min/max tasks per player
- Fake tasks for impostors; only crew tasks count toward shared progress
- Player task checkboxes + public completion bar
- Body report / emergency meeting button with sound + vibration alert
- Dead/alive tracking
- One vote per alive player, Skip option, host-controlled end of voting
- Tie handling + reveal whether ejected player was an impostor
- D1 persistence for games, players, tasks, and votes

## Deploy

### 1. Install
```bash
npm install
```

### 2. Create D1
```bash
npx wrangler d1 create irl-among-us-db
```
Copy the returned `database_id` into `wrangler.jsonc`.

### 3. Create tables
```bash
npx wrangler d1 execute irl-among-us-db --remote --file=./schema.sql
```

For local development:
```bash
npx wrangler d1 execute irl-among-us-db --local --file=./schema.sql
npm run dev
```

### 4. Deploy
```bash
npm run deploy
```

## GitHub → Cloudflare
Push this folder to GitHub. In Cloudflare, create/connect a Workers project to the repo and use the deploy command:

```bash
npx wrangler deploy
```

The D1 binding must be named `DB`, matching `wrangler.jsonc`.

## Notes
- Phones poll game state roughly every 1.8 seconds. This avoids additional realtime infrastructure and is more than fast enough for an in-person party game.
- Mobile Safari/Chrome require a user gesture before browser-generated sound can play. The app initializes audio when a player creates/joins, and also vibrates where supported.
- The current version trusts the host and players socially, just like the physical game. It does not implement account login.

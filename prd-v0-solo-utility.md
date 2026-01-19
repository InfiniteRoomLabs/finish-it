# Game Picker Platform - V0 PRD

## Solo Utility

---

## Overview

A web application that helps users decide what to play from their Steam library using a weighted random selection system. V0 focuses entirely on the solo utility experience—no social features.

---

## Problem Statement

Gamers with large Steam libraries often experience decision paralysis when choosing what to play. The abundance of options leads to either replaying familiar games or not playing at all. Existing solutions (Steam's built-in random selector, external wheels) lack intelligence and don't account for user preferences or progress.

---

## Goals

1. Eliminate "what should I play" paralysis
2. Surface games that are close to completion
3. Let users organize their library in ways Steam doesn't support
4. Provide satisfying stats about gaming habits

---

## Non-Goals (V0)

- Social features (friends, feed, boosts)
- Content creation (blogs, guides)
- Non-Steam game sources
- Mobile apps

---

## User Stories

### Authentication

- As a user, I can sign in with my Steam account
- As a user, my profile is automatically created using my Steam profile info
- As a user, I can sign out

### Library Sync

- As a user, my Steam library syncs automatically on first login
- As a user, I can manually trigger a library resync
- As a user, I can see all my Steam games with their banner artwork

### The Spinner

- As a user, I can spin to get a random game from my library
- As a user, I see a visually satisfying animation as games rotate through
- As a user, I can spin again if I don't like the result
- As a user, I can configure which weighting factors apply to my spins
- As a user, games marked "finished" are excluded from spins by default
- As a user, I can toggle whether to include finished games

### Weighting System

- As a user, games I'm close to completing (based on HLTB) are weighted higher
- As a user, I can disable HLTB weighting if I prefer pure random
- As a user, I can filter spins by my custom tags
- As a user, I can filter spins by genre/category

### Game Management

- As a user, I can view details for any game in my library
- As a user, I can mark a game as finished
- As a user, I can mark a game as abandoned
- As a user, I can add custom tags to any game
- As a user, I can remove tags from games
- As a user, I can see my playtime vs HLTB estimates

### Stats

- As a user, I can see per-game stats:
  - Total playtime
  - Session count (derived from playtime deltas)
  - Average session length
  - Achievements (X/Y completed)
  - HLTB comparison
  - Status (unplayed/playing/finished/abandoned)
  - My tags
- As a user, I can see profile-level stats:
  - Total games owned
  - Games finished
  - Games abandoned
  - Completion rate
  - Total playtime across all games
  - Most played games

### Profile

- As a user, I have a public profile page showing:
  - Steam avatar and username
  - Aggregate stats
  - Recently played games
  - Finished games

---

## Data Requirements

### External Integrations

**Steam Web API**
- Authentication (OpenID)
- Library fetch (owned games)
- Player stats (playtime, achievements)
- Game metadata (name, banner, etc.)

**Steam Store API / SteamDB**
- Genres
- Categories
- Community tags
- Additional metadata

**HowLongToBeat**
- Main story completion time
- Main + extras completion time
- Completionist time

### Sync Strategy

- Full library sync on first login
- Incremental playtime/achievement sync (polling interval TBD)
- HLTB data fetched on-demand and cached

---

## Weighting Algorithm

The spinner uses weighted random selection. Each game receives a weight based on configurable factors:

```
base_weight = 1.0

if hltb_weighting_enabled:
  completion_ratio = user_playtime / hltb_main_time
  if 0.7 <= completion_ratio < 1.0:
    weight *= 2.0  # Close to finishing
  elif completion_ratio >= 1.0:
    weight *= 0.5  # Already past estimate

if tag_filter_active:
  if game has selected tag:
    weight *= 1.0
  else:
    weight = 0

if status == 'finished' and exclude_finished:
  weight = 0

if status == 'abandoned':
  weight *= 0.25  # Low but not zero
```

Weights are normalized and used for random selection.

---

## Pages / Views

1. **Landing** - Marketing page, sign in with Steam CTA
2. **Spinner** - Main feature, the wheel
3. **Library** - Grid/list of all games, filterable
4. **Game Detail** - Single game view with stats, tags, status
5. **Profile** - User's public profile with stats
6. **Settings** - Weighting preferences, sync controls

---

## Technical Considerations

- Steam API rate limits will require caching and smart polling
- HLTB has no official API; will need scraping or a third-party service
- Banner images should be lazy-loaded and cached
- Spinner animation should be performant (Canvas or WebGL)

---

## Success Metrics

- Users complete a spin (activation)
- Users return and spin again within 7 days (retention)
- Users mark games as finished (engagement with status system)
- Users create custom tags (engagement with organization)

---

## Open Questions

- How often should we poll Steam for playtime updates?
- Should we support multiple HLTB categories (main vs completionist)?
- What's the spinner animation style? (slot machine, wheel, card flip?)

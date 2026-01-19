# Game Picker Platform - V1 PRD

## Social Layer

---

## Overview

V1 builds on the solo utility foundation (V0) by adding social features. Users can connect with friends, see activity feeds, boost games in friends' libraries, and engage through reactions and comments.

---

## Prerequisites

All V0 features are complete and stable.

---

## Problem Statement

Gaming is often social, but the "what should I play" decision is isolated. Friends have opinions about what you should play, but there's no lightweight way to express that. Additionally, gaming accomplishments (finishing a game, getting achievements) go unshared unless you actively post about them elsewhere.

---

## Goals

1. Make gaming decisions social without being annoying
2. Give friends a way to influence what you play (boosts)
3. Create a feed of authentic gaming activity
4. Celebrate completions and achievements

---

## Non-Goals (V1)

- Content creation (blogs, guides)
- Group spin sessions
- Non-Steam friend connections
- Direct messaging

---

## User Stories

### Friend Connections

- As a user, my Steam friends who are also on the platform are automatically connected
- As a user, I can see which of my Steam friends are on the platform
- As a user, I can view my friends list
- As a user, I can view a friend's public profile

### Activity Feed

- As a user, I have a feed showing my friends' activity
- As a user, I see feed items when a friend:
  - Logs significant playtime on a game
  - Unlocks an achievement
  - Marks a game as finished (with stats summary)
  - Marks a game as abandoned
  - Adds a new game to their library
- As a user, I can view my own activity history
- As a user, feed items link to the relevant game/profile

### Finished Announcements

- As a user, when I mark a game finished, a rich feed item is created showing:
  - Total playtime
  - Session count
  - Average session length
  - Achievement progress
  - Time vs HLTB estimate
- As a user, I can add a short comment when marking finished

### Boosts

- As a user, I can boost a game in a friend's library
- As a user, I can see which friends have boosted games in my library
- As a user, boosted games are weighted higher in my spinner
- As a user, I can see how many boosts each game has received
- As a user, I can configure how much boosts affect my weighting
- As a user, I can disable boost weighting entirely
- As a user, I receive a notification when someone boosts a game for me

### Reactions

- As a user, I can react to feed items (emoji reactions)
- As a user, I can see who reacted to my activity
- As a user, I can remove my reaction

### Comments

- As a user, I can comment on feed items
- As a user, I can edit my comments
- As a user, I can delete my comments
- As a user, I receive a notification when someone comments on my activity
- As a user, I can view all comments on a feed item

### Notifications

- As a user, I receive notifications for:
  - New boosts on my games
  - Reactions to my activity
  - Comments on my activity
- As a user, I can view my notification history
- As a user, I can mark notifications as read
- As a user, I can configure notification preferences

---

## Feed Item Types

| Event Type | Auto-Published | Data Shown |
|------------|----------------|------------|
| Playtime logged | Yes (threshold TBD) | Game, hours added, total hours |
| Achievement unlocked | Yes | Game, achievement name/icon |
| Game finished | Yes | Game, full stats summary |
| Game abandoned | Yes | Game, playtime, note (optional) |
| Game added | Yes | Game |

---

## Boost Mechanics

Boosts are the core social mechanic—a friend's way of saying "you really need to play this."

**Rules:**
- One boost per friend per game (no spam)
- Boost persists until the game is finished or abandoned
- Boost weight is configurable (default: +50% weight per boost)
- Boosts from multiple friends stack
- User can cap max boost influence (e.g., max 2x weight)

**Visibility:**
- Game detail shows who boosted it
- Library view shows boost count badge
- Spinner shows "X friends want you to play this" on result

---

## Weighting Algorithm (Updated)

Extends V0 weighting:

```
base_weight = 1.0

# V0 factors
if hltb_weighting_enabled:
  # ... same as V0

if tag_filter_active:
  # ... same as V0

if status == 'finished' and exclude_finished:
  weight = 0

if status == 'abandoned':
  weight *= 0.25

# V1 additions
if boost_weighting_enabled:
  boost_multiplier = 1 + (boost_count * boost_strength)
  boost_multiplier = min(boost_multiplier, max_boost_cap)
  weight *= boost_multiplier
```

---

## Pages / Views (New & Updated)

### New Pages

1. **Feed** - Activity feed (own + friends)
2. **Friends** - List of connected friends
3. **Notifications** - Notification center

### Updated Pages

1. **Game Detail** - Now shows boosts, social activity for this game
2. **Profile** - Now shows recent activity, can be viewed by others
3. **Library** - Now shows boost badges
4. **Spinner** - Shows boost info on result
5. **Settings** - Now includes notification preferences, boost weight config

---

## Notification System

**Channels:**
- In-app (always)
- Email (configurable, default off)
- Push (future consideration)

**Batching:**
- Multiple reactions on same item batched ("3 people reacted to...")
- Achievement spam batched ("You unlocked 5 achievements in...")

---

## Privacy Considerations

- All activity is visible to friends by default
- Users can set profile to private (friends only)
- Users can hide specific games from activity
- Playtime is already public on Steam; we mirror that model

---

## Technical Considerations

- Feed requires efficient fan-out (activity → all friends' feeds)
- Consider event sourcing for activity stream
- Notification batching needs background job processing
- Real-time updates (WebSocket) nice-to-have, not required

---

## Success Metrics

- Users with 3+ friends connected (network activation)
- Boost actions per user per week (core mechanic engagement)
- Feed views per session (feed engagement)
- Comments per active user (deeper engagement)
- Retention improvement vs V0 (does social increase stickiness?)

---

## Open Questions

- Should boosts expire after some time?
- Should there be a "boost back" or reciprocity mechanic?
- What's the playtime threshold for auto-publishing? (30 min? 1 hour?)
- Should users be able to manually post to feed?

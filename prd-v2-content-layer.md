# Game Picker Platform - V2 PRD

## Content Layer

---

## Overview

V2 transforms the platform from a utility with social features into a content destination. Users can write notes, journals, reviews, and guides. Content can be scoped to specific games or stand alone on profiles. This creates organic discovery, SEO acquisition, and deeper engagement.

---

## Prerequisites

All V1 features are complete and stable. Active user base generating social activity.

---

## Problem Statement

Users finish games and have thoughts, but sharing them is fragmented (Reddit posts, tweets, Discord messages). There's no natural home for gaming journals or personal game notes. Meanwhile, guides are scattered across outdated wikis, cluttered YouTube videos, and Steam's clunky guide system.

---

## Goals

1. Give users a place to write about their gaming experiences
2. Create community content around specific games
3. Enable guide/walkthrough creation with proper tooling
4. Drive organic traffic through SEO

---

## Non-Goals (V2)

- Video content hosting
- Streaming integration
- Wiki-style collaborative editing
- Monetization for creators

---

## Content Model

### Post Types

All content uses a unified post system with different intents:

| Type | Scoped to Game | Description |
|------|----------------|-------------|
| Note | Optional | Quick thought, tip, observation |
| Journal | Yes | Session log, progress update |
| Review | Yes | Thoughts on completed game |
| Guide | Yes | Walkthrough, tutorial, strategy |
| Blog | No | General gaming thoughts, roundups |

### Content Scope

**Game-scoped content appears in:**
- Author's profile (game section)
- Author's profile (content tab)
- Global game page
- Activity feed (on publish)

**Unscoped content appears in:**
- Author's profile (content tab)
- Activity feed (on publish)

---

## User Stories

### Content Creation

- As a user, I can create a new post from my profile or a game page
- As a user, I can select the post type (note, journal, review, guide, blog)
- As a user, I can optionally link a post to a specific game
- As a user, I can write content using a rich text editor
- As a user, I can save drafts
- As a user, I can preview before publishing
- As a user, I can publish content to my profile and feed

### Rich Text Editor

- As a user, I can format text (bold, italic, headers, lists)
- As a user, I can add images (upload or link)
- As a user, I can embed screenshots from Steam
- As a user, I can add spoiler tags that hide content until clicked
- As a user, I can add links
- As a user, I can create code blocks (for config/command guides)

### Guide-Specific Features

- As a user, I can organize guides into sections/chapters
- As a user, I can create a table of contents
- As a user, I can link to specific sections (anchor links)
- As a user, I can mark sections with spoiler warnings
- As a user, I can update/edit published guides
- As a user, I can see edit history on my guides

### Content Management

- As a user, I can view all my content on my profile
- As a user, I can edit published content
- As a user, I can delete my content
- As a user, I can see view counts on my content
- As a user, I can see reactions and comments on my content

### Content Discovery

- As a user, I can browse all content for a specific game
- As a user, I can filter game content by type (notes, guides, reviews)
- As a user, I can sort by recent, popular, or most helpful
- As a user, I can search content within a game
- As a user, content appears in my feed when friends publish

### Global Game Pages

- As a user, I can view a global page for any game
- As a user, the game page shows:
  - Game metadata (from Steam/HLTB)
  - Aggregate stats (avg playtime, completion rate on platform)
  - Community content (all user posts about this game)
  - Guides section
  - Recent activity

### Reactions & Comments (Extended)

- As a user, I can react to posts (same as feed items)
- As a user, I can comment on posts
- As a user, I can mark a guide as "helpful"
- As a user, helpful votes affect guide ranking

---

## Content Editor Spec

### Supported Blocks

- Paragraph
- Heading (H1, H2, H3)
- Bullet list
- Numbered list
- Image (with caption, optional spoiler)
- Spoiler block
- Code block
- Blockquote
- Horizontal rule
- Table (for guides)

### Spoiler System

Two levels:
1. **Inline spoiler** - Blurred text, click to reveal
2. **Block spoiler** - Entire section hidden with warning label

Authors can label spoilers: "Story spoiler", "Boss strategy", "Endgame content", etc.

### Image Handling

- Direct upload (stored on platform CDN)
- Steam screenshot import (via Steam API)
- External URL embed
- Max file size TBD
- Auto-compression for large images

---

## Global Game Pages

Each game in Steam's catalog gets a page on the platform, regardless of whether any user owns it.

### Page Sections

1. **Hero** - Banner, title, metadata
2. **Stats** - Platform-wide stats (users own it, avg playtime, completion %)
3. **HLTB Info** - Completion times
4. **Guides** - Community guides, sorted by helpful votes
5. **Reviews** - User reviews from platform
6. **Activity** - Recent posts, notes, journals about this game
7. **Tags** - Aggregated community tags

### SEO Value

These pages are the primary SEO asset:
- "[Game name] guide" searches
- "[Game name] how long to beat" searches
- "[Game name] review" searches

Content is public and crawlable.

---

## Feed Integration

New feed item types:

| Event Type | Auto-Published | Data Shown |
|------------|----------------|------------|
| Note published | Yes | Game (if scoped), preview text |
| Journal published | Yes | Game, preview text |
| Review published | Yes | Game, preview text |
| Guide published | Yes | Game, title, description |
| Blog published | Yes | Title, preview text |

Users can choose to publish drafts silently (no feed item) if desired.

---

## Content Moderation

### Automated

- Spam detection on publish
- Prohibited content filtering
- Rate limiting on posts

### Community

- Report button on all content
- Moderator review queue
- Content removal with author notification

### Author Tools

- Edit/delete own content
- Disable comments on specific posts

---

## Pages / Views (New & Updated)

### New Pages

1. **Post Editor** - Rich text editor for creating content
2. **Post View** - Single post page (shareable URL)
3. **Global Game Page** - Community hub for each game
4. **Content Browse** - Discover content by game, type, popularity
5. **My Content** - Manage all created content

### Updated Pages

1. **Profile** - New content tab showing all posts
2. **Game Detail** (personal) - Shows user's notes/journals for that game
3. **Feed** - Now includes content publish events

---

## Technical Considerations

- Rich text editor library (Tiptap, Slate, or similar)
- Image CDN with resizing/optimization
- Full-text search for content
- SEO: Server-side rendering for game pages and posts
- Content versioning for edit history on guides

---

## Success Metrics

- Posts created per active user (content creation)
- Guide helpful votes (content quality)
- Organic traffic to game pages (SEO success)
- Time on site for content pages (engagement)
- Conversion from organic visitor → registered user (acquisition)

---

## Rollout Strategy

### Phase 1: Notes & Journals

- Launch with simple note/journal posting
- No guides yet (simpler editor)
- Test content creation engagement

### Phase 2: Reviews

- Add review post type
- Reviews surface on global game pages
- Test community content discovery

### Phase 3: Guides

- Full guide editor with sections, TOC
- Helpful voting system
- Guide-focused SEO push

---

## Open Questions

- Should guides support multiple authors/collaboration?
- Should there be a "featured" or "official" guide designation?
- How do we handle outdated guides for games that update?
- Should users be able to follow specific games (not own them)?
- Import existing content from Steam guides?

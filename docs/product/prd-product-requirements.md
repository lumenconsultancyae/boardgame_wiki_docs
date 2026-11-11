# Product Requirements Document (PRD) — Boardgame Wiki

## 1. Product Vision & Goals

**Boardgame Wiki** is a web application built to catalog and track played boardgames, share curated game lists with friends, generate digestible rule summaries and visual gameplay diagrams/graphs, share summaries and lists directly via WhatsApp, and track boardgame events happening across the United Arab Emirates (UAE).

### Core Objectives
1. **Track Played & Owned Games**: Maintain a structured collection of boardgames with play history, player counts, playtime, complexity ratings, and session logs.
2. **Shareable Game Collection**: Enable friends to browse and filter the game list by player count, duration, complexity weight, and game mechanics.
3. **Readable Rule Summaries & Visual Rule Graphs**: Transform dense rulebooks into concise cheat-sheets and visual node-edge diagrams (setup flows, turn phases, action trees, scoring breakdowns) that friends can understand in minutes.
4. **WhatsApp Sharing Integration**: Generate WhatsApp-ready formatted messages and one-click `wa.me` share links for individual game rule summaries, curated game lists, and UAE event invitations.
5. **UAE Boardgame Events Hub**: Track tabletop meetups, café game nights, tournaments, and conventions across Dubai, Abu Dhabi, Sharjah, and the wider UAE.

---

## 2. Target Personas & Use Cases

| Persona | Primary Need | Key Workflow |
| :--- | :--- | :--- |
| **Game Host / Collector** | Track owned/played games, prepare friends before game night, and organize group sessions. | Selects 3–4 candidate games for 5 players, generates a WhatsApp shortlist message, and shares the chosen game's rule summary & flowchart on WhatsApp. |
| **Game Night Friend / Player** | Quickly understand how a game works without reading a full rulebook. | Opens the shared game page or WhatsApp summary to review the 1-page cheat-sheet and visual turn-sequence graph. |
| **UAE Tabletop Enthusiast** | Discover and track local boardgame events across UAE venues and communities. | Filters upcoming events in Dubai/Abu Dhabi by date and venue, marks RSVP status, and forwards the event card to a WhatsApp group. |

---

## 3. Functional Requirements

### FR-1: Boardgame Collection & Play Tracker
- **Game Catalog Entry**: Title, subtitle, cover illustration/icon, min/max/recommended players, average playtime (minutes), complexity weight (`1.0` Light to `5.0` Heavy), categories/mechanics, BGG-style rating, and ownership status (`Owned`, `Played`, `Wishlist`).
- **Collection Filtering & Search**: Instant search by title/mechanic and filters by player count (e.g., *"Supports 5 players"*), max duration (e.g., *"<= 60 min"*), and complexity tier.
- **Play Session Logging**: Record date played, location/venue, participating players, scores, winner, and session highlights.
- **Collection Analytics**: Visual statistics showing most-played games, mechanics distribution graph, complexity distribution, and win-rate summaries.

### FR-2: Rule Summaries & Visual Rule Graphs
- **Structured Rule Cheat-Sheet**:
  - *Objective / Winning Condition*: 1–2 sentence summary of how to win.
  - *Setup Summary*: Step-by-step table preparation checklist by player count.
  - *Turn Flowchart & Phases*: Ordered sequence of actions available on a player's turn.
  - *End-Game & Scoring Breakdown*: Trigger conditions and point calculation categories.
  - *Golden Rules & Edge Cases*: Common pitfalls and rule clarifications for new players.
- **Interactive Visual Rule Diagrams**:
  - Turn-phase flowcharts, resource conversion graphs, and scoring dependency diagrams rendered visually so players can follow decision trees at a glance.

### FR-3: WhatsApp Sharing Engine
- **WhatsApp Game Summary Formatter**:
  - Generates pre-formatted WhatsApp text (using `*bold*`, `_italic_`, and bullet points) containing the game's player count, duration, complexity, winning condition, turn phases, and direct link to the visual diagram.
- **WhatsApp Game List / Poll Exporter**:
  - Exports the currently filtered game list (or selected games for game night) into a numbered WhatsApp message ready for friends to vote on in a group chat.
- **WhatsApp Delivery Channels**:
  - Direct launch via `https://wa.me/?text=<encoded_message>` (mobile & WhatsApp Web compatible).
  - One-click "Copy for WhatsApp" button with instant preview.

### FR-4: UAE Boardgame Events Tracker
- **UAE Event Directory**:
  - Track events across **Dubai**, **Abu Dhabi**, **Sharjah**, and other emirates.
  - Event attributes: Title, Emirate/City, Venue Name (e.g., Unwind Specialty Boardgame Cafe, Hive Board Game Cafe, Back to Games, Dice & Beans, community hubs), Date & Time, Featured Games, Entry Fee (AED / Free), Organizer, RSVP Status (`Going`, `Interested`, `Hosting`), and External/Map Link.
- **Event Filtering & WhatsApp Invite**:
  - Filter by Emirate, event type (`Meetup`, `Tournament`, `Convention`, `Game Night`), and upcoming date range.
  - Share formatted event invite card directly to WhatsApp.

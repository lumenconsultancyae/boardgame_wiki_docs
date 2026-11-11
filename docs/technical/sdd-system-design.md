# System Design Document (SDD) — Boardgame Wiki

## 1. Multi-Repository Ecosystem Topology

The project spans two synchronized repositories under the `lumenconsultancyae` GitHub organization:

```mermaid
flowchart LR
    subgraph GitHub["lumenconsultancyae GitHub Organization"]
        AppRepo["Boardgame-Wiki (Web Application)"]
        DocsRepo["boardgame_wiki_docs (PRD, SDD, Diagrams & BDD Suite)"]
    end

    DocsRepo -->|"Specifies & Verifies"| AppRepo
```

1. **[`lumenconsultancyae/Boardgame-Wiki`](https://github.com/lumenconsultancyae/Boardgame-Wiki)**: Main web application housing the UI components, domain models, rule graph renderer, WhatsApp formatting engine, and UAE events tracker.
2. **[`lumenconsultancyae/boardgame_wiki_docs`](https://github.com/lumenconsultancyae/boardgame_wiki_docs)**: Companion specification, architectural diagrams, Docker Compose stack, and BDD E2E test suite.

---

## 2. Domain Model & Data Schema

### 2.1 Core Entities

```mermaid
erDiagram
    BOARD_GAME ||--|| RULE_SUMMARY : "has"
    BOARD_GAME ||--o{ RULE_GRAPH_NODE : "visualizes"
    BOARD_GAME ||--o{ PLAY_SESSION : "logs"
    UAE_EVENT }o--o{ BOARD_GAME : "features"

    BOARD_GAME {
        string id PK
        string slug
        string title
        int minPlayers
        int maxPlayers
        int bestPlayers
        int playTimeMinutes
        float complexityWeight
        float personalRating
        string status "Owned | Played | Wishlist"
        string[] mechanics
        string[] categories
    }

    RULE_SUMMARY {
        string gameId FK
        string objective
        string[] setupSteps
        string[] turnPhases
        string[] scoringRules
        string[] goldenRules
        string mermaidDiagram
    }

    RULE_GRAPH_NODE {
        string id PK
        string gameId FK
        string phaseType "setup | action | decision | scoring"
        string label
        string description
        string[] nextNodeIds
    }

    PLAY_SESSION {
        string id PK
        string gameId FK
        string playedAt
        string location
        string winner
        int durationMinutes
        string notes
    }

    UAE_EVENT {
        string id PK
        string title
        string emirate "Dubai | Abu Dhabi | Sharjah | Other"
        string venueName
        string address
        string startDateTime
        string eventType "Meetup | Tournament | Cafe Night | Convention"
        string entryFeeAed
        string rsvpStatus "Going | Interested | Hosting | None"
        string[] featuredGameIds
    }
```

---

## 3. WhatsApp Message Serialization Architecture

The WhatsApp Sharing service (`whatsappFormatter`) transforms domain entities into deterministic, UTF-8 encoded WhatsApp messages using WhatsApp native markdown (`*bold*`, `_italic_`, `~strikethrough~`, monospace blocks) and generates deep links (`https://wa.me/?text=...`):

1. **Single Game Summary Payload**:
   - Header: `🎲 *{title}* ({minPlayers}-{maxPlayers} Players • ⏱ {playTimeMinutes}m • ⚙️ Weight: {complexityWeight}/5)`
   - Objective: `🏆 *Goal:* {objective}`
   - Setup & Turn Flow: Bulleted step list of `setupSteps` and `turnPhases`
   - Golden Rules & Link to Interactive Rule Diagram
2. **Game List / Game Night Shortlist Payload**:
   - Header: `📚 *Boardgame Night Shortlist ({count} Games)*`
   - Filtered summary line per game with player range, playtime, complexity, and 1-line pitch
   - Voting prompt for group chat replies
3. **UAE Event Invite Payload**:
   - Header: `📅 *UAE Boardgame Event: {title}*`
   - Location (`{venueName}, {emirate}`), Date/Time, Entry Fee (AED), Featured Games, and RSVP note.

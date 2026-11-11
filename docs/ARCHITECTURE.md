# Architecture & Visual Rule Graph Design — Boardgame Wiki

## 1. High-Level System Architecture

**Boardgame Wiki** is structured into four cohesive domain modules:
1. **Collection & Play Tracker Module**: Manages the personal boardgame catalog, player/time/complexity filtering, play logs, and collection analytics graphs.
2. **Rule Summary & Visual Graph Engine**: Renders structured cheat-sheets alongside interactive turn-structure flowcharts and scoring graphs so friends can learn rules visually.
3. **WhatsApp Sharing Service**: Formats single-game rule summaries, curated game lists, and UAE event cards into WhatsApp-native markdown with instant `wa.me` deep-linking and clipboard export.
4. **UAE Boardgame Events Module**: Tracks tabletop meetups, café nights, tournaments, and conventions across Dubai, Abu Dhabi, and Sharjah.

```mermaid
flowchart TD
    User["Player / Game Night Host"] --> UI["Boardgame Wiki Web UI"]

    subgraph CoreModules["Core Application Modules"]
        Catalog["Game Collection & Play Tracker"]
        RuleEngine["Rule Summary & Visual Graph Renderer"]
        UAEHub["UAE Boardgame Events Tracker"]
        WhatsApp["WhatsApp Message Formatter & Share Engine"]
    end

    UI --> Catalog
    UI --> RuleEngine
    UI --> UAEHub
    Catalog --> WhatsApp
    RuleEngine --> WhatsApp
    UAEHub --> WhatsApp
    WhatsApp -->|"https://wa.me/?text=..."| Friends["Friends & WhatsApp Game Night Group"]
```

---

## 2. Visual Rule Graph Architecture (Example Rule Flowcharts)

Each boardgame entry includes a structured **Rule Graph** representing Setup, Turn Phases, Decision Branches, and End-Game Scoring. Below is an example of how a game's turn and scoring flow is modeled visually for friends:

```mermaid
flowchart LR
    Setup["1. Setup Board & Deal Starting Cards"] --> Phase1["2. Resource Production / Draw Phase"]
    Phase1 --> Decision{"3. Player Action Choice"}
    Decision -->|"Trade / Market"| ActionA["Exchange Resources with Bank or Players"]
    Decision -->|"Build / Expand"| ActionB["Place Roads, Settlements, or Upgrades"]
    Decision -->|"Develop / Card"| ActionC["Acquire Development or Bonus Card"]
    ActionA --> CheckVP{"Victory Threshold Reached?"}
    ActionB --> CheckVP
    ActionC --> CheckVP
    CheckVP -->|"No"| NextTurn["Pass Turn Clockwise"]
    NextTurn --> Phase1
    CheckVP -->|"Yes"| Scoring["Final Scoring & Winner Declared"]
```

---

## 3. Collection & Play Analytics Graphs

In addition to per-game rule flowcharts, the application generates collection-level graphs:
- **Complexity vs. Playtime Scatter/Distribution Graph**: Helps groups pick between quick filler games (`15–30 min`, weight `< 2.0`) and deep strategy games (`90–150 min`, weight `> 3.0`).
- **Player Count Coverage Matrix**: Shows which games scale best for 2, 3, 4, 5, or 6+ players.
- **Play Session Frequency & Win Breakdown**: Tracks most-played titles and session history across game nights.

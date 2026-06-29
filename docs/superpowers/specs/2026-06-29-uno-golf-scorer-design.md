# Uno Golf Scorer — Design Spec

## Overview

A single-file HTML scorekeeping web app for the Mattel card game **Uno Golf**. Players enter names, then track their scores across 9 rounds on a digital golf scorecard styled with UNO card design language. Built as a zero-dependency HTML/CSS/JS single-page app with localStorage persistence.

## Tech Stack

- **One file:** `index.html` — no build step, no dependencies, no frameworks
- **Storage:** `localStorage` — auto-saves game state, prompts to resume on load
- **Hosting:** Works as a local file or dropped on any static host

## Color Palette

| Usage | Hex | Notes |
|-------|-----|-------|
| Background / card surfaces | `#c4bbb3` | Warm beige |
| Header / primary accent | `#53775f` | Forest green — grounds the page |
| Secondary / buttons | `#6fa067` | Brighter green |
| Text / dark elements | `#262e31` | Dark charcoal |
| Red (penalties / danger) | `#b42d2a` | Classic UNO red |
| Olive / dividers | `#78953a` | Subdued green tone |
| Steel blue accents | `#8cabd1` | Interactive highlights |
| Gold highlights | `#af9d47` | Accents / leader flair |

## Typography

- **Heads-up display / logos:** A bold, chunky sans-serif (system UI or a bold web font like Bebas Neue or similar)
- **Body / scores:** Clean system sans-serif
- Scores shown in oversized bold type in white rounded-rect "card" cells

## User Flow

### Screen 1 — Player Setup
- App title "UNO GOLF SCORER" in a UNO-style oval badge (green fill, white bold text)
- 8 name input fields (2 visible by default, "Add Player" button reveals more, max 8)
- "Start Game" button
- "Continue last game?" prompt on page load if saved state exists

### Screen 2 — Scorecard (game in progress)
- Header bar with UNO oval badge, "Round X of 9", and round progress dots
- Scorecard table:
  - Rows = players, columns = rounds 1–9 + TOTAL
  - Column labels are round numbers (not "hole" labels — keep it simple)
  - Current round column is highlighted
  - Each score cell: white rounded rect with shadow (mini UNO card), bold number
  - Leader row gets a subtle green glow/tint on the total column
- Score entry: below the current round column, an input per player + "Record Scores" button
- "End Game" button in header or footer to finish early

### Screen 3 — Final Results
- Ranked list of players with final total scores
- Winner highlighted in gold with a trophy/flag icon
- "New Game" button

## Responsive / Mobile

- **No separate mobile layout** — the scorecard is the same design on all screens
- On narrow viewports (<600px): the scorecard table scrolls horizontally with a sticky player-name column so names stay visible while scores scroll
- Score entry inputs stack vertically below the table on small screens (instead of a column-row layout)
- Setup screen inputs stack vertically by default at all sizes
- Buttons and text scale naturally with viewport — no hardcoded pixel sizes that break on small screens
- Touch-friendly tap targets (min 44px) on all interactive elements

## Background — Flowing SVG Curves

Inspired by the Uno Golf box art: sweeping bezier curves in `#6fa067` and `#53775f` (at low opacity) flow across the background behind the scorecard. These are large-scale paths — no small patterns, no repetition — just 2–3 elegant curves that give the page atmosphere and echo the box design without competing with the scorecard.

- Color: `#6fa067` at ~8% opacity and `#53775f` at ~6% opacity
- Positioned behind the content area, spanning full viewport
- Generated as inline SVG in the HTML file (no external assets)
- Fixed position so they scroll with the page

## Visual Design Language

- **UNO card cells:** Scores sit in white rounded-rectangle cards (border-radius ~8px) with a soft drop shadow
- **Grid:** Alternating row shading (very subtle) for readability, like a real scorecard
- **Background:** Sweeping bezier curves in the two greens (inspired by the product box)
- **Golf touches:** Green header, flag icons, leader indicator
- **UNO touches:** Red accents on penalties, card-stack corner decoration, bold typography
- **Overall:** Clean, retro, simple — bold colors on warm off-white background

## Data Model

```js
{
  players: [
    { name: "Alice", scores: [25, 18, 30, null, null, null, null, null, null] }
  ],
  currentRound: 2,
  gameActive: true
}
```

- `null` = not yet played
- `scores` array length = 9 (one per round)

## Persistence

- On every "Record Scores" action: serialize state to `localStorage` key `unoGolfScorer`
- On page load: detect saved state, offer "Continue" or "New Game"
- On "End Game" or final round completion: show results, clear saved state

## Edge Cases

- **Browser reload mid-game:** Restored via localStorage — current round, all scores intact
- **Accidental score entry:** Each round's scores can be edited until "Record Scores" is confirmed (clicking the same round again allows re-entry)
- **Browser without localStorage:** Falls back to session-only mode — warn user scores won't persist
- **Empty player name:** Disallow starting game with unnamed players
- **All 9 rounds complete:** Auto-transition to final results screen
- **0 players / 1 player:** Min 2 players enforced at setup

## Non-Goals (explicitly out of scope)

- Tracking individual cards, grid state, or game board
- Rules enforcement or validation of scores
- Network / multiplayer / cloud sync
- Theme switching (light/dark)
- Animations beyond basic transitions

---

*Design approved: 2026-06-29*

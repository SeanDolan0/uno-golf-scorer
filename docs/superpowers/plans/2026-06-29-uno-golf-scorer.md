# Uno Golf Scorer — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a single-file HTML scorekeeping web app for Uno Golf with a golf-scorecard × UNO-card aesthetic, localStorage persistence, and inline SVG background curves.

**Architecture:** Single `index.html` file containing all HTML, CSS, and JavaScript. Three screen areas (setup, scorecard, results) toggled by CSS class. Game state stored as a plain JS object serialized to localStorage. SVG bezier curves rendered inline in the background as a fixed-position layer.

**Tech Stack:** Vanilla HTML5 / CSS3 / JavaScript (ES6). Zero dependencies, zero build step. `localStorage` for persistence.

## Global Constraints

- The only file is `index.html` at the project root — no other files, no build step, no frameworks
- Colors (exact hex): `#c4bbb3` (bg), `#53775f` (header/accent), `#6fa067` (secondary), `#262e31` (text), `#b42d2a` (red/danger), `#78953a` (olive/dividers), `#8cabd1` (steel blue/interactive), `#af9d47` (gold/highlights)
- localStorage key must be `unoGolfScorer`
- Data model: `{ players: [{ name: "Alice", scores: [25, 18, 30, null, null, null, null, null, null] }], currentRound: 1 (1-indexed), gameActive: true }`
- Max 8 players, min 2 players, exactly 9 rounds per game
- `null` in scores means not yet played
- No external assets — fonts use system sans-serif only
- For agentic workers: Always add `Co-Authored-By: Claude Opus 4.8 (1M context) <noreply@anthropic.com>` to git commit messages

---

## File Structure

```
UnoGolfScorer/
└── index.html          ← the entire application
```

`index.html` has these logical sections, listed top-to-bottom:

| Section | Lines | Responsibility |
|---------|-------|----------------|
| HTML `<head>` | 1–40 | DOCTYPE, meta viewport, title, all CSS in `<style>` |
| CSS: Variables / Reset | — | Custom properties for all 8 colors, box-sizing reset, body defaults |
| CSS: SVG Background | — | Fixed-position SVG container with flowing bezier curves |
| CSS: Shared Layout | — | Container width, screen show/hide via `.screen--active` |
| CSS: Setup Screen | — | UNO oval badge, name inputs, add/start buttons |
| CSS: Scorecard Screen | — | Header bar, scorecard table, UNO card cells, score entry |
| CSS: Results Screen | — | Ranked list, winner highlight, trophy icon |
| CSS: Mobile | — | `@media (max-width: 599px)` horizontal scroll, sticky column |
| HTML: SVG Background | — | Inline SVG element with `<path>` bezier curves |
| HTML: Screen 1 (Setup) | — | UNO badge, name input fields, Add Player / Start Game |
| HTML: Screen 2 (Scorecard) | — | Round header, scorecard table, score entry form |
| HTML: Screen 3 (Results) | — | Ranked list container, New Game button |
| HTML: Script `<script>` | — | All application JS |

---

### Task 1: HTML Scaffold + CSS Foundation

**Files:**
- Create: `index.html`

**Interfaces:**
- Consumes: Nothing (first task)
- Produces: The skeleton HTML file with all three screen containers, CSS variable system, base typography, screen show/hide system that later tasks populate

- [ ] **Step 1: Write the complete scaffold**

Write `C:\Users\sedol\Documents\UnoGolfScorer\index.html` with the full HTML structure, empty screen divs, CSS variables, reset styles, and shared layout:

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Uno Golf Scorer</title>
<style>
  /* =========================================================
     CSS Custom Properties (Color Palette)
     ========================================================= */
  :root {
    --bg: #c4bbb3;
    --header: #53775f;
    --accent: #6fa067;
    --text: #262e31;
    --red: #b42d2a;
    --olive: #78953a;
    --steel: #8cabd1;
    --gold: #af9d47;
    --card-bg: #ffffff;
    --card-radius: 8px;
    --shadow: 0 2px 6px rgba(0,0,0,0.15);
    --font-display: system-ui, -apple-system, 'Segoe UI', Roboto, sans-serif;
    --font-body: system-ui, -apple-system, 'Segoe UI', Roboto, sans-serif;
  }

  /* =========================================================
     Reset & Base
     ========================================================= */
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  html { font-size: 16px; }

  body {
    font-family: var(--font-body);
    background: var(--bg);
    color: var(--text);
    min-height: 100vh;
    overflow-x: hidden;
  }

  /* =========================================================
     Shared Layout
     ========================================================= */
  .container {
    max-width: 900px;
    margin: 0 auto;
    padding: 20px;
    position: relative;
    z-index: 1;
  }

  /* Screen visibility — only the active screen shows */
  .screen { display: none; }
  .screen--active { display: block; }

  /* =========================================================
     Screen 1 — Player Setup (placeholder)
     ========================================================= */
  /* Filled in Task 3 */

  /* =========================================================
     Screen 2 — Scorecard (placeholder)
     ========================================================= */
  /* Filled in Task 5 */

  /* =========================================================
     Screen 3 — Results (placeholder)
     ========================================================= */
  /* Filled in Task 6 */

  /* =========================================================
     SVG Background (placeholder)
     ========================================================= */
  /* Filled in Task 2 */

  /* =========================================================
     Mobile Responsive (placeholder)
     ========================================================= */
  /* Filled in Task 7 */
</style>
</head>
<body>
  <!-- SVG background container (filled in Task 2) -->
  <div id="bg-svg-container"></div>

  <div class="container">
    <!-- Screen 1: Player Setup -->
    <div id="screen-setup" class="screen screen--active">
      <!-- Filled in Task 3 -->
    </div>

    <!-- Screen 2: Scorecard -->
    <div id="screen-scorecard" class="screen">
      <!-- Filled in Task 5 -->
    </div>

    <!-- Screen 3: Results -->
    <div id="screen-results" class="screen">
      <!-- Filled in Task 6 -->
    </div>
  </div>

  <script>
    // =========================================================
    // App State (filled in Task 4)
    // =========================================================
  </script>
</body>
</html>
```

- [ ] **Step 2: Open in browser and verify**

Open `index.html` in Chrome. Expected: A blank beige (`#c4bbb3`) page with no visible content. The setup screen is `display: block` but empty. No console errors.

- [ ] **Step 3: Commit**

```bash
git -C "C:\Users\sedol\Documents\UnoGolfScorer" init
git -C "C:\Users\sedol\Documents\UnoGolfScorer" add index.html
git -C "C:\Users\sedol\Documents\UnoGolfScorer" commit -m "feat: scaffold index.html with CSS variables, reset, and three screen containers"
```

---

### Task 2: SVG Background Curves

**Files:**
- Modify: `index.html` — add inline SVG bezier curves and their CSS

**Interfaces:**
- Consumes: Task 1's scaffold (the `#bg-svg-container` div and the SVG background CSS placeholder)
- Produces: Visible flowing bezier curves in #6fa067 (~8% opacity) and #53775f (~6% opacity) behind the content area

- [ ] **Step 1: Add SVG background CSS + SVG markup**

Replace the SVG background placeholder comments with actual content.

After `<!-- SVG background container -->` line, add the SVG element:

```html
  <!-- SVG background container -->
  <svg class="bg-curves" viewBox="0 0 1440 900" preserveAspectRatio="xMidYMid slice" xmlns="http://www.w3.org/2000/svg" aria-hidden="true">
    <defs>
      <linearGradient id="g1" x1="0%" y1="0%" x2="100%" y2="100%">
        <stop offset="0%" stop-color="#6fa067" stop-opacity="0.08"/>
        <stop offset="100%" stop-color="#6fa067" stop-opacity="0.02"/>
      </linearGradient>
      <linearGradient id="g2" x1="100%" y1="0%" x2="0%" y2="100%">
        <stop offset="0%" stop-color="#53775f" stop-opacity="0.06"/>
        <stop offset="100%" stop-color="#53775f" stop-opacity="0.01"/>
      </linearGradient>
    </defs>
    <!-- Curve 1 — large sweep from top-left -->
    <path d="M0,200 C300,400 400,100 720,350 C1040,600 1200,200 1440,400 L1440,0 L0,0 Z"
          fill="url(#g1)" />
    <!-- Curve 2 — mid-right sweep -->
    <path d="M1440,600 C1100,400 900,700 600,450 C300,200 150,500 0,300 L0,900 L1440,900 Z"
          fill="url(#g2)" />
    <!-- Curve 3 — lower left accent -->
    <path d="M0,700 C250,550 500,800 800,600 C1100,400 1300,650 1440,550 L1440,900 L0,900 Z"
          fill="url(#g1)" />
    <!-- Curve 4 — subtle full-height sweep -->
    <path d="M200,0 C400,300 200,600 500,900 L0,900 L0,0 Z"
          fill="url(#g2)" />
  </svg>
```

Replace the SVG Background CSS placeholder with:

```css
  /* =========================================================
     SVG Background — Flowing Bezier Curves
     ========================================================= */
  .bg-curves {
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    z-index: 0;
    pointer-events: none;
    overflow: hidden;
  }
```

- [ ] **Step 2: Open in browser and verify**

Open `index.html`. Expected: Flowing green bezier curves visible behind the content area. Curves in two green tones (`#6fa067` and `#53775f`) at low opacity, spanning the full viewport. Content area remains readable on top (z-index: 1).

- [ ] **Step 3: Commit**

```bash
git -C "C:\Users\sedol\Documents\UnoGolfScorer" add index.html
git -C "C:\Users\sedol\Documents\UnoGolfScorer" commit -m "feat: add flowing SVG bezier curves to background"
```

---

### Task 3: Player Setup Screen

**Files:**
- Modify: `index.html` — populate the setup screen HTML, CSS, and JS

**Interfaces:**
- Consumes: Task 1's scaffold (setup screen container), Task 2 (SVG background)
- Produces: A working setup screen where users enter 2–8 player names and click "Start Game"
  - HTML output: name input fields + buttons live in `#screen-setup`
  - JS output: `startGame(playerNames)` function called when form is valid

- [ ] **Step 1: Add setup screen HTML**

Replace the `<!-- Screen 1: Player Setup -->` placeholder content inside `#screen-setup`:

```html
    <!-- Screen 1: Player Setup -->
    <div id="screen-setup" class="screen screen--active">
      <div class="setup-container">
        <!-- UNO-style oval badge -->
        <div class="uno-badge">
          <span class="uno-badge__text">UNO GOLF SCORER</span>
        </div>

        <p class="setup-subtitle">Enter player names</p>

        <div id="player-inputs" class="player-inputs">
          <!-- Player inputs injected by JS -->
        </div>

        <div class="setup-actions">
          <button id="btn-add-player" class="btn btn--secondary" disabled>+ Add Player</button>
          <button id="btn-start-game" class="btn btn--primary" disabled>Start Game</button>
        </div>

        <p id="setup-error" class="setup-error"></p>
      </div>
    </div>
```

- [ ] **Step 2: Add setup screen CSS**

Replace the `/* Screen 1 — Player Setup */` CSS placeholder:

```css
  /* =========================================================
     Screen 1 — Player Setup
     ========================================================= */
  .setup-container {
    max-width: 500px;
    margin: 40px auto;
    text-align: center;
  }

  /* UNO oval badge */
  .uno-badge {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    background: var(--header);
    border: 3px solid #fff;
    border-radius: 60px / 40px;
    padding: 14px 36px;
    margin-bottom: 20px;
    box-shadow: var(--shadow);
  }

  .uno-badge__text {
    font-family: var(--font-display);
    font-weight: 900;
    font-size: 1.5rem;
    letter-spacing: 0.08em;
    color: #fff;
    text-transform: uppercase;
  }

  .setup-subtitle {
    font-size: 1.1rem;
    color: var(--text);
    margin-bottom: 24px;
    opacity: 0.8;
  }

  .player-inputs {
    display: flex;
    flex-direction: column;
    gap: 12px;
    margin-bottom: 20px;
  }

  .player-input-wrap {
    display: flex;
    align-items: center;
    gap: 10px;
  }

  .player-input-wrap label {
    font-weight: 700;
    font-size: 0.85rem;
    min-width: 44px;
    color: var(--header);
  }

  .player-input-wrap input {
    flex: 1;
    padding: 10px 12px;
    font-size: 1rem;
    border: 2px solid var(--olive);
    border-radius: 6px;
    background: #faf8f6;
    color: var(--text);
    outline: none;
    font-family: var(--font-body);
    transition: border-color 0.15s;
  }

  .player-input-wrap input:focus {
    border-color: var(--steel);
    box-shadow: 0 0 0 3px rgba(140, 171, 209, 0.3);
  }

  .player-input-wrap .btn-remove {
    background: none;
    border: none;
    color: var(--red);
    font-size: 1.3rem;
    cursor: pointer;
    width: 44px;
    height: 44px;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 50%;
    transition: background 0.15s;
  }

  .player-input-wrap .btn-remove:hover {
    background: rgba(180, 45, 42, 0.1);
  }

  .player-input-wrap .btn-remove:disabled {
    opacity: 0.2;
    cursor: default;
  }

  .setup-actions {
    display: flex;
    gap: 12px;
    justify-content: center;
    flex-wrap: wrap;
  }

  /* Buttons */
  .btn {
    padding: 12px 28px;
    font-size: 1rem;
    font-weight: 700;
    border: none;
    border-radius: 8px;
    cursor: pointer;
    font-family: var(--font-body);
    transition: background 0.15s, transform 0.1s, opacity 0.15s;
    min-height: 44px;
  }

  .btn:active { transform: scale(0.97); }

  .btn--primary {
    background: var(--accent);
    color: #fff;
  }

  .btn--primary:hover:not(:disabled) {
    background: #5d8a56;
  }

  .btn--secondary {
    background: var(--steel);
    color: #fff;
  }

  .btn--secondary:hover:not(:disabled) {
    background: #7a9abc;
  }

  .btn:disabled {
    opacity: 0.4;
    cursor: not-allowed;
  }

  .setup-error {
    margin-top: 16px;
    color: var(--red);
    font-weight: 600;
    font-size: 0.9rem;
    min-height: 1.4em;
  }
```

- [ ] **Step 3: Add setup screen JavaScript**

Inside the `<script>` block, replace the placeholder comment with the game state and setup logic:

```js
    // =========================================================
    // Game State
    // =========================================================
    let state = {
      players: [],
      currentRound: 1,
      gameActive: false
    };

    const STORAGE_KEY = 'unoGolfScorer';
    const MAX_PLAYERS = 8;
    const MIN_PLAYERS = 2;
    const TOTAL_ROUNDS = 9;

    // =========================================================
    // DOM References
    // =========================================================
    const $ = id => document.getElementById(id);
    const screenSetup   = $('screen-setup');
    const screenScorecard = $('screen-scorecard');
    const screenResults = $('screen-results');
    const playerInputs  = $('player-inputs');
    const btnAddPlayer  = $('btn-add-player');
    const btnStartGame  = $('btn-start-game');
    const setupError    = $('setup-error');

    // =========================================================
    // Player Setup
    // =========================================================
    let playerCount = 0;

    function addPlayerInput(name) {
      if (playerCount >= MAX_PLAYERS) return;
      const idx = playerCount;
      const wrap = document.createElement('div');
      wrap.className = 'player-input-wrap';
      wrap.dataset.index = idx;
      wrap.innerHTML = `
        <label for="pname-${idx}">#${idx + 1}</label>
        <input type="text" id="pname-${idx}" class="pname-input"
               placeholder="Player ${idx + 1}" maxlength="20"
               value="${name || ''}" autocomplete="off">
        <button class="btn-remove" data-index="${idx}" title="Remove player">&times;</button>
      `;
      playerInputs.appendChild(wrap);
      playerCount++;

      // Wire up remove
      wrap.querySelector('.btn-remove').addEventListener('click', () => {
        removePlayerInput(idx);
      });

      // Wire up input change to re-validate
      wrap.querySelector('.pname-input').addEventListener('input', updateStartButton);

      updateAddButton();
      updateStartButton();
    }

    function removePlayerInput(index) {
      const wraps = playerInputs.querySelectorAll('.player-input-wrap');
      if (wraps.length <= MIN_PLAYERS) return; // keep at least 2
      const wrap = playerInputs.querySelector(`[data-index="${index}"]`);
      if (wrap) {
        wrap.remove();
        playerCount--;
        // Re-index remaining inputs so labels stay sequential
        reindexInputs();
        updateAddButton();
        updateStartButton();
      }
    }

    function reindexInputs() {
      const wraps = playerInputs.querySelectorAll('.player-input-wrap');
      wraps.forEach((wrap, i) => {
        wrap.dataset.index = i;
        wrap.querySelector('label').textContent = `#${i + 1}`;
        const input = wrap.querySelector('.pname-input');
        input.id = `pname-${i}`;
        input.placeholder = `Player ${i + 1}`;
        wrap.querySelector('.btn-remove').dataset.index = i;
      });
      playerCount = wraps.length;
    }

    function updateAddButton() {
      btnAddPlayer.disabled = playerCount >= MAX_PLAYERS;
    }

    function updateStartButton() {
      const names = getPlayerNames();
      const valid = names.length >= MIN_PLAYERS && names.every(n => n.trim().length > 0);
      btnStartGame.disabled = !valid;
      setupError.textContent = '';
    }

    function getPlayerNames() {
      const inputs = playerInputs.querySelectorAll('.pname-input');
      return Array.from(inputs).map(inp => inp.value.trim()).filter(n => n.length > 0);
    }

    function validateSetup() {
      const names = getPlayerNames();
      if (names.length < MIN_PLAYERS) {
        setupError.textContent = `At least ${MIN_PLAYERS} players are required.`;
        return null;
      }
      const empty = Array.from(playerInputs.querySelectorAll('.pname-input'))
                        .some(inp => inp.value.trim().length === 0);
      if (empty) {
        setupError.textContent = 'All player names must be filled in.';
        return null;
      }
      setupError.textContent = '';
      return names;
    }

    // Initialize with 2 empty inputs
    addPlayerInput('');
    addPlayerInput('');

    btnAddPlayer.addEventListener('click', () => {
      addPlayerInput('');
      // Focus the new input
      const inputs = playerInputs.querySelectorAll('.pname-input');
      inputs[inputs.length - 1].focus();
    });

    btnStartGame.addEventListener('click', () => {
      const names = validateSetup();
      if (!names) return;
      startNewGame(names);
    });
```

- [ ] **Step 4: Open in browser and verify**

Open `index.html`. Expected:
- Page shows the UNO-style oval badge with "UNO GOLF SCORER"
- Two name input fields labeled "#1" and "#2"
- "Start Game" button is disabled until both inputs have text
- "+ Add Player" button is enabled; clicking it adds more inputs (up to 8)
- Remove (×) buttons remove inputs but keep at least 2
- Typing in inputs enables/disables "Start Game" correctly
- Empty names show validation error

- [ ] **Step 5: Commit**

```bash
git -C "C:\Users\sedol\Documents\UnoGolfScorer" add index.html
git -C "C:\Users\sedol\Documents\UnoGolfScorer" commit -m "feat: add player setup screen with UNO badge, name inputs, validation"
```

---

### Task 4: Game State + localStorage Persistence

**Files:**
- Modify: `index.html` — add `startNewGame()`, `showScreen()`, localStorage save/load/resume logic

**Interfaces:**
- Consumes: Task 3's `state` object, `screenSetup`, `screenScorecard`, `screenResults` DOM refs
- Produces:
  - `startNewGame(names)` — initializes state, hides setup, shows scorecard, saves
  - `saveGame()` — serializes state to localStorage
  - `loadSavedGame()` — deserializes from localStorage, returns state or null
  - `clearSavedGame()` — removes from localStorage
  - `showScreen(id)` — toggles `.screen--active` between the three screens
  - On page load: checks for saved state, shows resume prompt if found

- [ ] **Step 1: Add screen management and game state functions**

After `const TOTAL_ROUNDS = 9;` and the DOM references block, add:

```js
    // =========================================================
    // Screen Management
    // =========================================================
    function showScreen(id) {
      [screenSetup, screenScorecard, screenResults].forEach(el => {
        el.classList.toggle('screen--active', el.id === id);
      });
    }

    // =========================================================
    // Game Lifecycle
    // =========================================================
    function startNewGame(names) {
      state = {
        players: names.map(name => ({
          name: name,
          scores: Array(TOTAL_ROUNDS).fill(null)
        })),
        currentRound: 1,
        gameActive: true
      };
      saveGame();
      renderScorecard();
      showScreen('screen-scorecard');
    }

    function endGame() {
      state.gameActive = false;
      clearSavedGame();
      showScreen('screen-results');
      renderResults();
    }

    // =========================================================
    // localStorage Persistence
    // =========================================================
    function saveGame() {
      try {
        localStorage.setItem(STORAGE_KEY, JSON.stringify(state));
      } catch (e) {
        // localStorage full or unavailable — fail silently
      }
    }

    function loadSavedGame() {
      try {
        const raw = localStorage.getItem(STORAGE_KEY);
        if (!raw) return null;
        const parsed = JSON.parse(raw);
        // Basic validation
        if (!parsed || !parsed.players || !parsed.currentRound || !parsed.gameActive) {
          return null;
        }
        return parsed;
      } catch (e) {
        return null;
      }
    }

    function clearSavedGame() {
      try {
        localStorage.removeItem(STORAGE_KEY);
      } catch (e) { /* ignore */ }
    }

    // Check localStorage warning
    try {
      localStorage.getItem('__test__');
    } catch (e) {
      console.warn('localStorage is not available. Scores will not persist across page reloads.');
    }
```

- [ ] **Step 2: Add resume-prompt logic on page load**

After the localStorage check, add:

```js
    // =========================================================
    // On Load — Check for Saved Game
    // =========================================================
    (function init() {
      const saved = loadSavedGame();
      if (saved) {
        // Prompt user to resume or start fresh
        const resume = confirm(
          `You have an unfinished game in progress (Round ${saved.currentRound} of ${TOTAL_ROUNDS}).\n\n` +
          'Press OK to continue where you left off, or Cancel to start a new game.'
        );
        if (resume) {
          state = saved;
          renderScorecard();
          showScreen('screen-scorecard');
        } else {
          clearSavedGame();
        }
      }
    })();
```

Also wire `startNewGame` into the setup flow by replacing the placeholder call in `btnStartGame` handler (already done in Task 3 step 3).

- [ ] **Step 3: Open in browser and verify**

Open `index.html`. Expected:
- Two-player inputs shown (setup screen)
- Enter names "Alice" and "Bob" and click Start Game
- (Scorecard render is placeholder — but the screen should switch)
- Reload the page — expected: a `confirm()` dialog "You have an unfinished game..."
- Click "Cancel" → setup screen shown fresh
- Enter names and start again → click "Cancel" on reload → works

- [ ] **Step 4: Commit**

```bash
git -C "C:\Users\sedol\Documents\UnoGolfScorer" add index.html
git -C "C:\Users\sedol\Documents\UnoGolfScorer" commit -m "feat: add game state, localStorage persistence, resume prompt"
```

---

### Task 5: Scorecard Screen

**Files:**
- Modify: `index.html` — add the full scorecard: header, table, score entry, all CSS, render + event functions

**Interfaces:**
- Consumes: Task 4's `state`, `showScreen()`, `saveGame()`, `endGame()`, DOM refs `screenScorecard`
- Produces: `renderScorecard()` — renders the full scorecard UI, updates on score entry
  - Calls `saveGame()` after every "Record Scores" action
  - Calls `endGame()` when round 9 scores are recorded
  - Requires `renderResults()` (Task 6) to be defined before round-9 auto-transition

- [ ] **Step 1: Add scorecard HTML**

Replace the `<!-- Screen 2: Scorecard -->` placeholder:

```html
    <!-- Screen 2: Scorecard -->
    <div id="screen-scorecard" class="screen">
      <!-- Header -->
      <header class="scorecard-header">
        <div class="scorecard-header__left">
          <div class="uno-badge uno-badge--small">
            <span class="uno-badge__text">UNO GOLF</span>
          </div>
        </div>
        <div class="scorecard-header__center">
          <h2 id="round-title" class="round-title">Round 1 of 9</h2>
          <div id="round-dots" class="round-dots"></div>
        </div>
        <div class="scorecard-header__right">
          <button id="btn-end-game" class="btn btn--danger">End Game</button>
        </div>
      </header>

      <!-- Scorecard Table -->
      <div class="scorecard-table-wrap">
        <table id="scorecard-table" class="scorecard-table">
          <thead id="scorecard-thead"></thead>
          <tbody id="scorecard-tbody"></tbody>
        </table>
      </div>

      <!-- Score Entry -->
      <div id="score-entry" class="score-entry">
        <h3 class="score-entry__title">Enter Round <span id="entry-round-num">1</span> Scores</h3>
        <div id="score-entry-inputs" class="score-entry__inputs"></div>
        <button id="btn-record-scores" class="btn btn--primary btn--record">Record Scores</button>
        <p id="entry-error" class="setup-error"></p>
      </div>
    </div>
```

- [ ] **Step 2: Add scorecard CSS**

Replace the `/* Screen 2 — Scorecard */` placeholder:

```css
  /* =========================================================
     Screen 2 — Scorecard
     ========================================================= */
  .scorecard-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 12px;
    background: var(--header);
    border-radius: 12px;
    padding: 12px 20px;
    margin-bottom: 20px;
    box-shadow: var(--shadow);
  }

  .uno-badge--small {
    padding: 6px 16px;
    border-width: 2px;
    border-radius: 40px / 28px;
    margin-bottom: 0;
  }

  .uno-badge--small .uno-badge__text {
    font-size: 0.85rem;
    letter-spacing: 0.06em;
  }

  .scorecard-header__center {
    text-align: center;
    flex: 1;
  }

  .round-title {
    font-family: var(--font-display);
    font-weight: 800;
    font-size: 1.2rem;
    color: #fff;
    margin-bottom: 6px;
  }

  .round-dots {
    display: flex;
    gap: 6px;
    justify-content: center;
  }

  .round-dot {
    width: 10px;
    height: 10px;
    border-radius: 50%;
    background: rgba(255,255,255,0.3);
    transition: background 0.2s;
  }

  .round-dot--active {
    background: #fff;
  }

  .round-dot--past {
    background: rgba(255,255,255,0.6);
  }

  .btn--danger {
    background: var(--red);
    color: #fff;
  }

  .btn--danger:hover {
    background: #9e2826;
  }

  /* =========================================================
     Scorecard Table
     ========================================================= */
  .scorecard-table-wrap {
    overflow-x: auto;
    margin-bottom: 24px;
    border-radius: 8px;
  }

  .scorecard-table {
    width: 100%;
    border-collapse: separate;
    border-spacing: 0;
    background: transparent;
    min-width: 600px;
  }

  .scorecard-table th,
  .scorecard-table td {
    padding: 10px 8px;
    text-align: center;
    white-space: nowrap;
  }

  .scorecard-table thead th {
    font-family: var(--font-display);
    font-weight: 800;
    font-size: 0.8rem;
    text-transform: uppercase;
    letter-spacing: 0.04em;
    color: var(--text);
    background: rgba(196, 187, 179, 0.6);
    border-bottom: 2px solid var(--olive);
    position: sticky;
    top: 0;
    z-index: 2;
  }

  .scorecard-table thead th:first-child {
    text-align: left;
    padding-left: 12px;
  }

  /* player name cell (first column) */
  .scorecard-table tbody td:first-child {
    font-weight: 700;
    text-align: left;
    padding-left: 12px;
    color: var(--text);
    background: var(--bg);
    position: sticky;
    left: 0;
    z-index: 1;
  }

  /* Alternate row shading */
  .scorecard-table tbody tr:nth-child(even) td {
    background-color: rgba(255,255,255,0.35);
  }
  .scorecard-table tbody tr:nth-child(even) td:first-child {
    background-color: rgba(196, 187, 179, 0.85);
  }

  /* UNO card score cells */
  .score-cell {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    width: 44px;
    height: 44px;
    background: var(--card-bg);
    border-radius: var(--card-radius);
    box-shadow: var(--shadow);
    font-weight: 800;
    font-size: 1.05rem;
    color: var(--text);
    transition: transform 0.15s, box-shadow 0.15s;
  }

  .score-cell:hover {
    transform: translateY(-1px);
    box-shadow: 0 4px 10px rgba(0,0,0,0.18);
  }

  /* Current round column highlight */
  .scorecard-table .col-current .score-cell {
    border: 2px solid var(--steel);
    box-shadow: 0 0 0 3px rgba(140, 171, 209, 0.25);
  }

  /* Total column */
  .scorecard-table td:last-child {
    font-weight: 800;
    font-size: 1.1rem;
    border-left: 2px solid var(--olive);
    background: rgba(175, 157, 71, 0.08);
  }

  .scorecard-table tbody tr:nth-child(even) td:last-child {
    background: rgba(175, 157, 71, 0.12);
  }

  /* Leader glow on total */
  .leader-glow td:last-child {
    color: var(--header);
    position: relative;
  }

  .leader-glow td:last-child::after {
    content: '';
    position: absolute;
    inset: 0;
    background: radial-gradient(ellipse at center, rgba(83, 119, 95, 0.15), transparent 70%);
    pointer-events: none;
  }

  /* =========================================================
     Score Entry Section
     ========================================================= */
  .score-entry {
    background: rgba(255,255,255,0.5);
    border-radius: 12px;
    padding: 20px;
    box-shadow: var(--shadow);
  }

  .score-entry__title {
    font-family: var(--font-display);
    font-weight: 800;
    font-size: 1rem;
    color: var(--header);
    margin-bottom: 16px;
    text-align: center;
  }

  .score-entry__inputs {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(140px, 1fr));
    gap: 12px;
    margin-bottom: 16px;
  }

  .score-entry__input-group {
    display: flex;
    flex-direction: column;
    gap: 4px;
  }

  .score-entry__input-group label {
    font-size: 0.8rem;
    font-weight: 700;
    color: var(--text);
    opacity: 0.7;
  }

  .score-entry__input-group input {
    padding: 10px 12px;
    font-size: 1rem;
    font-weight: 700;
    border: 2px solid var(--olive);
    border-radius: 6px;
    background: var(--card-bg);
    color: var(--text);
    outline: none;
    width: 100%;
    font-family: var(--font-body);
    transition: border-color 0.15s;
  }

  .score-entry__input-group input:focus {
    border-color: var(--steel);
    box-shadow: 0 0 0 3px rgba(140, 171, 209, 0.3);
  }

  .btn--record {
    display: block;
    margin: 0 auto;
    min-width: 180px;
  }
```

- [ ] **Step 3: Add scorecard JavaScript**

After the `endGame()` function from Task 4, add:

```js
    // =========================================================
    // DOM Refs — Scorecard
    // =========================================================
    const scorecardThead = $('scorecard-thead');
    const scorecardTbody = $('scorecard-tbody');
    const roundTitle     = $('round-title');
    const roundDots      = $('round-dots');
    const entryRoundNum  = $('entry-round-num');
    const scoreEntryInputs = $('score-entry-inputs');
    const btnRecordScores = $('btn-record-scores');
    const entryError     = $('entry-error');
    const btnEndGame     = $('btn-end-game');

    // =========================================================
    // Render Scorecard
    // =========================================================
    function renderScorecard() {
      if (!state.players || state.players.length === 0) return;

      // Update round title
      roundTitle.textContent = `Round ${state.currentRound} of ${TOTAL_ROUNDS}`;
      entryRoundNum.textContent = state.currentRound;

      // Render round dots
      renderRoundDots();

      // Render table header
      let theadHtml = '<tr><th>Player</th>';
      for (let r = 1; r <= TOTAL_ROUNDS; r++) {
        const cls = r === state.currentRound ? 'class="col-current"' : '';
        theadHtml += `<th ${cls}>${r}</th>`;
      }
      theadHtml += '<th>Total</th></tr>';
      scorecardThead.innerHTML = theadHtml;

      // Calculate totals
      const totals = state.players.map(p => {
        return p.scores.reduce((sum, s) => sum + (s !== null ? s : 0), 0);
      });
      const minTotal = Math.min(...totals);

      // Render table body
      let tbodyHtml = '';
      state.players.forEach((player, pi) => {
        const isLeader = totals[pi] === minTotal;
        const leaderCls = isLeader ? ' class="leader-glow"' : '';
        tbodyHtml += `<tr${leaderCls}>`;
        tbodyHtml += `<td>${escapeHtml(player.name)}</td>`;
        for (let r = 0; r < TOTAL_ROUNDS; r++) {
          const score = player.scores[r];
          const isCurrent = (r + 1) === state.currentRound;
          const scoreDisplay = score !== null ? score : '-';
          const cellCls = isCurrent ? ' class="col-current"' : '';
          tbodyHtml += `<td${cellCls}><span class="score-cell">${scoreDisplay}</span></td>`;
        }
        tbodyHtml += `<td>${totals[pi]}</td>`;
        tbodyHtml += `</tr>`;
      });
      scorecardTbody.innerHTML = tbodyHtml;

      // Render score entry inputs
      renderScoreEntry();
    }

    function renderRoundDots() {
      roundDots.innerHTML = '';
      for (let r = 1; r <= TOTAL_ROUNDS; r++) {
        const dot = document.createElement('span');
        dot.className = 'round-dot';
        if (r === state.currentRound) dot.classList.add('round-dot--active');
        else if (r < state.currentRound) dot.classList.add('round-dot--past');
        roundDots.appendChild(dot);
      }
    }

    function renderScoreEntry() {
      scoreEntryInputs.innerHTML = '';
      state.players.forEach((player, pi) => {
        const group = document.createElement('div');
        group.className = 'score-entry__input-group';
        const existingScore = player.scores[state.currentRound - 1];
        const inputId = `score-${pi}`;
        group.innerHTML = `
          <label for="${inputId}">${escapeHtml(player.name)}</label>
          <input type="number" id="${inputId}" class="score-input"
                 min="0" max="999" placeholder="0"
                 value="${existingScore !== null ? existingScore : ''}"
                 autocomplete="off">
        `;
        scoreEntryInputs.appendChild(group);
      });
      entryError.textContent = '';

      // Focus first input
      const firstInput = scoreEntryInputs.querySelector('.score-input');
      if (firstInput) firstInput.focus();
    }

    // =========================================================
    // Record Scores
    // =========================================================
    btnRecordScores.addEventListener('click', () => {
      const inputs = scoreEntryInputs.querySelectorAll('.score-input');
      const roundIdx = state.currentRound - 1;

      // Validate
      let valid = true;
      inputs.forEach((inp, pi) => {
        const val = inp.value.trim();
        if (val === '') {
          valid = false;
          inp.style.borderColor = 'var(--red)';
        } else {
          inp.style.borderColor = '';
          const num = parseInt(val, 10);
          if (isNaN(num) || num < 0) {
            valid = false;
            inp.style.borderColor = 'var(--red)';
          } else {
            state.players[pi].scores[roundIdx] = num;
          }
        }
      });

      if (!valid) {
        entryError.textContent = 'Enter a valid score (0 or higher) for each player.';
        return;
      }

      entryError.textContent = '';

      // Advance or end game
      if (state.currentRound >= TOTAL_ROUNDS) {
        // Game complete
        state.gameActive = false;
        saveGame();
        clearSavedGame();
        showScreen('screen-results');
        renderResults();
      } else {
        state.currentRound++;
        saveGame();
        renderScorecard();
      }
    });

    // End Game early
    btnEndGame.addEventListener('click', () => {
      if (confirm('End the current game? All progress will be lost.')) {
        endGame();
      }
    });

    // =========================================================
    // Utilities
    // =========================================================
    function escapeHtml(text) {
      const div = document.createElement('div');
      div.appendChild(document.createTextNode(text));
      return div.innerHTML;
    }
```

- [ ] **Step 4: Open in browser and verify**

Open `index.html`. Enter 2 players, start game. Expected:
- Header shows "Round 1 of 9" with UNO badge, round dots (1 active, 8 dimmed), End Game button
- Scorecard table with player names, 9 round columns (column 1 highlighted with blue border), Total column
- Each score cell is a white rounded rect with shadow
- Score entry inputs below the table — one per player
- Enter scores, click "Record Scores" → saves, advances to round 2, re-renders
- Column 1 shows scores, column 2 now highlighted
- End Game → confirm dialog → (Task 6 renders results — may show blank for now)

- [ ] **Step 5: Commit**

```bash
git -C "C:\Users\sedol\Documents\UnoGolfScorer" add index.html
git -C "C:\Users\sedol\Documents\UnoGolfScorer" commit -m "feat: add full scorecard screen with table, UNO card cells, score entry"
```

---

### Task 6: Results Screen

**Files:**
- Modify: `index.html` — add results screen HTML, CSS, render function

**Interfaces:**
- Consumes: Task 5's `endGame()` calls `renderResults()`, `showScreen('screen-results')`
- Produces: `renderResults()` — renders ranked list with winner highlight

- [ ] **Step 1: Add results screen HTML**

Replace the `<!-- Screen 3: Results -->` placeholder:

```html
    <!-- Screen 3: Results -->
    <div id="screen-results" class="screen">
      <div class="results-container">
        <div class="uno-badge">
          <span class="uno-badge__text">FINAL SCORES</span>
        </div>

        <div id="results-list" class="results-list">
          <!-- Rendered by JS -->
        </div>

        <button id="btn-new-game" class="btn btn--primary">New Game</button>
      </div>
    </div>
```

- [ ] **Step 2: Add results screen CSS**

Replace the `/* Screen 3 — Results */` placeholder:

```css
  /* =========================================================
     Screen 3 — Results
     ========================================================= */
  .results-container {
    max-width: 500px;
    margin: 40px auto;
    text-align: center;
  }

  .results-list {
    display: flex;
    flex-direction: column;
    gap: 10px;
    margin: 28px 0;
    text-align: left;
  }

  .result-row {
    display: flex;
    align-items: center;
    gap: 12px;
    background: rgba(255,255,255,0.7);
    border-radius: 10px;
    padding: 14px 18px;
    box-shadow: var(--shadow);
    transition: transform 0.15s;
  }

  .result-row:hover {
    transform: translateX(4px);
  }

  .result-rank {
    font-weight: 900;
    font-size: 1.1rem;
    min-width: 32px;
    color: var(--text);
    opacity: 0.5;
  }

  .result-name {
    flex: 1;
    font-weight: 700;
    font-size: 1.05rem;
  }

  .result-score {
    font-weight: 800;
    font-size: 1.15rem;
    color: var(--header);
  }

  /* Winner styling */
  .result-row--winner {
    background: linear-gradient(135deg, rgba(175, 157, 71, 0.15), rgba(175, 157, 71, 0.05));
    border: 2px solid var(--gold);
    transform: scale(1.02);
  }

  .result-row--winner:hover {
    transform: scale(1.02) translateX(4px);
  }

  .result-row--winner .result-rank::before {
    content: '\1F3C6'; /* trophy emoji */
    margin-right: 4px;
  }

  .result-row--winner .result-name {
    color: var(--gold);
  }

  .result-row--winner .result-score {
    color: var(--gold);
  }
```

- [ ] **Step 3: Add results JavaScript**

After the `endGame()` function block, add:

```js
    // =========================================================
    // Results
    // =========================================================
    const resultsList = $('results-list');
    const btnNewGame  = $('btn-new-game');

    function renderResults() {
      if (!state.players || state.players.length === 0) return;

      // Calculate totals and sort
      const ranked = state.players.map((p, i) => ({
        name: p.name,
        total: p.scores.reduce((sum, s) => sum + (s !== null ? s : 0), 0),
        index: i
      })).sort((a, b) => a.total - b.total); // ascending — lower is better in golf

      const minTotal = ranked[0].total;

      resultsList.innerHTML = '';
      ranked.forEach((player, rank) => {
        const isWinner = player.total === minTotal;
        const row = document.createElement('div');
        row.className = `result-row${isWinner ? ' result-row--winner' : ''}`;
        row.innerHTML = `
          <span class="result-rank">${isWinner ? '' : `#${rank + 1}`}</span>
          <span class="result-name">${escapeHtml(player.name)}</span>
          <span class="result-score">${player.total}</span>
        `;
        resultsList.appendChild(row);
      });
    }

    btnNewGame.addEventListener('click', () => {
      state = { players: [], currentRound: 1, gameActive: false };
      // Reset setup inputs
      playerInputs.innerHTML = '';
      playerCount = 0;
      addPlayerInput('');
      addPlayerInput('');
      updateAddButton();
      updateStartButton();
      setupError.textContent = '';
      showScreen('screen-setup');
    });
```

- [ ] **Step 4: Open in browser and verify**

Open `index.html`. Enter 2 players, play through 9 rounds of scores. Expected:
- After round 9, the results screen appears
- Players are ranked by total (lowest first)
- Winner has gold highlight, trophy emoji, and slightly larger row
- Each player shows name + total score
- "New Game" button returns to setup screen with fresh inputs

Also test End Game button during scorecard: Expected:
- Confirm dialog appears
- Click OK → skips to results screen with current scores

- [ ] **Step 5: Commit**

```bash
git -C "C:\Users\sedol\Documents\UnoGolfScorer" add index.html
git -C "C:\Users\sedol\Documents\UnoGolfScorer" commit -m "feat: add results screen with ranked list and winner highlight"
```

---

### Task 7: Mobile Responsive + Edge Cases

**Files:**
- Modify: `index.html` — add media queries for <600px, touch targets, edge case handling

**Interfaces:**
- Consumes: All prior tasks' CSS/HTML/JS
- Produces: Responsive layout that works on narrow viewports, handles all edge cases

- [ ] **Step 1: Add mobile CSS**

Replace the `/* Mobile Responsive */` CSS placeholder:

```css
  /* =========================================================
     Mobile Responsive (max-width: 599px)
     ========================================================= */
  @media (max-width: 599px) {
    .container {
      padding: 12px;
    }

    /* UNO badge scales down */
    .uno-badge {
      padding: 10px 20px;
      border-radius: 40px / 28px;
    }

    .uno-badge__text {
      font-size: 1.1rem;
    }

    /* Setup inputs stack naturally (already flex column) */
    .setup-container {
      margin: 20px auto;
    }

    .setup-actions {
      flex-direction: column;
      align-items: stretch;
    }

    .setup-actions .btn {
      width: 100%;
    }

    /* Scorecard — sticky player column + horizontal scroll */
    .scorecard-header {
      flex-wrap: wrap;
      padding: 10px 12px;
    }

    .scorecard-header__center {
      order: 3;
      width: 100%;
    }

    .round-title {
      font-size: 1rem;
    }

    .scorecard-table-wrap {
      margin: 0 -12px 20px;
      border-radius: 0;
    }

    .scorecard-table tbody td:first-child {
      position: sticky;
      left: 0;
      z-index: 1;
      min-width: 80px;
    }

    .score-cell {
      width: 38px;
      height: 38px;
      font-size: 0.9rem;
    }

    /* Score entry inputs stack vertically */
    .score-entry__inputs {
      grid-template-columns: 1fr;
    }

    /* Results */
    .results-container {
      margin: 20px auto;
    }

    .result-row {
      padding: 12px 14px;
    }

    .result-row--winner {
      transform: scale(1.01);
    }

    .result-row--winner:hover {
      transform: scale(1.01) translateX(4px);
    }
  }
```

- [ ] **Step 2: Add edge case — localStorage not available**

At the top of the `<script>` block, before all other code, add a try/catch that wraps every localStorage call. This was partially done in Task 4 — add a graceful fallback:

Replace the existing localStorage check in Task 4 with this:

```js
    // Check localStorage availability and set up fallback
    let storageAvailable = true;
    try {
      localStorage.setItem('__test__', '1');
      localStorage.removeItem('__test__');
    } catch (e) {
      storageAvailable = false;
      console.warn('localStorage unavailable — scores will not persist across reloads.');
    }
```

Then modify `saveGame()` and `loadSavedGame()` and `clearSavedGame()` to check the flag:

```js
    function saveGame() {
      if (!storageAvailable) return;
      try {
        localStorage.setItem(STORAGE_KEY, JSON.stringify(state));
      } catch (e) { /* ignore */ }
    }

    function loadSavedGame() {
      if (!storageAvailable) return null;
      try {
        const raw = localStorage.getItem(STORAGE_KEY);
        if (!raw) return null;
        const parsed = JSON.parse(raw);
        if (!parsed || !parsed.players || !parsed.currentRound || !parsed.gameActive) return null;
        return parsed;
      } catch (e) {
        return null;
      }
    }

    function clearSavedGame() {
      if (!storageAvailable) return;
      try {
        localStorage.removeItem(STORAGE_KEY);
      } catch (e) { /* ignore */ }
    }
```

Also show a one-time warning if localStorage is unavailable:

```js
    if (!storageAvailable) {
      const warn = document.createElement('div');
      warn.className = 'storage-warn';
      warn.textContent = 'Warning: localStorage is unavailable. Scores will not persist if you close or reload the page.';
      document.querySelector('.container').prepend(warn);
    }
```

Add CSS for the warning:

```css
  .storage-warn {
    background: rgba(180, 45, 42, 0.1);
    border: 1px solid var(--red);
    border-radius: 6px;
    padding: 10px 14px;
    font-size: 0.85rem;
    color: var(--red);
    margin-bottom: 16px;
    text-align: center;
  }
```

- [ ] **Step 3: Add edge case — score input validation improvement**

Add keyboard handling to make it easier to enter scores quickly. After `btnRecordScores` click handler in Task 5, add:

```js
    // Allow Enter key to advance through score inputs and submit
    document.addEventListener('keydown', (e) => {
      if (e.key === 'Enter' && document.activeElement?.classList.contains('score-input')) {
        e.preventDefault();
        const inputs = [...scoreEntryInputs.querySelectorAll('.score-input')];
        const idx = inputs.indexOf(document.activeElement);
        if (idx < inputs.length - 1) {
          inputs[idx + 1].focus();
        } else {
          btnRecordScores.click();
        }
      }
    });
```

- [ ] **Step 4: Open in browser and fully test**

Test on **desktop** (Chrome DevTools responsive mode):
1. Set viewport to 1440px → full layout with 2-across score inputs
2. Resize down to 768px → inputs go to wider grid, everything readable
3. Set viewport to 375px (mobile) → expected:
   - Setup screen: full-width buttons stacked, badge smaller
   - Scorecard: horizontal scroll, player name column sticky on left, rest scrolls
   - Score entry: inputs stacked vertically
   - All buttons 44px+ tap targets
4. Test with 8 players on mobile → should scroll fine

Test **edge cases**:
1. **Reload mid-game** → confirm dialog shows → OK resumes, Cancel starts fresh
2. **Score re-entry** → enter score for round 1, record, click round 2, go back — not spec'd to allow re-edit (only current round is editable via the score entry inputs). Verify the current round's input pre-fills if you haven't recorded it.
3. **0 or 1 player** → min 2 enforced, Start Game stays disabled
4. **Empty player name** → validation error shown
5. **End Game** → confirm → results screen with correct totals
6. **All 9 rounds** → auto-transition to results after Round 9 scores recorded
7. **Negative score** → validation rejects (min 0)
8. **Score entry Enter key** → pressing Enter moves to next input or submits

- [ ] **Step 5: Commit**

```bash
git -C "C:\Users\sedol\Documents\UnoGolfScorer" add index.html
git -C "C:\Users\sedol\Documents\UnoGolfScorer" commit -m "feat: add mobile responsive layout and edge case handling"
```

---

## Self-Review

### 1. Spec Coverage

Cross-check against spec sections:

| Spec Section | Covered By |
|---|---|
| Overview (single-file HTML app) | Task 1 — scaffold |
| Tech Stack (zero-dependency, localStorage) | Task 1 (scaffold), Task 4 (persistence) |
| Color Palette (all 8 colors) | Task 1 — CSS variables in `:root` |
| Typography (system sans-serif) | Task 1 — `--font-display` and `--font-body` |
| Screen 1 — Player Setup (2-8 names, UNO badge, validation) | Task 3 |
| Screen 2 — Scorecard (9 columns, totals, UNO cells, score entry) | Task 5 |
| Screen 3 — Final Results (ranked, winner highlight, New Game) | Task 6 |
| SVG Background Curves (#6fa067 at 8%, #53775f at 6%) | Task 2 |
| Data Model (players, scores array, currentRound, gameActive) | Task 4 |
| Persistence (localStorage key unoGolfScorer, save/load/resume) | Task 4 |
| Edge Cases (reload, re-entry, localStorage fallback, empty names, auto-transition) | Tasks 4, 7 |
| Mobile (<600px horizontal scroll, sticky column, stacked inputs, 44px targets) | Task 7 |
| Non-Goals (no card tracking, no rules, no network, no themes, no animations) | Implicit — never implemented |

### 2. Placeholder Scan

- No TBD, TODO, "fill in details", or "add later" patterns
- Every step has exact code and exact commands
- No "Similar to Task N" references
- All CSS: block comments used consistently
- All function signatures defined explicitly

### 3. Type Consistency

- `state` object shape: `{ players: [{name: string, scores: (number|null)[]}], currentRound: number (1-indexed), gameActive: boolean }` — same in Task 4, 5, 6
- `saveGame()` / `loadSavedGame()` / `clearSavedGame()` — names consistent across Tasks 4, 5, 6, 7
- `showScreen(id)` — called with string id in Tasks 4, 5, 6
- `renderScorecard()` — no args, reads from `state` — consistent in Tasks 4, 5
- `renderResults()` — no args, reads from `state` — consistent in Tasks 5, 6
- `endGame()` — no args, calls `renderResults()` + `showScreen()` — consistent in Tasks 4, 5, 6
- `startNewGame(names)` — called from setup (Task 3), defined in Task 4
- CSS class names: `.screen--active`, `.score-cell`, `.round-dot--*`, `.col-current`, `.leader-glow`, `.result-row--winner` — consistent across all tasks
- `escapeHtml()` — used in Task 5 and 6

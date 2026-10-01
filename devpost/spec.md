---
doc: spec
status: approved
---

# Protein Pal — Technical Spec

## How This Works, In Plain Language
One HTML file holds everything: the layout (HTML), the dark styling (CSS), and the behavior (JavaScript). When the page loads, the script reads the saved data from the browser's localStorage — a small built-in database every browser has — and draws today's log, the totals, and the history. Every add/delete/goal change updates the data in memory, writes it back to localStorage, and re-draws the screen. No server, no build step, no dependencies. Double-click the file and it runs.

## The Core Journey Through the System
PRD ref: `prd.md > The Core Journey`.
1. Page loads → script reads `proteinpal.v1` from localStorage (or starts empty) → renders header (date, goal), hero (0/180g), empty log, empty recents.
2. Learner submits the add form → JS validates name/grams → pushes `{name, grams, lactose, ts}` into today's entry list → saves → re-renders: hero total, bar width, log list, recents chips.
3. Taps a recent chip → same as step 2 with the chip's name/grams/lactose, no typing.
4. Deletes an entry → removes it from today's list → saves → re-renders.
5. Edits goal → updates stored goal → re-renders hero/bar.
6. New calendar day → today's key differs from stored days → fresh empty log; yesterday's list now appears under History.

## Stack
- HTML + CSS + JavaScript, no framework, no build tools, no npm dependencies. Rationale (learner-selected): zero setup, runs by opening the file, easiest possible demo recording. Tradeoff accepted: no component structure; fine at this size.
- Browser localStorage for persistence. Docs: MDN Web Storage API. Rationale: single-user POC, no server needed.

## Where It Runs and How Someone Tries It
Runs in any modern browser. Requirements: none beyond the browser. Start: open `index.html` (double-click or `python3 -m http.server` + visit localhost). For the demo recording: open the file, add 2–3 foods, tap a recent, show history. Deployment optional — not needed for submission (video + public repo suffice).

## Look and Feel
Dark theme: near-black background (#111), light text, one strong accent for the progress bar (lime green, gym-energy). System sans-serif stack. Spacious single column, max-width ~480px, big tap targets (min 44px). Interface copy is terse: "Add", "Today", "Recents", "History". No charts, no gradients-for-fun.

## Components

### State store
In-memory object + localStorage read/write. Shape: `{ goal: number, days: { "YYYY-MM-DD": [entries] }, recents: [{name, grams, lactose}] }`.
PRD ref: `prd.md > Features and Behavior`, `prd.md > States and Boundaries`.

### Add form
Text input (name), number input (grams), checkbox (contains lactose), submit button. Validates and hands a new entry to the store.
PRD ref: `prd.md > Features and Behavior > Logging`.

### Progress hero
Big total/goal readout + bar. Re-renders on every state change; green at ≥100%.
PRD ref: `prd.md > Screens and Layout`.

### Today's log
List of today's entries with delete buttons and lactose badges.
PRD ref: `prd.md > Features and Behavior > Log management`.

### Recents
Deduped chips from all past entries (case-insensitive name match), most-recent-first, capped at 12. One-tap re-add.
PRD ref: `prd.md > Features and Behavior > Logging`.

### History
Past days newest-first with totals; click to expand entries.
PRD ref: `prd.md > Features and Behavior > History`.

## Data Model
- `goal`: number, default 180. Lives in the store root; updated via inline edit.
- `days`: map of local-date string → entry array. Entry: `{name: string, grams: number, lactose: bool, ts: epoch}`. Updated on add/delete. A day with zero entries still renders in history only if it had entries.
- `recents`: derived from all entries, deduped by lowercase name, ordered by latest `ts`, capped at 12. Recomputed on render (no separate storage needed, but cached in store for simplicity).
- Everything persists under one localStorage key `proteinpal.v1` as JSON. Leaving and coming back: state restored exactly.

## File Structure
```
learn-ai-basics/
├── index.html          # the entire app (HTML + CSS + JS)
├── devpost/            # Devpost learning workspace
│   ├── learner-profile.md
│   ├── scope.md
│   ├── prd.md
│   └── spec.md
├── .agents/skills/     # Devpost Learn Skill Pack (not learner code)
└── .gitignore
```

## External Services and Dependencies
None. No APIs, no databases, no hosting required.

## Important Failure Modes
- **localStorage full or blocked (private mode)** → catch on write; show a small notice "couldn't save — check browser storage settings"; app still works in-memory for the session.
- **Corrupt stored JSON** → catch on parse; back up the bad value under `proteinpal.v1.bak`, start fresh, show notice.
- **Midnight rollover while open** → date key changes; timer checks every minute and re-renders so a new day starts cleanly.

## What Was Simplified and Why
- **Single self-contained file** instead of separate CSS/JS/modules — why: zero tooling, opens anywhere, demo-friendly. The fuller version (bundler, modules) would add setup for no POC benefit.
- **localStorage** instead of any backend — why: single user by design. A backend would need auth/hosting for nothing the POC needs.
- **Derived recents** instead of a real food database — why: typing + one-tap re-add covers the loop; a database is a Later item.

## Decisions and Open Issues
- Chose dark theme default (learner trains/logs at night) — tradeoff: light-mode users get dark anyway; acceptable for POC.
- Chose 180g default goal (learner's actual target) — editable, persisted.
- Genuine learner uncertainty discussed: none surfaced — the learner knew exactly what he wanted (protein-only, lactose flag). Per template, stating this plainly instead of inventing one.
- Open: demo video recording happens on the learner's own machine before submission; no code dependency.

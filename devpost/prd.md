---
doc: prd
status: approved
---

# Protein Pal — Product Requirements

One line: a dead-simple daily protein tracker for lifters — log what you ate, see where you stand against your goal.
Source: `scope.md > The Unique Kernel`, `scope.md > Who It's For`.

## The Core Journey
1. hao opens the page in the morning. He sees today's date, a big "0 / 180g" and an empty progress bar.
2. After breakfast he types "eggs" and "18", checks nothing else, hits Add. The total jumps to 18/180g; eggs appears under Today's log and in Recents.
3. After lunch he taps "chicken breast" in Recents — one tap, 40g added, no typing.
4. He tries adding "regular milk" and marks the lactose flag; the entry shows a small ⚠️ lactose badge so he notices before it becomes a habit.
5. By evening the bar is full at 185/180g. He closes the tab.
6. Next morning the log is fresh at 0, but yesterday is visible in History with its total. The goal (180g) persisted.

## Screens and Layout
Single page, single column, mobile-narrow friendly:
1. **Header** — app name, today's date, goal display with an edit control.
2. **Progress hero** — big "X / 180g" number and a full-width progress bar. Turns green at 100%.
3. **Add form** — food name (text), protein grams (number), lactose checkbox, Add button.
4. **Recents** — chips of previously logged foods (name + grams); one tap re-adds.
5. **Today's log** — list of entries with grams, lactose badge where flagged, per-entry delete.
6. **History** — past days with date and total, expandable to see entries.

## Look and Feel
Spare and fast, gym-whiteboard energy. Dark theme by default (he logs at night too). One accent color for the progress bar. Big touch targets; the Add flow must feel like five seconds. No design system beyond: readable sans, generous spacing, no clutter. Avoid: dashboards, charts, onboarding tours.

## Features and Behavior

### Logging
- Add a food with name (required), protein grams (required, positive number), lactose flag (optional checkbox).
- Entry appears instantly in Today's log and updates the total and bar.
- As the learner (a lifter), I want one-tap re-add from Recents so that repeat meals take one second.
  - [ ] Tapping a recent chip adds the same food+grams as a new entry for today
  - [ ] Recents are deduplicated by name (case-insensitive), most recent first, max ~12 shown

### Goal
- Daily goal defaults to 180g, editable inline; persists across sessions.
- As the learner, I want to change my protein goal so that cut/bulk phases are reflected.
  - [ ] Editing the goal updates the hero number and bar immediately and persists

### Log management
- Delete any of today's entries; total and bar update.
- Lactose-flagged entries show a ⚠️ badge in the log.

### History
- Each day's entries are stored under the calendar date; History lists past days (newest first) with totals; expanding a day shows its entries.
- "Today" is determined by local date; a new day starts a fresh log automatically.

## States and Boundaries
- **First use** — empty log, goal 180g, Recents empty with a hint ("logged foods show up here").
- **Empty today** — hero shows 0/180g, log shows "nothing logged yet".
- **Goal reached** — bar full + green, hero shows e.g. "185 / 180g".
- **Invalid input** — empty name or non-positive grams: inline message, entry not added.
- **Persistence** — everything in localStorage; survives tab close and reload. No account, no server.
- **What disappears** — nothing user-entered disappears; only the "today" view resets at midnight local.

## Product Decisions
- Single self-contained `index.html` (inline CSS/JS) — learner's choice: zero build step, opens anywhere, trivial to demo-record.
- localStorage over any backend — learner's reason: single user, single device, POC.
- Dark theme default — learner trains/logs at night; light text on dark is easier.
- No food database — typing + recents is faster for the POC (from scope's Explicitly Cut).

## What We're Building
Everything under Features and Behavior above, in one page, working end to end.

## Deferred From the POC
- Food database/API search — out: typing + recents covers the POC loop.
- Reminders/notifications — out: no backend, and scope is logging only.
- Streaks/gamification — out: the progress bar is the game for now.

## Possible Later Enhancements
Weekly average protein line. Export day as text. Per-meal grouping (breakfast/lunch/dinner).

## Non-Goals
- Calorie, carb, or fat tracking — protein-only is the kernel.
- Multi-user, accounts, cloud sync — single user by design.
- Barcode scanning — needs native/API work, out of POC.

## Open Questions
None blocking `4-spec`. (Demo video: learner records on his own machine before submission.)

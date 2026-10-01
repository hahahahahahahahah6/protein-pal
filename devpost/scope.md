---
doc: scope
status: approved
---

# Protein Pal

One line: a dead-simple daily protein tracker for lifters — log what you ate, see where you stand against your goal.

## The Unique Kernel
Most nutrition apps want your life story, then bury protein under calories, macros, and upsells. This does one thing: protein grams in, progress toward today's goal. Plus a lactose flag, because the person it's built for can't do dairy.

## Who It's For
hao, a 20-year-old lifter eating 160–190g of protein a day. Today he does the running total in his head or in a notes app, and re-adds the same chicken breast and protein shake every single day.

## The Core Loop
He opens it after a meal, types the food and grams (or taps a recent item), and sees the day's bar move. He comes back because the bar resets every morning and the streak of hitting the goal is the game.

## Inspiration & Identity
Spare and fast, like a gym whiteboard. Big number, big progress bar, no charts-for-charts'-sake. Dark-friendly. Should feel like it takes five seconds to log a meal — because it does.

## Why This Matters to the Learner
"I track 160–190g protein a day for training and I'm tired of doing the math in his head." Built for himself, used daily if it's good.

## What "Working" Looks Like
Open the page, add "chicken breast — 40g", watch today's total jump to 40/180g with the bar filling. Close the tab, reopen — it's still there. The "oh, that's cool" beat: tapping a recent food logs it in one tap, and the lactose flag warns before he logs regular milk.

## The POC Boundary
In: add food (name + grams + optional lactose flag), today's total vs. adjustable goal, progress bar, recent-foods quick re-add, per-day history, localStorage persistence, delete/edit an entry.
Out: accounts, cloud sync, food database/API lookup, barcode scanning, other macros, sharing, streak gamification beyond a simple day count.

## Later
Food database with search, barcode scan, weekly averages chart, reminders.

## Explicitly Cut
- Calorie/macro tracking — the whole point is protein-only; anything else dilutes the kernel.
- Accounts & sync — single user, single device is fine for the POC; localStorage is enough.
- Food search API — typing "chicken 40" is faster than searching for the POC; recent-foods covers repetition.

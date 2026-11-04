# How to work with me
I'm building this project to LEARN. Follow these rules:
- Work one small step at a time. Tell me the plan for the step before doing it.
- Explain WHY, not just what: what each file is for, what new concepts mean.
- For short, important pieces of code, let me try writing them first, then review.
- After each step, tell me how to run and test it myself.
- Explain every terminal and git command the first time we use it.
- NEVER run git commit or git push yourself. When a step is done, suggest a
  commit message and tell me the exact commands to run. I'll run them myself.
- Never add AI attribution to commit messages or any files.
- One branch per phase. At the end of each phase: quiz me with 3 questions,
  then guide me through opening a pull request and tagging a release.
- Never commit secrets. Keep keys in .env, which is listed in .gitignore.
- I'm on Windows and use PowerShell.

# Project
# PUZZLE COMPANION: PROJECT SUMMARY

## Overview
A web app installable on phone and laptop (PWA). It helps me finish jigsaw
puzzles using photos: it finds where a piece goes, or finds which piece fits a
spot. It tracks my time per puzzle and keeps a full history and stats.
We build it ONE STEP AT A TIME: foundation first, then one feature per phase.

## Tech stack
- Frontend: React + Vite, installable web app (camera access, phone + laptop)
- Matching server: Python (FastAPI + OpenCV) for finding pieces in photos,
  image matching and shape matching
- Data: Supabase (login, database, image storage), so phone and laptop sync
- Build and test locally first, then deploy online

## Features

### 1. Puzzle setup and library
- Upload the box image and enter the piece count
- Width and height are OPTIONAL. If blank, the grid comes from the box image's
  proportions. I can add or edit dimensions LATER; the grid recalculates and
  existing progress is carried over to the new grid
- Grid preview over the image to check the setup
- Library entry: cover, brand, piece count, dimensions, status
  (want to do / in progress / done), notes, finished photo
- Several puzzles can be in progress at once, each with its own timer and progress

### 2. Mode A: "Where does this piece go?"
- Up to 10 pieces at a time (one photo of several pieces, or separate photos)
- Results in tabs (Piece 1-10). Each tab shows the piece crop, a heatmap over
  the puzzle image, and the top 5-10 likely spots ranked by confidence
- Checks all 4 rotations
- Edge detection: pieces with a flat side are matched only against the border
  (accurate edges are a priority; I love doing edge pieces)
- "Placed it" button marks the cell filled; filled cells are skipped in future searches
- Focus area: tell the app which region I'm working on, and it searches there first
- Honest low-confidence messages for plain areas like sky or water

### 3. Mode B: "Find the piece for this spot" (reverse search)
- Pick a spot: tap a cell on the reference, or photograph the gap in my
  partly built puzzle
- Lay out loose pieces on a plain, contrasting surface; take one photo from above
- The app finds and numbers each piece and ranks the best fits by:
  - Shape: each side classified as flat / tab / blank, outline compared to the gap
  - Image: does the piece's picture match the reference at that spot
- Results: top candidates circled and numbered on the photo, plus a ranked list

### 4. Timer and sessions
- I START and END the timer manually
- Recorded session times can't be edited afterward
- Each session is logged: date, duration, pieces placed, lookups used

### 5. End of session
- When I end the timer, a prompt asks: "Did you complete the puzzle?"
  - No: session summary (time, pieces placed, lookups), saved to the log
  - Yes: completion screen with total time, number of sessions and days,
    start and finish dates, Mode A uses, Mode B uses (each piece looked up = 1),
    pieces per hour, and an optional finished photo

### 6. Quality and corrections
- Camera guide frame, plus warnings for blur, glare or a busy background
  before searching
- Undo and fix: reverse a mistaken "Placed it" or correct a wrong match

### 7. Stats and history
- Calendar heatmap of puzzle days, streaks, total hours
- Pace over time, comparisons between puzzles
- Personal records: fastest 500 / 1000 piece, longest streak, fewest lookups
- Progress timelapse: optional photo at the end of each session, made into a
  timelapse when I finish
- Rating and difficulty after completing a puzzle
- Missing-pieces log

### Not included
- Shareable completion card

## Data model (main records)
- Puzzle: reference image, piece count, optional width/height, grid
  rows/cols, status, rating, notes
- Session: puzzle, start, end, duration, pieces placed, lookups
- PieceQuery: puzzle, mode (A or B), photo, ranked results, whether I placed it
- Placement: puzzle, grid cell, time placed (can be undone)
- ProgressPhoto: puzzle, session, image

## GAME PLAN (one phase at a time; finish and test each before moving on)

PHASE 1: Foundation
- Project setup (React frontend + Python server + Supabase)
- Installable web app shell that works on phone and laptop
- Login, so data syncs between devices
- Puzzle library: add / edit / delete puzzles, upload box image
- Puzzle setup: piece count, optional dimensions, grid preview,
  edit dimensions later
Done when: I can log in on phone and laptop, add a puzzle, and see its grid.

PHASE 2: Timer and sessions
- Manual start / end timer, saved sessions that can't be edited
- End-of-session prompt: "Did you complete the puzzle?"
- Session summary and completion screen (lookup counts start at 0 until the
  matching modes exist)
Done when: I can time a session, end it, and mark a puzzle complete.

PHASE 3: Mode A, single piece
- Photo of one piece, background removed, matched against the grid in 4 rotations
- Heatmap + top 5-10 ranked spots, tap for a side-by-side comparison
- Camera guide + photo quality warnings
Done when: real piece photos land in the right area most of the time.

PHASE 4: Mode A, full version
- Up to 10 pieces with tabbed results
- Edge piece detection, border-only matching
- "Placed it", progress map, undo and fix
- Focus area
- Lookup counts connected to sessions and the completion stats

PHASE 5: Mode B, reverse search
- Finding several pieces in one layout photo
- Target spot by tapping a cell, or by a photo of the gap
- Shape analysis (flat / tab / blank, outline comparison) + image match
- Ranked candidates circled on the photo

PHASE 6: Stats and history
- Stats dashboard: calendar heatmap, streaks, pace, comparisons
- Personal records
- Progress photos + timelapse
- Rating / difficulty, missing-pieces log

PHASE 7: Polish and deploy
- Speed and accuracy tuning with my real puzzles
- Deploy online and install on my phone
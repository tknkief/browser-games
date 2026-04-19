# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Two browser games as standalone HTML files — no build step, no dependencies, no package manager. Each file is fully self-contained (HTML + CSS + JS in one file).

## Files

- `tictactoe.html` — Tic Tac Toe (2-player + unbeatable CPU via minimax)
- `music-quiz.html` — Music quiz using the iTunes Search API

## Running / Testing

Open any `.html` file directly in a browser (double-click or `start filename.html`). No server required.

## Architecture: music-quiz.html

**Data source:** iTunes Search API (`https://itunes.apple.com/search`) — no auth, CORS-enabled, returns `previewUrl` (30-sec MP3).

**Genre progression:** Pop (ID 14) → Rock (21) → Hip-Hop (18) → Electronic (7). Unlocked genres stored in `localStorage` under key `mqUnlocked`.

**State machine** — single `state` object drives all UI:
- `screen`: `'menu'` | `'quiz'` | `'roundEnd'`
- `songs[]`: 10 shuffled songs for the current round
- `pool[]`: full fetch result used to pick distractors
- `currentIndex`, `score`, `answered`, `choices[]`

**Question flow:** fetch 80 songs → filter for `previewUrl` → shuffle → pick 10 → per question: correct song + 2 distractors from pool → render 3 choice buttons.

**Audio:** `new Audio(previewUrl)`, stopped via `timeupdate` at 5 seconds. `stopAudio()` must be called before every screen transition.

**Pass condition:** 8 / 10 correct → next genre unlocked and persisted.

## Architecture: tictactoe.html

Minimax AI (no alpha-beta pruning — board is small enough). Score persists only for the session (not localStorage). `vsCPU` flag toggles mode; CPU always plays as `'O'`.

## Git & GitHub

Repository: `https://github.com/tknkief/browser-games`  
Commit and push after every meaningful change.

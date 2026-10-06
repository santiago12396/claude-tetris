# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

Vanilla JavaScript Tetris on HTML5 Canvas. No dependencies, no `package.json`, no build step, no tests, no linter. The whole project is three files: `index.html` (DOM + canvases), `style.css` (dark/retro theme), and `game.js` (all game logic).

## Running

Open `index.html` directly (`open index.html`) or serve the directory statically, e.g. `python3 -m http.server 8000`, then visit `http://localhost:8000`. Verify changes by playing it in a browser.

## Architecture (`game.js`)

- **Global mutable state**: all game state (`board`, `current`, `next`, `score`, `lines`, `level`, `paused`, `gameOver`, `dropInterval`, `animId`, …) lives in module-level `let` variables. `init()` resets all of it and is also the restart-button handler, so any new state must be reset there.
- **Cell values double as color indices**: `PIECES[type]` matrices store the piece's type number (1–7) in their filled cells, and `board` cells store the same number once locked. `COLORS[n]` maps it to a color. Index 0 / `null` means empty. Adding a piece type means adding to both `PIECES` and `COLORS` and updating the `* 7` in `randomPiece()`.
- **Collision is the single source of truth**: `collide(shape, x, y)` checks bounds and the board; movement, rotation (`tryRotate` with kicks `[0, -1, 1, -2, 2]`), ghost projection (`ghostY`), gravity, and game-over detection all go through it. Cells above the board (`y < 0`) are allowed.
- **Piece lifecycle**: gravity in `loop()` / `softDrop()` / `hardDrop()` → `lockPiece()` → `merge()` → `clearLines()` (updates score, level, `dropInterval`) → `spawn()` (promotes `next` to `current`; if it collides immediately, `endGame()`).
- **Game loop**: `requestAnimationFrame`-driven; accumulates `dt` in `dropAccum` and steps the piece once `dropInterval` is exceeded. The whole board is redrawn each frame in `draw()`; the next-piece preview is only redrawn in `spawn()` via `drawNext()`.
- **Pause / game over**: implemented by cancelling the animation frame and showing the shared `#overlay` (title/score text is swapped per state).

## Gotchas

- Canvas size in `index.html` must equal `COLS × BLOCK` by `ROWS × BLOCK` (currently 300×600). The `#next-canvas` (120×120) assumes a 4×4 grid of 30px cells (`NB` in `drawNext`).
- User-facing text (HTML `lang="es"`, overlay messages, README) is in Spanish; keep new UI strings consistent with that.

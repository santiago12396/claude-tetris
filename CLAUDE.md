# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

No build step, tests, or linter — verify changes by opening `index.html` in a browser and playing it.

## Gotchas

- All game state is module-level `let` variables in `game.js`; `init()` resets it and is also the restart-button handler, so any new state must be reset there.
- Cell values double as color indices: `PIECES[type]` matrices and locked `board` cells store the piece type (1–7), mapped by `COLORS[n]`; 0 / `null` is empty. Adding a piece type means adding to both `PIECES` and `COLORS` and updating the `* 7` in `randomPiece()`.
- The board is redrawn every frame, but the next-piece preview is only redrawn in `spawn()` via `drawNext()`.
- Canvas size in `index.html` must equal `COLS × BLOCK` by `ROWS × BLOCK` (currently 300×600). The `#next-canvas` (120×120) assumes a 4×4 grid of 30px cells (`NB` in `drawNext`).
- User-facing text (HTML `lang="es"`, overlay messages, README) is in Spanish; keep new UI strings consistent with that.

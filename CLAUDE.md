# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This repository contains a single-file, browser-based Tetris game: `tetris.html`. There is no build system, package manager, or server — open the file directly in a browser to run it.

## Running the Game

```bash
open tetris.html        # macOS
xdg-open tetris.html    # Linux
start tetris.html       # Windows
```

## Architecture

Everything lives in `tetris.html` as a single self-contained file with three sections:

- **HTML** — minimal layout: a `<canvas id="board">` for the game grid and a side panel with score, level, lines cleared, next-piece preview (`<canvas id="next-canvas">`), and controls legend.
- **CSS** — dark theme, flexbox layout, no external dependencies.
- **JavaScript** — vanilla JS, no frameworks or imports.

### Key JS Concepts

| Concept | Details |
|---|---|
| Board | `ROWS×COLS` (20×10) 2D array; cells are `null` (empty) or a piece-type string (`'I'`, `'T'`, etc.) |
| Pieces | Defined as 2D binary matrices in `PIECES`; colors in `COLORS` |
| Collision | `collides(board, piece, dx, dy, mat?)` — checks bounds and occupied cells |
| Rotation | `rotate(matrix)` transposes + reverses; `tryRotate()` applies wall-kick offsets `[0, -1, 1, -2, 2]` |
| Ghost piece | `ghostY()` projects the current piece straight down to show landing position |
| Game loop | `requestAnimationFrame`-based; `dropInterval` (ms) controls gravity, decreases with level |
| Scoring | Lines cleared × `SCORE_TABLE[n]` × level; hard-drop adds 2pts per row |
| Leveling | Level = `floor(totalLines / 10) + 1`; speed = `max(100, 1000 − (level−1) × 90)` ms |

# Animal Run

> An endless survival game for the Atari ST, built in C as part of COMP 2659 (Fall 2024).

<table>
  <tr>
    <td><img src="mmAR.png" width="400"/></td>
    <td><img src="AR_gameplay.png" width="400"/></td>
  </tr>
</table>

---

## Overview

Animal Run is a fast-paced, endless survival game where you control a chicken navigating a dangerous world full of monsters and collectible coins. The goal is simple: survive as long as possible and rack up the highest score you can.

The game was developed for the **Atari ST** platform in C, featuring low-level raster graphics, custom ISRs, double-buffered animation, and PSG-driven music and sound effects.

---

## Gameplay

The character automatically moves horizontally across the screen at a constant speed. You control only its vertical movement:

- **Spacebar** — Jump (burst of vertical + extra horizontal velocity)
- **W (hold)** — Fly (sustained upward movement against gravity)
- **Q** — Quit the game

Collect coins to increase your score. Avoid the monster — one hit and it's game over. When the character reaches the right edge of the screen, it wraps around to the left.

### Scoring
- Each coin collected = **+1 point**
- Score never decreases
- Up to **5 coins** on screen at once; all 5 respawn when collected

---

## Physics & Movement

| Action | Vertical Velocity | Horizontal Velocity |
|---|---|---|
| Normal movement | — | 8 px/tick |
| Flying (W held) | 15 px/tick upward | 8 px/tick |
| Jump (Spacebar) | +100 px/tick | +200 px/tick |
| Falling | −10 px/tick | constant |
| At screen top | 0 (hovering) | 8 px/tick |
| Landed | 0 | 8 px/tick |

---

## Game Objects

| Object | Size | Description |
|---|---|---|
| **Character (Chicken)** | 64×64 px | Player-controlled; moves horizontally, flies/jumps vertically |
| **Monster** | 64×64 px | Stationary; spawns near center of screen; ends game on collision |
| **Coin** | 16×16 px | Stationary; randomly placed above ground; up to 5 on screen |
| **Ground** | Full width | Horizontal boundary at y=320; character cannot go below |

---

## 🖥️ Technical Details

The game targets the **Atari ST** and is written in **C** with some assembly for performance-critical routines.

### Architecture
- **Model** (`model.c/h`) — Game state, object properties, physics
- **Renderer** (`renderer.c/h`) — Frame rendering, bitmap blitting
- **Raster** (`raster.c`, `rast_asm.s`) — Low-level plotting routines (no OS/library calls)
- **Events** (`events.c/h`) — Asynchronous (input) and synchronous (timer) event handlers
- **Input** (`input.c/h`) — Three-level input abstraction
- **PSG** (`psg.c/h`) — Programmable Sound Generator: tones, noise, envelopes
- **Music** (`music.c/h`) — Background music playback
- **Effects** (`effects.c/h`) — Sound effect triggers
- **ISRs** — Custom VBL interrupt service routine for timing and double-buffered page flipping

### Graphics
- **640×400** monochrome display
- Double-buffered rendering (flicker and tear-free)
- 256-byte-aligned second frame buffer
- Custom 1bpp bitmap assets for all game objects

### Sound
| Effect | Trigger |
|---|---|
| Ding | Coin collected |
| Bop | Character jumps |
| Du | Monster collision (game over) |
| Background music | Continuous during gameplay |

---

## Project Structure

```
/
├── main.c           # Main game loop, initialization, termination
├── model.c/h        # Game world state and object manipulation
├── events.c/h       # Event handlers (async, sync, condition-based)
├── renderer.c/h     # Master render routine and object renderers
├── raster.c/h       # Low-level raster graphics library
├── rast_asm.s       # Assembly-optimized raster routines
├── input.c/h        # Input abstraction module
├── psg.c/h          # PSG sound chip interface
├── music.c/h        # Music module
├── effects.c/h      # Sound effects module
├── bitmaps.c/h      # Bitmap asset definitions
├── MENU.C/H         # Main menu bitmap
├── types.h          # Shared type definitions and constants
└── makefile         # Build automation
```

---

## Building

Requires a cross-compilation toolchain targeting the Atari ST (e.g., **VASM** / **GCC for m68k**).

```bash
make        # Build the game
make test   # Build and run test drivers
```

---

## Authors

- **Jacky On**
- **Ochihai Omuha**

COMP 2659 — Computing Machinery II, Fall 2024  

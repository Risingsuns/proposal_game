# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A single-file Phaser 3 interactive marriage proposal game ("Marry Me - 求婚小程序"). All game code lives in `index.html` (952 lines) with embedded CSS and JavaScript. No build system, no package manager — just open `index.html` in a browser or serve it statically.

## Running

```bash
# Serve locally (any static server works)
python3 -m http.server 8080
# Then open http://localhost:8080
```

Phaser 3.86.0 is loaded from CDN, so an internet connection is required on first load.

## Architecture

Single-scene Phaser game (`ProposalScene`) with these subsystems:

- **Movement**: Arrow keys (left/right) move the character along a predefined path of 9 `LANDMARKS`. Xbox gamepad is also supported with manual deadzone handling (0.15) and vJoy virtual device filtering.
- **Background transitions**: Background switches from day → dusk → night as the player progresses through milestones (at 1/3 and 2/3 progress thresholds). Background images loaded from `assets/images/`.
- **Photo display**: Each milestone shows a corresponding photo in the top-left area. Real photos loaded from `assets/images/photo_N.*`. A `createFadeEdgeTexture()` utility applies radial fade-to-transparent masking on photos.
- **Proposal UI**: When the last milestone is reached, a semi-transparent overlay appears with "Will you marry me?" text, a Yes button, and a No button. The No button moves to a random position on click — it cannot be pressed.
- **Fireworks**: On "Yes", particle emitters fire at 5 preset screen positions, with continuous looping bursts every 2 seconds. Background alpha oscillates for a celebratory effect.
- **BGM**: MP3 audio played via `this.sound.add()` from `assets/audio/bgm.mp3`, looped at 50% volume.
- **Placeholder textures**: The codebase retains `createPlaceholderTexture()` which generates canvas-drawn placeholder graphics (backgrounds, character, photo frames) as a fallback when real assets aren't available.

Key game constants:
- Resolution: 1920×1080, scaled with `Phaser.Scale.FIT` + `CENTER_BOTH`
- `LANDMARKS` array (line 44): defines the path points, each with x/y coordinates, a color, and a name label

## Asset Directories

- `assets/images/` — Game images (backgrounds, character, landmarks, photos). Mixed formats: PNG/JPG.
- `assets/audio/` — Background music (`bgm.mp3`)
- `resources/` — Raw/unused source images; not loaded by the game

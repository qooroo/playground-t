# Swimming Race (Adie's Arcade) — Design

**Date:** 2026-06-14

## Goal

Add a single-file arcade game, **Swimming Race**, to Adie's Arcade. Tap to stroke
forward, but stamina is limited so mashing backfires — pace your strokes to beat
three (deliberately easy) CPU swimmers to the finish.

## File & integration

- New file: `swim-race.html` — self-contained (inline HTML/CSS/JS, canvas), matching
  the existing game convention (see `sandwich-stacker.html`).
- Back link in topbar → `adie-arcade.html` ("◄ Adie's Arcade").
- Styling uses Adie's neon-pool palette (pool cyan `#0bd3ff`, magenta `#ff2bce`,
  lime `#aaff00`, deep `#03102e`).
- In `adie-arcade.html`: replace **one** "coming soon" placeholder card with the live
  game card (keep the other two placeholders). Bump that page's version badge to **v2**.

## View & layout

- Top-down pool, **4 lanes**, swimmers race **left → right** toward a finish line.
- Player is lane 1 (top); 3 CPUs in lanes 2–4.
- Camera scrolls horizontally to follow the pack; a distance/progress indicator shows
  race completion.
- Lane ropes drawn between lanes; water-shimmer background.
- Swimmers rendered as 🏊 emoji (with a winded 😮‍💨 state — see below).

## Core mechanic: tap-to-stroke with stamina

- **Input:** tap / click / Space = one stroke. Each stroke adds a forward velocity
  impulse to the player.
- **Stamina** (0–100):
  - Each stroke costs a fixed amount of stamina.
  - Stamina regenerates over time; regen is **faster while gliding** (not stroking).
  - A stroke's power **scales with current stamina** — strokes at high stamina propel
    well; strokes at low stamina do little.
- **Winded state:** when stamina bottoms out (near 0), the player enters "winded" —
  strokes give almost nothing and the swimmer drifts to a crawl until stamina recovers
  above a threshold. Visual cue: swimmer shows 😮‍💨 and a slowdown; stamina bar flashes.
- **Result:** rhythmic, paced tapping (stroke → short recovery → stroke) is optimal;
  mashing is actively worse. Doing nothing = drift slowly and lose.

### Tuning targets (starting values, adjustable in playtest)

- Race length: **short & punchy**, ~15 seconds for a competent player, one screen-width
  of scroll worth of distance (exact px tuned to the canvas).
- Stroke cost ~18 stamina; regen ~22/sec gliding, ~8/sec right after a stroke.
- Stroke impulse scales linearly with stamina (full impulse near 100, ~0 near 0).
- Passive drag decelerates the swimmer so velocity must be maintained by strokes.
- **CPU speed** set so a player tapping in a sensible rhythm finishes clearly ahead;
  CPUs swim at a steady base pace with small random wobble, capped below optimal player
  pace. Goal: easy to beat, but a player who never strokes still loses.

## Game flow

1. **Start overlay** — title, brief how-to ("Tap to stroke — but watch your stamina!"),
   ▶ Start button.
2. **Countdown** — "Ready… GO!" before strokes count.
3. **Race** — player + CPUs swim to finish line.
4. **Finish overlay** — placement (🥇 / 🥈 / 🥉 / 4th), finish time, and **best time**
   (fastest, saved to `localStorage` key `swimRaceBest`). "↻ Race Again" button.

## Shared conventions (match other games)

- Topbar: back link, title, mute button (🔊/🔇).
- HUD: stamina bar + (optional) place/position indicator.
- WebAudio "blip" SFX for strokes, winded, and finish.
- `prefers-reduced-motion`: disable decorative animation.
- Best time persisted in `localStorage`.

## Out of scope

- No multiplayer, no difficulty selector, no power-ups, no multiple race lengths.
- No changes to other games or pages beyond the one Adie's placeholder swap.

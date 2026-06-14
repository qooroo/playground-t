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
  - Stamina regenerates steadily over time — recover by easing off the strokes.
  - A stroke's power **scales linearly with current stamina** — full impulse near 100,
    near-zero near 0. This is the natural anti-mash mechanic: mash and your strokes go
    limp, so you can't out-run a paced swimmer.
- **Gassed cue (visual, not a hard penalty):** when stamina drops to/below a threshold
  (~25), the swimmer shows 😮‍💨 and the stamina bar flashes red, signalling "ease off."
  There is no separate crawl penalty — the weak-stroke scaling alone handles balance,
  which keeps the feel smooth and forgiving rather than punishing a single narrow rhythm.
- **Result:** any comfortable rhythm (~0.6–1.4s between strokes) wins clearly; frantic
  mashing finishes last; doing nothing never reaches the wall (DNF / last).

### Tuning (shipped values, adjustable in playtest)

- Race distance: 1000 world units. A comfortable rhythm finishes in ~9–15s
  (short & punchy); the fastest CPU finishes ~16s.
- Stroke cost 16 stamina; steady regen 20/sec.
- Stroke impulse 190 at full stamina, scaling linearly to ~0 as stamina empties.
- Drag 2.0/sec decelerates the swimmer so speed must be maintained by strokes.
- **CPU speeds** `[62, 55, 48]` units/sec with a small ±10% wobble — set clearly below
  a sensible rhythm-tapper's pace so it's easy to beat, while mashing or idling loses.
- Balance verified by simulating the exact physics across tapping cadences (mash → 4th,
  brisk/steady/relaxed → 🥇, idle → DNF).

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

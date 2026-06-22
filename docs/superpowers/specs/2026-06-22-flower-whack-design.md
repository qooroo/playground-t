# Flower Whack 🌸 — Design Spec

**Date:** 2026-06-22
**Arcade:** Bea's Arcade
**File:** `flower-whack.html` (self-contained, no build step)

## Concept

A cosy whack-a-mole in a pastel cloud garden. Flowers pop up from a grid of
garden holes — tap/click them fast before they duck back down. Bop the rare
golden flower for bonus points, but **don't whack the bees!**

## Theme & Look

Matches Bea's Arcade aesthetic (consistent with `cloud-hopper.html`):

- Palette: lavender `#c9b6ff`, mint `#b9f6d8`, peach `#ffd6c2`, sky `#bfe3ff`,
  pink `#ffc6e7`, grape `#7454bd`, plum `#4f3b78`.
- Fonts: Baloo 2 (headings/UI) + Quicksand (body).
- Soft rounded cards, pastel gradient background, drifting cloud feel.
- Mobile-first: touch + click, `touch-action:none`, `user-select:none`.

## Layout

- **Topbar:** `◄ Bea's Arcade` back link, title `🌸 Flower Whack`, mute toggle.
- **HUD:** three pills — ⏱️ Time, Score, Best.
- **Stage:** a 3×3 grid of 9 garden "holes" (soft mounds). Each hole can hold at
  most one pop-up at a time.
- **Overlays:** a start screen and an end screen, layered over the stage.

## Cast

| Pop-up | Frequency | Tap result |
|---|---|---|
| 🌸 Flower (common) | majority | +1 point, happy pop + sound |
| 🌟 Golden flower (rare) | ~1 in 8, short-lived | +5 points, sparkle burst |
| 🐝 Bee (occasional) | ~1 in 6 | −3 points, brief screen shake + buzz |

Flower emoji may vary cosmetically (🌸🌺🌷🌻) but all common flowers score +1.

## Gameplay

- **60-second timed run.** Whack as many flowers as you can.
- **Difficulty ramps** as the clock counts down: pop-ups appear faster and stay
  visible for shorter windows. The final ~15s gets frantic.
  - Pop interval shrinks from ~900ms (start) toward ~380ms (end).
  - Pop-up "up" duration shrinks from ~1100ms toward ~600ms.
- A hole holds one thing at a time; the spawner only targets empty holes.
- **Missing a flower has no penalty** — it just retreats (low-frustration, fits
  the "cosy" tagline). Whacking a bee is the only score penalty.
- Tapping an empty hole does nothing (no penalty).
- Score floor is 0 (never goes negative).

## Scoring & Persistence

- Score accumulates from whacks.
- Best score saved to `localStorage` key `flowerWhackBest`, shown in HUD and on
  the end screen (mirrors Cloud Hopper's best-distance pattern).
- Floating "+1 / +5 / −3" text rises from each hit.

## Feedback / Juice

- Pop-in / squish-down animations on each hole's occupant.
- Whack: scale-punch + sparkle particles; golden adds extra sparkles.
- Bee hit: short screen shake + sad face flash + low buzz tone.
- Optional sound via Web Audio (no asset files); mute toggle persists for the
  session. Distinct blips for flower, golden, bee, and end.

## Accessibility

- Respects `prefers-reduced-motion`: disables background drift, screen shake,
  and decorative animation; gameplay still fully playable.
- Holes are real buttons with `aria-label`s; keyboard: keys 1–9 map to the grid
  (top-left to bottom-right) so it's playable without a pointer.

## Arcade Integration

- Replace the existing 🌸 "Mystery Game" placeholder card in `bea-arcade.html`
  with a live "Flower Whack" card (PLAY badge, `href="flower-whack.html"`).
- Update the footer version note in `bea-arcade.html`.

## Non-Goals (YAGNI)

- No multiple lives / game-over-on-bee (timed run only).
- No second avoid-type beyond the bee.
- No global/online leaderboard (local best only).
- No music tracks or image/audio asset files.

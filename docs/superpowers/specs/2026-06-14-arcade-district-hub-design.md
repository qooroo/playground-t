# Arcade District Hub — Design

**Date:** 2026-06-14

## Goal

Turn the single landing page (currently Theo's Arcade) into a **hub** that lets
visitors pick between three arcades, each with its own distinct theme:

- **Theo's Arcade** — existing neon synthwave arcade (unchanged).
- **Bea's Arcade** — soft pastels, drifting clouds, floating axolotls. New, with
  placeholder game slots.
- **Adie's Arcade** — neon pool-party vibes: night-pool blues, hot neon accents,
  people/dancing, music. New, with placeholder game slots.

## File structure

| File | Role |
|------|------|
| `index.html` | **New hub.** "Arcade District" landing with 3 big themed portal buttons. |
| `theo-arcade.html` | Current `index.html` content, **moved verbatim**. Theo's arcade + its 3 games unchanged. |
| `bea-arcade.html` | New pastel/clouds/axolotl arcade. Empty-ish cabinet with placeholder slots. |
| `adie-arcade.html` | New neon pool-party arcade. Empty-ish cabinet with placeholder slots. |

All pages stay **self-contained** (inline HTML/CSS/JS, no build step), matching the
existing convention.

## Hub page (`index.html`)

- Title/marquee: "Arcade District" (or similar) — neutral framing, not tied to one vibe.
- Three **big portal buttons**, one per arcade. Each button's **background is a flavour
  of that arcade's own theme**, so the button previews the vibe:
  - Theo → neon synthwave gradient + grid hint, 🕹️
  - Bea → pastel sky gradient with a cloud + 🦎 axolotl
  - Adie → night-pool neon gradient with water shimmer + 🕺🎵
- Each button is a large clickable card linking to its arcade page.
- Footer credit + version badge.

## New arcade pages (Bea, Adie)

Each follows the same structural skeleton as Theo's page (header/title, tagline,
"game cabinet" grid, footer) but **fully re-themed**:

- **Bea's** — palette: lavender, mint, peach, baby blue. Drifting clouds, floating
  axolotl emojis, rounded friendly type, gentle/dreamy motion.
- **Adie's** — palette: deep night-pool blue + hot neon (pink/cyan/magenta). Water
  shimmer, floating people/dancers/swimmers, music notes, party energy.

**Game cabinet:** each starts with **2–3 placeholder "coming soon" cards** (so the page
isn't bare) plus the copy-paste `HOW TO ADD A GAME` template comment from Theo's page,
adapted to each page's card classes.

**Back link:** each arcade page has a "← Arcade District" link returning to the hub.
Theo's moved page gets this link added too.

## Versioning

Version badges continue the `vX` convention. The hub and both new pages start at **v1**.
(Theo's page keeps its existing badge.)

## Out of scope

- No real new games — placeholder slots only for Bea's and Adie's.
- No shared CSS/JS extraction; pages stay self-contained per existing convention.
- No changes to existing game files (`pizza-panic.html`, `theo-says.html`,
  `sandwich-stacker.html`).

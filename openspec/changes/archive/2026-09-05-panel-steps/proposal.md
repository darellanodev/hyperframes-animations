## Why

The user needs a reusable "steps cartel" (steps card) animation for a HyperFrames video: a slides-style card showing a 3-step progression, where the active step's circle flashes with color to indicate which step is happening. The mockup already exists as static HTML (tmp-prompt.txt): a white rounded card with three numbered circles joined by connectors and a two-line description, over a gray page. The gray page must become transparency, and the card must animate through three timed blocks (step 1, then 2, then 3), each fading in, holding ~5s with the active circle pulsing, then fading out — the whole card (including the circles) fades in and out together, so at some moments nothing is visible.

## What Changes

- Create a new HyperFrames composition `panel_steps/` in this workspace (mirroring the existing `lower_third_slide/` project structure), rendering a transparent 1920×1080 canvas.
- Implement the steps cartel: a white, fully-rounded card, bottom-centered, containing a step-indicator row (three numbered circles joined by connectors) and a description line per step.
- Sequence of three 6.2s blocks (total 18.6s):
  1. Card fades in (0.6s), step 1 circle is blue and pulses repeatedly (~5 flashes over 5s), card fades out (0.6s).
  2. Card fades in, step 2 circle turns blue and pulses, card fades out.
  3. Card fades in, step 3 circle turns blue and pulses, card fades out.
- Inactive circles stay white with a light gray border; only the active circle of the current block is blue and flashes.
- The whole card — including all three step circles — fades in and out as a unit; between blocks nothing is visible while the active-circle recoloring happens invisibly at opacity 0.
- No new dependencies beyond the project's pinned `hyperframes@0.7.60` CLI, GSAP (jsdelivr) and the Poppins font via Google Fonts. No modifications to any existing capability or composition.

## Capabilities

### New Capabilities
- `panel-steps`: A standalone HyperFrames overlay that renders an animated steps cartel for a transparent video canvas — a bottom-centered white card with numbered step circles (one active blue/flashing per block), connectors, per-step description, and a deterministic fade-in/hold/pulse/fade-out choreography across three sequential blocks.

### Modified Capabilities
<!-- None -->

## Impact

- New directory `panel_steps/` under the workspace root, with `index.html`, `package.json`, `meta.json`, `hyperframes.json` (scaffolding identical in shape to `lower_third_slide/`, hyperframes pinned 0.7.60).
- Uses the existing `logo.png`‑free, pure CSS+GSAP pattern: single paused GSAP timeline registered synchronously at `window.__timelines["panel-steps"]`, deterministic and seek-safe, validated via `hyperframes check`.
- Known accepted tradeoffs (as in lower_third_slide): `google_fonts_import` lint warning (Poppins resolved deterministically at compile time) and a `system_font_will_alias` info.
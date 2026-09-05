## Why

The user needs a reusable lower-third overlay for a 16:9 (1920×1080) YouTube scene that identifies a repository link, with a transparent background so it can be composited over existing footage. No such overlay composition exists yet in the workspace.

## What Changes

- Create a new HyperFrames composition `lower_third_slide/` in the workspace with:
  - A solid white rounded rectangle bar (bottom-left position)
  - The user-supplied `logo.png` on the left
  - The text `https://github.com/darellanodev/pattern-lab-php` on the right in a thick, slightly rounded sans-serif typeface
  - A transparent background (alpha output for compositing)
- Animate the compact sequence (total ~7s):
  1. Bar slides in from off-screen left (~1s, smooth ease-out)
  2. Bar holds still for 5 seconds
  3. During the hold, the logo sways subtly left-right (rotation sway ±4°, continuous)
  4. Bar fades out entirely (~1s, ease-in)
- Project scaffolding: `package.json`, `meta.json`, `hyperframes.json`, `index.html` with the composition
- Animation is pure CSS + GSAP (standard HyperFrames timeline registered on `window.__timelines`), not Three.js

## Capabilities

### New Capabilities
- `lower-third-slide`: A standalone HyperFrames overlay composition — white rounded lower-third bar with logo + URL text, bottom-left placement, slide-in/hold-with-logo-sway/fade-out choreography, transparent 1920×1080 canvas, MP4 or transparent-alpha output.

### Modified Capabilities
- `robotic-arm-overlay`: No requirement changes. This change only adds a new sibling composition.

## Impact

- New directory `lower_third_slide/` containing `index.html`, `package.json`, `meta.json`, `hyperframes.json`, and the already-provided `logo.png` asset.
- Uses `hyperframes` CLI pinned to `0.7.60` (matching existing workspace projects) for `dev`, `check`, `render`, `publish`.
- Loads the Poppins Bold font from Google Fonts CDN for the URL text.
- No changes to existing compositions (`arm_display_left`, `arm_display_right`), the root `package.json`, or shared infrastructure.
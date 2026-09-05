## Context

The workspace `D:\projects\design\hyperframes-animations` is a collection of independent HyperFrames projects, each a self-contained subdir with its own `package.json`, `meta.json`, `hyperframes.json`, and composition HTML. The existing compositions (`arm_display_left`, `arm_display_right`) use the Three.js adapter with `data-no-timeline` and a manual `hf-seek` render loop. Those are 3D-orientation-specific patterns and do not fit a 2D lower-third overlay.

The target directory `lower_third_slide/` already exists and contains the user-supplied `logo.png`, but no composition files. The deliverable is a 1920×1080 (YouTube 16:9) transparent-background overlay: a white rounded bar, bottom-left, with logo left and a repository URL text right, animated as slide-in → 5s hold (logo sways) → fade-out, total ~7s.

## Goals / Non-Goals

**Goals:**
- Deterministic, seek-safe HyperFrames composition (single paused timeline registered on `window.__timelines`)
- Transparent background (alpha) so the bar composites over existing footage
- 1920×1080, bottom-left placement, ~80px-tall white (#fff) rounded bar
- Logo left, URL text right in a thick round sans-serif (Poppins Bold, dark #333), laid out on the true frame center
- Sequence: slide-in from off-screen left (power2.out, ~1s) → hold 5s with continuous logo rotation sway ±4° (~0.4Hz) → fade-out to transparent (power2.in, ~1s)
- Validates with `hyperframes check` and renders via the pipeline

**Non-Goals:**
- No Three.js / WebGL; this is a flat 2D overlay
- No narration or audio
- No text-variable templating or per-render URL substitution (URL is hardcoded)
- No crowd of content into the right band (captions-overlay style) — the bar is a compact self-contained unit

## Decisions

- **Runtime: pure CSS + GSAP**, not the Three.js pattern used by the arm compositions. A lower-third is flat DOM; injecting a WebGL canvas adds nothing. GSAP is the HyperFrames default adapter and gives the requested eases (`power2.out/in`) directly.
- **Standard timeline, not `data-no-timeline`.** The arm projects opt out because they drive the render themselves. This composition uses a normal paused GSAP timeline registered on `window.__timelines` so the framework owns seeking. No `data-no-timeline` attribute, no `hf-seek` listener, no dynamic `root.dataset.duration` override — duration is declared statically with `data-duration`.
- **Layout anchoring: fixed `data-width/data-height` (1920×1080) with the bar positioned bottom-left.** Bar uses absolute positioning inside `#root`: `bottom: 40px; left: 40px` (plus the slide-in offset) so placement is exact in the 1920×1080 canvas. Text baseline is centered with the bar via an inner flex row.
- **Logo sway as GSAP rotation tween (yoyo/alternate), deterministic.** A `repeat: -1, yoyo: true` tween on rotation between −4° and +4° over ~1.25s per half-cycle (~0.4Hz), wrapped inside the timeline's hold window so it stops when the fade begins. Deterministic (no `Date.now`/`Math.random`), satisfying the determinism rule.
- **Timeline structure via one master timeline**:
  - Scene 1 `data-start="0" data-duration="1"` — bar slides from `x: −(barWidth)` to `x: 0`, ease `power2.out`.
  - Scene 2 `data-start="1" data-duration="5"` — bar static; logo rotation sway runs the full 5s.
  - Scene 3 `data-start="6" data-duration="1"` — bar opacity `1 → 0`, ease `power2.in`.
  - Total 7s (`data-duration="7"`). All timed elements carry `class="clip"` plus `data-start`/`data-duration`/`data-track-index`.
- **Fonts: Google Fonts Poppins (weight 700/800).** Thick, slightly rounded sans-serif. Loaded via a `<link>`; `font-display: block` not required but fallback stack to system sans-serif included so rendering is never blocked.
- **Alpha output:** composition renders transparent by default (no background fill on `#root`); release targets transparent WebM/MOV for compositing or MP4 with an explicit background if the user overlays on a known color.
- **Project scaffolding mirrors existing workspace projects:** `package.json` (scripts `dev`/`check`/`render`/`publish`, devDependency `hyperframes` pinned `0.7.60`), `meta.json` `{id, name, createdAt}`, `hyperframes.json` with `paths` for blocks/components/assets.

## Risks / Trade-offs

- **Pinned CLI 0.7.60 is older than latest.** Existing projects pin it and the workflow says the pin never advances on its own. → Keep the pin for reproducibility; run the read-only `npx hyperframes@latest upgrade --project . --check` probe before the first render and report if a bump is warranted.
- **Google Fonts load is a network dependency during render.** Deterministic-render rules imply deterministic layout; a font loading after first paint can shift text width. → The injected GSAP timeline and seek behavior are deterministic, and layout is static after the initial render; rendered output uses the fully-loaded font cache. If font race becomes visible, inline or bundle the font subset.
- **Transparent WebM/MOV alpha may not be supported by every target editor/player.** → Provide MP4 as fallback; note in the composition that the transparent background only exists when the volume/alpha track is preserved.
- **Text overflow if the URL is long.** At 24px Poppins Bold the URL is ~33 chars; the 1920px canvas with a bottom-left bar has ample room. → Verified during the build via `hyperframes check`; if it approaches the right edge, reduce letter-spacing or font-size.

## Migration Plan

- New, isolated project subdir — no migration. If changes are needed, delete/regenerate the subdir; nothing else in the workspace is touched.
- Rollback = remove `lower_third_slide/` and the related change artifacts.
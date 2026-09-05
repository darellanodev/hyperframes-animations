## Context

A steps cartel animation ("cartel de pasos") is needed for a HyperFrames video. A static HTML mockup (tmp-prompt.txt) shows a white bottom panel with three numbered circles (32px, joined by thin connectors), each with a "Paso N" label, over a gray page, plus a two-line description using the brand's code styling for `Character`. The user wants this as a HyperFrames overlay: the gray page becomes transparency, and the animation communicates a 3-step tutorial progression (Fluent Interface Pattern for a `Character` class). This mirrors the existing `lower_third_slide/` project (pure CSS + GSAP, pinned `hyperframes@0.7.60`, transparent 1920×1080 canvas, deterministic single paused GSAP timeline).

## Goals / Non-Goals

**Goals:**
- Transparent 1920×1080 overlay composition in a new `panel_steps/` project (package.json/meta.json/hyperframes.json/index.html).
- Bottom-centered white card with three numbered step circles joined by connectors, per-step labels, and a per-step description; only the active step circle is brand-blue and flashes.
- Deterministic choreography: three blocks of 6.2s each (total 18.6s). Each block fades the whole card in (0.6s), holds ~5s while the active circle pulses, then fades the whole card out (0.6s). Between blocks nothing is visible.
- Pass `hyperframes check` with 0 errors; output must preserve real alpha (transparent background) when rendered.

**Non-Goals:**
- No Three.js/WebGL; 2D only.
- No narration, no audio, no interactive behavior.
- No modification to any existing capability or composition (robotic-arm-overlay, lower_third_slide untouched).
- Not a slideshow/deck; the card's three blocks always play once in order.

## Decisions

**1. Pure CSS + GSAP (default runtime), not Three.js.**
Like `lower_third_slide/`, the content is flat 2D DOM — GSAP timeline is the right tool. Single `gsap.timeline({ paused: true })` registered synchronously as `window.__timelines["panel-steps"]`; no `data-no-timeline`, no `tl.play()`, no hf-seek listener. Render duration = root `data-duration="18.6"`.

**2. One card element whose opacity is the only "scene change" mechanism.**
A single wrapper `#card` holds the whole cartel (indicator row + description). Per block: fade card in (opacity 0→1, 0.6s, power2.out), recolor the active circle via `tl.set` (frame-correct because it happens while the card is still invisible at opacity 0), run the active circle's pulse for 5s, then fade card out (opacity 1→0, 0.6s, power2.in). All three description `<p>` coexist in the DOM; only one is effectively visible per block since the card is otherwise at opacity 0. This avoids any display/visibility tweens and keeps the framework the sole owner of clip visibility.

**3. Data-driven step pivot via three identical sub-timelines at offsets k×6.2s (k=0,1,2).**
Position parameter on the master timeline: block k starts at `k * 6.2`. Each block animates the SAME `#card` element for fade and the block's OWN circle element (`#circle-1|2|3`) for color/pulse. Inactive circles are white `#fff` with `0.5px solid #d0d0d0` border; active circle is `#1d6fd8` (brand blue). Because the fade opacity is a fromTo on a fresh element state each block, use `immediateRender: false` on all fromTo tweens after the first block to avoid mutating earlier-keyframe visuals.

**4. Pulse = scale "destello" + halo, finite repeats, yoyo.**
Each active circle pulses ~5 times over its 5s hold: `scale 1→1.12`, `duration 0.5`, `ease sine.inOut`, `repeat` finite/even count with `yoyo: true`; a soft glow uses box-shadow color `.2s` re-tween (or `filter: drop-shadow`) on the same timing. Deterministic (no `repeat: -1`); compute repeat as `floor(holdDuration / cycleDuration / 2) * 2 - 1` to end at the resting scale.

**5. Font = Poppins (Google Fonts), same accepted tradeoff as lower_third_slide.**
Google `fonts.googleapis` `<link>` for Poppins weights 600/700 + jsdelivr `gsap@3.14.2`. The `google_fonts_import` lint warning is accepted (compiler resolves deterministically); `system_font_will_alias` info is expected.

**6. Canvas, layout, and scale approach the mockup at 1080p depth.**
Root: `data-composition-id="panel-steps" data-start="0" data-width="1920" data-height="1080" data-duration="18.6"`, `background: transparent`, `overflow: hidden`. Card absolute `bottom: 80px`, centered via flex or `left: 50%; transform: translateX(-50%)`. Circles ≈ 120px diameter, connectors ≈ 150px wide × 2px, labels ≈ 30px, description ≈ 32px; card padding generous to match the mockup proportions. All pre-calculated constants (no `getBoundingClientRect` at tween time).

**7. Scaffolding mirrors lower_third_slide exactly.**
package.json (`name panel_steps`, type module, scripts dev/check/render/publish = `hyperframes <cmd>`, devDependency `hyperframes` pinned `0.7.60`), meta.json `{id: panel_steps, name: Panel Steps, createdAt: <now ISO>}`, hyperframes.json with the standard schema + registry + paths. After authoring: `rtk pnpm run check` must pass 0 errors.

## Risks / Trade-offs

- **Google font fetch at render** → accepted tradeoff (identical to lower_third_slide); the compiler resolves the stylesheet deterministically at compile/render time. Fallback `sans-serif` defined so even a font failure renders legibly.
- **Whole-card opacity choreography hides nothing else** → by design the user asked that "nothing is visible" between blocks; the three-block cadence (6.2s each, 18.6s total) is symmetric and frame-deterministic.
- **`immediateRender` mutation across blocks** → mitigated by `immediateRender: false` on block 2+ fromTo tweens and by recolor via `tl.set` only at the block start (card at opacity 0).
- **Connector/circle alignment at 1080p depth** → use flex with fixed `gap` and `align-items: center`; lint Layout/Contrast pass is the gate (as in lower_third_slide: Layout 0 issues, Contrast 4/4 WCAG AA expected).
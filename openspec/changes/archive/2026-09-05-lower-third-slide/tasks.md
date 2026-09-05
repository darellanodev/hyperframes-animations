## 1. Project scaffolding

- [x] 1.1 Create `lower_third_slide/package.json` mirroring existing workspace projects (name `lower_third_slide`, private, type module, scripts `dev`/`check`/`render`/`publish`, devDependency `hyperframes` pinned `0.7.60`)
- [x] 1.2 Create `lower_third_slide/meta.json` (`{"id": "lower_third_slide", "name": "Lower Third Slide", "createdAt": <now ISO>}`)
- [x] 1.3 Create `lower_third_slide/hyperframes.json` (same schema/registry/paths as existing workspace projects)
- [x] 1.4 Run `pnpm install` in `lower_third_slide/` (or `npm install` if pnpm unavailable) to install the pinned `hyperframes` CLI
- [x] 1.5 Verify `logo.png` exists in `lower_third_slide/` and inspect its aspect ratio for layout sizing

## 2. Composition HTML

- [x] 2.1 Create `lower_third_slide/index.html` with `#root` at 1920×1080 (`data-width`/`data-height`), transparent background (no background fill), `data-composition-id="lower-third-slide"`, `data-duration="7"`, and no `data-no-timeline`
- [x] 2.2 Add Google Fonts Poppins (Bold / ExtraBold) `<link>` plus a dark-sans fallback stack
- [x] 2.3 Build the bar DOM: absolute-positioned white (`#fff`) rounded rectangle (~80px tall, ~8px radius), bottom-left margin (~40px from bottom and left), inner flex row with `logo.png` (left) and URL text `https://github.com/darellanodev/pattern-lab-php` (right, Poppins Bold ~24px, `#333`), logo vertically centered with approx. 30px left padding
- [x] 2.4 Declare `data-start`/`data-duration`/`data-track-index` (and `class="clip"`) on the timed scene elements: entry scene (0–1s), hold scene (1–6s), exit scene (6–7s)

## 3. Animation

- [x] 3.1 Register a single paused GSAP timeline on `window.__timelines["lower-third-slide"]` (no `data-no-timeline`, framework-owned seeking)
- [x] 3.2 Entry tween: bar slides from off-screen left (`x: −barWidth`) to resting position over ~1s, ease `power2.out`
- [x] 3.3 Hold + sway: bar static for 5s while the logo runs a deterministic continuous rotation sway (yoyo, ±4°, ~0.4Hz) confined to the hold window (1–6s); sway stops when the fade begins
- [x] 3.4 Exit tween: bar opacity `1 → 0` over ~1s, ease `power2.in` (6–7s); ensure a clean opaque stage-ground (#root) so no white flash artifacts occur at the seam
- [x] 3.5 Confirm the timeline is paused by default so the composition is seek-safe and deterministic

## 4. Validation

- [x] 4.1 Run the read-only pin probe once: `npx hyperframes@latest upgrade --project . --check` (report any newer CLI; keep the pinned version unless a bump is warranted) — ran via `pnpm dlx hyperframes@latest upgrade --project . --check` (npx is broken in this shell); latest 0.8.x available, project stays pinned on 0.7.60
- [x] 4.2 Run `pnpm run check` inside `lower_third_slide/`; fix all errors and review warnings — 0 errors; sole remaining warning is accepted `google_fonts_import`; layout overflow infos resolved via `data-layout-allow-overflow` on the intentionally off-canvas entry slide
- [x] 4.3 Run `hyperframes lint --verbose` and resolve any motion/layout/contrast findings relevant to the overlay — 0 errors (1 accepted warning, 1 info); contrast 4/4 WCAG AA
- [x] 4.4 Optionally render a preview frame via `hyperframes render` and confirm transparent background + bottom-left bar + logo/text layout — rendered 14 RGBA frames (2fps draft png-sequence); all colorType=6 (alpha channel present); frame-size progression matches entry→hold→fade; `check` Motion 0 errors + Contrast 4/4
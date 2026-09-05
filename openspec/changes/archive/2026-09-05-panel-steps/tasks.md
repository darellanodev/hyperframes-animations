## 1. Project scaffolding

- [x] 1.1 Create `panel_steps/package.json` mirroring `lower_third_slide/` (name panel_steps, type module, scripts dev/check/render/publish = `hyperframes <cmd>`, devDependency `hyperframes` pinned `0.7.60`)
- [x] 1.2 Create `panel_steps/meta.json` `{id: panel_steps, name: <Human-readable name>, createdAt: <now ISO>}`
- [x] 1.3 Create `panel_steps/hyperframes.json` with the standard schema/registry/paths (same shape as `lower_third_slide/`)

## 2. Composition HTML (index.html)

- [x] 2.1 Create `panel_steps/index.html` with head: Poppins Google Fonts `<link>` (weights 600/700, plus preconnect) and jsdelivr `gsap@3.14.2` script
- [x] 2.2 Root element: `<div>` with `data-composition-id="panel-steps"`, `data-start="0"`, `data-width="1920"`, `data-height="1080"`, `data-duration="18.6"`, transparent background, `overflow: hidden`; NO `data-no-timeline`; root sized 1920×1080
- [x] 2.3 Single clip wrapper `#card` with `class="clip"`, `data-start="0"`, `data-duration="18.6"`, `data-track-index="0"`, positioned absolute bottom-centered (e.g. bottom ~80px, horizontally centered), white background, fully rounded, Poppins 700, flex column, `data-layout-allow-overflow` where needed
- [x] 2.4 Inside the card: step-indicator row (flex, centered, gap) with three columns, each a circle (`#circle-1|2|3`, ~120px, border-radius 50%, transform-origin center) + "Paso N" label (~30px, Poppins, #333/#666), columns joined by `#connector-1|2` (~150px × 2px, #d0d0d0)
- [x] 2.5 Inside the card: three description `<p>`s (`#desc-1|2|3`, ~32px, #000, line-height ~1.6, centered, margin 0) with `Character`/`Fluent Interface Pattern` marked as `<code>` (mono, ~26-28px, #f2f2f2 bg, rounded); all three coexist in the DOM; no `<br>`
- [x] 2.6 Inactive-circle baseline CSS: `#circle-1|2|3` default white bg with `0.5px solid #d0d0d0` border; active color #1d6fd8 applied only via timeline `tl.set` (no CSS transform on elements that GSAP tweens)

## 3. Animation (single paused GSAP timeline)

- [x] 3.1 Inline script registers `window.__timelines` and builds exactly one `gsap.timeline({ paused: true })`, assigned at `window.__timelines["panel-steps"]`; built synchronously; no `tl.play()`
- [x] 3.2 Per-block choreography for block k (start = `k * 6.2`): fade card in via `tl.fromTo("#card", {opacity:0},{opacity:1, duration:0.6, ease:"power2.out"}, start)` with `immediateRender:false` on blocks 2+; `tl.set` active circle background to #1d6fd8 (and others back to #fff) at start while card is at opacity 0
- [x] 3.3 Active circle pulse for each block across the 5s hold: `tl.fromTo("#circle-k", {scale:1},{scale:1.12, duration:0.5, ease:"sine.inOut", repeat:<finite>, yoyo:true, immediateRender:false}, start+0.6)` plus a soft glow re-tween on box-shadow color; finite repeat (no `repeat:-1`), deterministic
- [x] 3.4 Fade card out per block: `tl.to("#card", {opacity:0, duration:0.6, ease:"power2.in"}, start+5.6)`; blocks at 0, 6.2, 12.4 → total 18.6s
- [x] 3.5 Confirm no render-time clock/random/network in the composition; all timing pre-calculated constants (no getBoundingClientRect at tween time); no width/height/top/left tweens on clips

## 4. Validation

- [x] 4.1 Probe pinned CLI once: `rtk pnpm dlx hyperframes@latest upgrade --project . --check` in `panel_steps/` (report pin vs latest; remain on 0.7.60 per tasks unless bump required)
- [x] 4.2 Run `rtk pnpm run check` in `panel_steps/` and confirm 0 errors (expected non-blocking: `google_fonts_import` warning, `system_font_will_alias` info; Layout 0 issues, Motion 0, Contrast pass)
- [x] 4.3 Fix any lint/runtime/layout/contrast findings and re-check until clean
- [x] 4.4 Optional: render a draft to verify transparent output (`rtk pnpm exec hyperframes render . --format png-sequence --fps 2 --quality draft --output renders/preview`) and confirm alpha frames
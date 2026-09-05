## ADDED Requirements

### Requirement: Transparent canvas with bottom-centered card
The `panel-steps` composition SHALL render on a transparent 1920×1080 canvas with a white card positioned at the bottom, horizontally centered. The gray page from the mockup SHALL NOT appear (replaced by transparency).

#### Scenario: Canvas is transparent
- **WHEN** the composition is rendered as an overlay
- **THEN** every pixel outside the card has full transparency (alpha 0)

#### Scenario: Card is bottom-centered
- **WHEN** a frame is inspected
- **THEN** the card is horizontally centered near the bottom of the 1920×1080 frame (consistent margin above the bottom edge, matching the mockup proportions scaled to 1080p)

### Requirement: Card composition mirrors the steps mockup
The card SHALL contain a step-indicator row with three numbered circles joined by thin connectors, each circle with a small "Paso N" label beneath it, and a per-step description line below the row. The step 1 description SHALL render the token `Character` with code styling.

#### Scenario: Three circles with labels and connectors
- **WHEN** the card is visible
- **THEN** three circular step indicators (numbered 1, 2, 3) are shown in a horizontal row, joined by connectors, each labeled with its step name

#### Scenario: Description with code-styled token
- **WHEN** step 1 block is visible
- **THEN** the description reads "Paso 1: Creamos la clase Character de nuestro personaje (todavía sin el patrón Fluent Interface)" and the token `Character` is styled as code

### Requirement: Three-block choreography with whole-card fade
The composition SHALL play three sequential blocks of 6.2 seconds each (18.6s total). In each block the entire card — including the three step circles and their labels — fades in from opacity 0 over 0.6s, holds for 5 seconds, then fades out to opacity 0 over 0.6s. Between blocks nothing is visible.

#### Scenario: Block cadence
- **WHEN** the timeline is played from 0s
- **THEN** block 1 runs 0–6.2s, block 2 runs 6.2–12.4s, and block 3 runs 12.4–18.6s, each starting with a 0.6s fade-in and ending with a 0.6s fade-out of the whole card

#### Scenario: Card fades as a unit
- **WHEN** the card fades in or out during any block
- **THEN** the three step circles fade together with the rest of the card, so at block boundaries nothing on the card is visible

### Requirement: Active step circle highlight and pulse
During each block the circle of the currently presented step SHALL be brand blue (#1d6fd8) and pulse repeatedly (a scale flash with a soft glow, yoyo, finite number of cycles) across the 5s hold; the other two circles SHALL stay white with a light gray border.

#### Scenario: Only active circle pulses per block
- **WHEN** block k (k=1,2,3) is in its hold phase
- **THEN** only circle k is blue and pulsing, while the other two circles remain white with the #d0d0d0 border

#### Scenario: Active color applied invisibly between blocks
- **WHEN** the card transitions from block k to block k+1
- **THEN** circle k+1 turns blue (and circle k returns to white) while the card is still at opacity 0, so the swap is not seen

### Requirement: Per-step content is fixed
The card SHALL display exactly these three descriptions, one per block:
1. "Paso 1: Creamos la clase Character de nuestro personaje (todavía sin el patrón Fluent Interface)"
2. "Paso 2: Implementamos el código cliente y al final empezamos a pensar en Fluent Interface Pattern"
3. "Paso 3: Modificamos la clase para usar Fluent Interface Pattern"

#### Scenario: Description matches its block
- **WHEN** block k is visible
- **THEN** the description displayed on the card is text k from the fixed list above

### Requirement: Deterministic seek-safe timeline with project scaffolding
The composition SHALL be built as a standalone HyperFrames project with a single paused, synchronous GSAP timeline registered at `window.__timelines["panel-steps"]`, every timed element carrying `class="clip"` plus `data-start`, `data-duration`, and `data-track-index`, a root element with `data-composition-id="panel-steps"`, `data-start="0"`, `data-width="1920"`, `data-height="1080"`, and `data-duration="18.6"`, and `data-no-timeline` absent. There SHALL be no `tl.play()`, no `repeat: -1`, and no render-time clocks/randomness. The project's scaffolding (package.json, meta.json, hyperframes.json) SHALL mirror the `lower_third_slide/` project with `hyperframes` pinned at `0.7.60`, and `hyperframes check` SHALL pass with 0 errors.

#### Scenario: Timeline is registered and paused
- **WHEN** the composition loads
- **THEN** exactly one GSAP timeline is registered at `window.__timelines["panel-steps"]`, is paused, and is built synchronously without async work

#### Scenario: Validation passes
- **WHEN** `hyperframes check` runs in `panel_steps/`
- **THEN** it reports 0 errors (known accepted warnings: `google_fonts_import` warning and `system_font_will_alias` info)
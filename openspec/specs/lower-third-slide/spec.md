# lower-third-slide

## Purpose

TBD

## Requirements

### Requirement: Lower-third bar layout
The composition SHALL render a transparent 1920×1080 (YouTube 16:9) canvas containing a single white (`#fff`) solid rounded rectangle bar (approx. 80px tall, rounded corners) positioned at the bottom-left of the frame with a margin from the screen edges. The bar SHALL contain the user-supplied `logo.png` on the left and the text `https://github.com/darellanodev/pattern-lab-php` on the right, in a thick, slightly rounded sans-serif typeface (Poppins Bold, dark `#333`). The bar SHALL NOT occupy or reserve a full-width bottom band.

#### Scenario: Transparent canvas
- **WHEN** the composition is rendered to an alpha-capable format (WebM/MOV with alpha)
- **THEN** everything outside the bar and its contents is transparent

#### Scenario: Bottom-left placement
- **WHEN** the composition is inspected at its resting position
- **THEN** the bar sits in the lower-left region of the 1920×1080 canvas, near the bottom/side edges, and does not span the full frame width

#### Scenario: Logo and URL content
- **WHEN** the composition is rendered at rest
- **THEN** `logo.png` appears at the left inside the bar and the text `https://github.com/darellanodev/pattern-lab-php` appears to its right in a thick, slightly rounded dark sans-serif

### Requirement: Slide-in entry
The bar SHALL enter the frame sliding in from off-screen left to its resting position with a smooth ease-out motion lasting approximately 1 second.

#### Scenario: Bar slides in from off-screen left
- **WHEN** the timeline plays from time 0 to the end of the entry phase
- **THEN** the bar begins fully off-screen on the left and translates horizontally to its resting bottom-left position with an ease-out curve, completing around time 1s

### Requirement: Five-second static hold with logo sway
After entry, the bar SHALL remain static in its resting position for 5 seconds. During this hold, the logo SHALL sway subtly and continuously (pendulum-style rotation sway of approximately ±4°).

#### Scenario: Bar remains static during hold
- **WHEN** the timeline is between approximately 1s and 6s
- **THEN** the bar's position does not change while the hold runs

#### Scenario: Logo sways continuously
- **WHEN** the timeline is between approximately 1s and 6s
- **THEN** the `logo.png` element rotates continuously between approximately −4° and +4° about its center, repeating back and forth for the entire hold

#### Scenario: Sway stops at fade
- **WHEN** the timeline reaches the exit phase
- **THEN** the logo sway is no longer active and the whole bar begins to fade out

### Requirement: Fade-out exit
The bar SHALL disappear at the end of the sequence via a full fade-out (opacity to 0) lasting approximately 1 second. The total composition duration SHALL be approximately 7 seconds.

#### Scenario: Full fade to transparent
- **WHEN** the timeline plays from approximately 6s to 7s
- **THEN** the entire bar and its contents transition opacity to 0 with an ease-in curve, ending fully transparent at ~7s

#### Scenario: Declared total duration
- **WHEN** the composition is validated
- **THEN** its declared duration is approximately 7s and matches the sum of the entry, hold, and fade phases

### Requirement: Deterministic seek-safe timeline
The composition SHALL be deterministic and seek-safe: it SHALL use a single paused GSAP timeline registered on `window.__timelines` and declare timing with `data-start`, `data-duration`, and `data-track-index` on every timed element (each carrying `class="clip"`). The composition SHALL NOT use nondeterministic logic (`Date.now`, `Math.random`, network-dependent layout) and SHALL NOT use the `data-no-timeline` opt-out.

#### Scenario: Timeline registered for framework seeking
- **WHEN** the composition loads
- **THEN** a paused GSAP timeline is registered on `window.__timelines` and the framework can seek it by time

#### Scenario: All timed elements declared
- **WHEN** the composition HTML is checked
- **THEN** every timed element declares `data-start` and `data-duration` (and `data-track-index`) and carries `class="clip"`

### Requirement: Project scaffolding
The `lower_third_slide/` project SHALL include `index.html`, `package.json`, `meta.json`, and `hyperframes.json`, mirroring the existing workspace projects (scripts `dev`/`check`/`render`/`publish`; `hyperframes` pinned at `0.7.60`) and SHALL pass `hyperframes check`.

#### Scenario: Validated project
- **WHEN** `hyperframes check` (or `pnpm run check`) runs inside `lower_third_slide/`
- **THEN** the project validates without errors

# Gap Decorations v2: Slide Content

## Slide 1: Title
**Pillars:** —
**Part:** —
**Label:** BlinkOn 2026

# StoryTime: CSS Gap Decorations
From spec proposal to cross-layer implementation in Blink

| Stat | Value |
|------|-------|
| CLs | 191 |
| Spec PRs | 22 |
| CSSWG Issues | 30 |

Javier Contreras · Sam Davis Omekara
Microsoft Web Platform

---

## Slide 2: Containers
**Pillars:** —
**Part:** 1 — The Origin
**Label:** The Building Blocks

### Containers

On the Web, container types help developers build the layouts they want:

- **Flex**: 1D placement. Items flow in a single direction.
- **Grid**: 2D placement. Items placed across rows and columns.
- **Multicol**: triggered not by `display`, but by specifying the number of columns

---

## Slide 3: The Problem
**Pillars:** CSSWG
**Part:** 1 — The Origin
**Label:** The Problem

### Gutters exist, but only one container could style them

- These containers can have **gutters**, but only **Multicol** had a way to style them: the `column-rule` property.
- `column-rule` lets you set style, color, and width, similar to borders.
- People wanted the same thing for Flex and Grid.
- **June 7, 2018**: CSSWG #2748 ("Styling Gaps/Gutters") is filed. The idea sat for six years. Lots of discussion, no spec.

**Links:** [CSSWG #2748](https://github.com/w3c/csswg-drafts/issues/2748)

---

## Slide 4: The Spec
**Pillars:** CSSWG | Design
**Part:** 1 — The Origin
**Label:** The Spec

### From draft to editor's draft

- Mats Palmgreen (Mozilla) took the first stab. **Kevin Babbitt** from our team at Microsoft picked it up and wrote it. Three things he had in mind:
  1. **Author use cases**. Grounded in real developer needs, with references collected in the Explainer.
  2. **Precedents**. Existing `column-rule` support, `repeat()` from Grid, and list-ified properties.
  3. **Scope and extensibility**. What ships in V1 vs. what gets deferred, shaped to leave room for future levels.

**Diagram:** the 3-card grid uses real gap decorations (dashed column-rule, rule-break: intersection, rule-inset: 0).

**Links:**
- [CSSWG #10393](https://github.com/w3c/csswg-drafts/issues/10393)
- [PR #11115](https://github.com/w3c/csswg-drafts/pull/11115)

---

## Slide 5: The Initial Feature
**Pillars:** CSSWG | Design
**Part:** 1 — The Origin
**Label:** The Initial Feature

### What the spec gave us

CSS Gap Decorations Level 1 added three things on top of basic `column-rule`.

- **Varying Styles**: List-valued properties let each gap carry a different style, width, and color, for both rows and columns.
- **Rule Break**: Control how decorations break at intersections: `none`, `spanning-item`, or `intersection`.
- **Rule Inset**: Shorten or extend decorations from gap edges with positive or negative inset values.

**Diagram:** Three cards with visual vignettes — varying line styles, a grid with break at intersection, and an inset rule with arrows.

**Links:**
- [w3.org/TR/css-gaps-1](https://www.w3.org/TR/css-gaps-1/)

---

## Slide 6: Three Layers
**Pillars:** Design | Implementation
**Part:** 1 — The Origin
**Label:** The Feature in Blink

### Style → Layout → Paint

While Kevin was writing the spec, we started getting familiar with Blink core. Gap decorations touch three layers:

- **Style**: parse the properties (`column-rule`, `row-rule`, shorthands, repeaters, lists)
- **Layout**: compute gap geometry for grid, flex, and multicol
- **Paint**: draw decorations with correct clipping, overflow, and scrolling

**Diagram:** BLINK RENDERING PIPELINE — Style → Layout → Paint layer stack

---

## Slide 7: Transition
**Pillars:** —
**Part:** Transition

Once we had an initial spec and implementation, interesting questions started coming up. Re: Standards.

---

## Slide 8: Working with Standards
**Pillars:** CSSWG | Design
**Part:** 2 — Iteration
**Label:** Working with Standards

### The spec evolved alongside the code

- Implementation surfaced questions. We filed issues. We worked with the CSSWG to resolve them. We updated the code.
- We tried to treat the WG as a partner, not a checkpoint to get past.
- A few of them:
  - #11492: Auto repeater behavior (✓ Feb 1, 2025)
  - #11494: Computed value with none/hidden (✓ Nov 11, 2025)
  - #11496: Shorthand grammar (✓ Apr 12, 2025)
  - #12024: Outsets at edges (✓ Dec 17, 2025)
  - #12540: Renamed rule-paint-order → rule-overlap (✓ Aug 8, 2025)
- **30 issues filed. 24 resolved.**
**Links:**
- [#11492](https://github.com/w3c/csswg-drafts/issues/11492)
- [#11494](https://github.com/w3c/csswg-drafts/issues/11494)
- [#11496](https://github.com/w3c/csswg-drafts/issues/11496)
- [#12024](https://github.com/w3c/csswg-drafts/issues/12024)
- [#12540](https://github.com/w3c/csswg-drafts/issues/12540)

---

## Slide 9: Gap Suppression & Fragmentation
**Pillars:** CSSWG | Implementation
**Part:** 2 — Iteration
**Label:** Fragmentation

### Gap suppression across fragment breaks

- When a container is **fragmented** (e.g. across pages or columns), gaps can land right at a break point.
- CSSWG #11520 resolved: **suppress gaps at fragment breaks**. The break itself already acts as a visual separator, so drawing a decoration there is redundant.
**Links:**
- [CSSWG #11520](https://github.com/w3c/csswg-drafts/issues/11520)

---

## Slide 10: The Great Rewrite — The Old Model
**Pillars:** Implementation
**Part:** 2 — Iteration
**Label:** The Great Rewrite

### The Old Model

- **Intersections as first-class objects.** Each gap stored a list of intersection points.
- 2D vectors of gaps × intersections.
- Storage: O(m × n). Every intersection stored as an (x, y) pair.
- Simple, but expensive at scale.

**Memory at O(m × n):**
| Grid Size | Memory |
|-----------|--------|
| 100 × 100 | 160 KB |
| 1,000 × 1,000 | 16 MB |
| 5,000 × 5,000 | 400 MB |

**Diagram:** "Before: 12 pairs" — a 3×3 grid with every intersection marked as a red cross, shown next to the memory cost callouts.

---

## Slide 11: The Great Rewrite — Why Grid Forced the Issue
**Pillars:** Implementation
**Part:** 2 — Iteration
**Label:** The Great Rewrite

### Why Grid forced the issue

- In Blink, **gaps were throw-away values**. Their positions were never stored. Layout computed them, used them, and discarded them.
- Grid tracks live as **compressed abstractions** (`GridRanges`, `GridSets`) to avoid expanding repeaters into individual positions.
- But to draw decorations, we had to expand every track to find where gaps lived. The result? Every intersection stored as an (x, y) pair, the O(m × n) cost from the previous slide.

**Diagram:** Split layout. Left: text. Right: two stacked cards — "Grid's world: compressed" showing 2 ranges each containing 2 sets as (offset, count) pairs vs. "What we needed: expanded" showing every track position enumerated.

---

## Slide 12: The Great Rewrite — Anchor/Dependent Model
**Pillars:** Implementation
**Part:** 2 — Iteration
**Label:** The Great Rewrite

### MainGap / CrossGap model

- **Two-direction model.** Anchor Gaps (primary) plus Dependent Gaps (orthogonal).
- Intersections computed **on-demand**, not stored.
- Storage: O(m + n).
- CL 6819000: introduced separate Main and Cross gap classes.
- About 75% memory reduction on grid, 50% on flex.

**Diagram:** Before: 12 pairs — every intersection stored as (x, y). After: 2 + 2 offsets — column offsets + row offsets, stored separately.

**Links:**
- [CL 6819000](https://chromium-review.googlesource.com/c/chromium/src/+/6819000)

---

## Slide 13: Do Not Expand (Except Animations)
**Pillars:** Implementation
**Part:** 2 — Iteration
**Label:** Property Lists

### Do not expand, except for animations

- `GapDataListIterator` walks property lists **without expanding** them.
- Three regions: **Leading**, **Auto**, **Trailing**.
- Animations (CSSWG #12431) are the exception. They need expansion. LCM alignment for interpolating lists of different lengths.

**Example:** interpolating `10px repeat(3, 5px 1px) 10px` to `5px 10px` — expand both, align to LCM, then interpolate.

**Animation demo caption:** column-rule-color, column-rule-width, rule-inset: animating

**Links:**
- [CL 6773829](https://chromium-review.googlesource.com/c/chromium/src/+/6773829)
- [CL 6961880](https://chromium-review.googlesource.com/c/chromium/src/+/6961880)
- [CL 7078855](https://chromium-review.googlesource.com/c/chromium/src/+/7078855)
- [CSSWG #12431](https://github.com/w3c/csswg-drafts/issues/12431)

---

## Slide 14: Paint Land — Fun but Subtle
**Pillars:** Implementation
**Part:** 2 — Iteration
**Label:** Into Unfamiliar Territory

### Paint Land: fun but subtle

Geometry from layout → rects → paint ops (`BoxFragmentPainter`). Four subtleties we had to figure out:

- **Ink overflow**: decorations extend past the content box
- **Clipping**: scrollable containers clip correctly at edges
- **Invalidation**: knowing what to repaint when state changes
- **Resize**: decorations reflow when the layout changes

Big shoutout to **Philip Rogers (pdr)**, a paint owner. He walked us through many rounds of review.

**Diagram:** four small vignettes — Ink overflow, Clipping, Invalidation, Resize — each a tiny illustration.

**Links:**
- [CL 6101656](https://chromium-review.googlesource.com/c/chromium/src/+/6101656)
- [CL 6451833](https://chromium-review.googlesource.com/c/chromium/src/+/6451833)
- [CL 6506379](https://chromium-review.googlesource.com/c/chromium/src/+/6506379)
- [CL 6625941](https://chromium-review.googlesource.com/c/chromium/src/+/6625941)
- [CL 6618134](https://chromium-review.googlesource.com/c/chromium/src/+/6618134)
- [CL 6907139](https://chromium-review.googlesource.com/c/chromium/src/+/6907139)
- [CL 6848348](https://chromium-review.googlesource.com/c/chromium/src/+/6848348)

---

## Slide 15: Transition
**Pillars:** —
**Part:** Transition

### But who are we building for?

Dev trial launched in Chrome/Edge 139, June 2025. We put the feature behind a flag and handed it to the people who'd actually use it.

---

## Slide 16: Feedback That Changed the Spec
**Pillars:** CSSWG | Design
**Part:** 3 — Resolving Loose Ends
**Label:** What Developers Told Us

### Feedback that changed the spec

12 issues from 11 developers. Some drove spec changes. Others became future-level ideas.

**Incorporated:**

| Source | Feedback | Outcome |
|--------|----------|---------|
| AHMAD SHADEED | Decorations around empty grid areas — responsive layouts leave cells empty, rules shouldn't appear there | ✓ NEW PROPERTY: `rule-visibility-items` |
| REXGALILAE | Asymmetric insets on each side — different inset values for start and end, unlocked new layout patterns | ✓ RESTRUCTURED: `rule-inset` properties |
| NAMING FEEDBACK | Property naming clarity — developer confusion around rule-break, rule-overlap, and shorthand grammar | ✓ INCORPORATED |

**Pushed to future:**

| Source | Feedback | Status |
|--------|----------|--------|
| SMASHINGCONF FREIBURG | Gradients and images as decorations — like border-image but for gaps. Dots, symbols, patterns between items. | ▶ DEFERRED |
| ALICO-CRA | Ornaments at crossing points — styling where row and column rules intersect | ▶ DEFERRED |
| PER-SIDE STYLING | Per-side independent styling — different decoration styles on each side of an item, like border shorthand granularity | ▶ DEFERRED |

**Quotes:**
- "This made my day!" — RexGalilae, on inset changes
- "Works well and is a great addition to the spec." — o-t-w, on rule-visibility-items

"Not 'no' — 'not yet.' They become the roadmap."

**Links:**
- [MSEdge #1099](https://github.com/MicrosoftEdge/MSEdgeExplainers/issues/1099)
- [MSEdge #996](https://github.com/MicrosoftEdge/MSEdgeExplainers/issues/996)
- [MSEdge #1064](https://github.com/MicrosoftEdge/MSEdgeExplainers/issues/1064)
- [MSEdge #988](https://github.com/MicrosoftEdge/MSEdgeExplainers/issues/988)
- [MSEdge #984](https://github.com/MicrosoftEdge/MSEdgeExplainers/issues/984)
- [MSEdge #985](https://github.com/MicrosoftEdge/MSEdgeExplainers/issues/985)

---

## Slide 17: From Intersections to Segments
**Pillars:** CSSWG | Design
**Part:** 3 — Resolving Loose Ends
**Label:** The Model Was Grid-specific

### From intersections to segments

- The intersection model leaned too heavily on grid. It didn't generalize for multicol and flex.
- The spec moved to a **segments-based model**. Biggest spec rewrite since the initial draft.

**Diagram:** Old: Intersections (grid-style cross marks at every junction). New: Segments (paired segment endpoints per rule line).

**Links:**
- [#12784](https://github.com/w3c/csswg-drafts/issues/12784)

---

## Slide 18: January 2026 Face-to-Face
**Pillars:** CSSWG | Design | Implementation
**Part:** 3 — Resolving Loose Ends
**Label:** January 28, 2026 CSSWG Face-To-Face

### ~8 issues resolved in one breakout session

1. Rename `spanning-item` → `normal` [#13127](https://github.com/w3c/csswg-drafts/issues/13127)
2. Initial inset value → `0`. Devs said rule-break changes had no visible effect. [#13137](https://github.com/w3c/csswg-drafts/issues/13137)
3. Alignment space contributes to gap size. Whiteboard session with Alison Maher. [#12922](https://github.com/w3c/csswg-drafts/issues/12922)
4. Gap suppression with spanners. No change needed. [#13362](https://github.com/w3c/csswg-drafts/issues/13362)

---

## Slide 19: By the Numbers
**Pillars:** —
**Part:** Wrap
**Label:** By the Numbers

### Two years of work

| Metric | Count |
|--------|-------|
| Chromium CLs | 191 |
| Spec PRs merged | 22 |
| CSSWG issues filed | 30 |
| Contributors | 6 |
| Layout types · Blink layers | 3 · 3 |

Sam Davis Omekara · Javier Contreras · Kevin Babbitt · Alison Maher · Kurt Catti-Schmidt · Ian Kilpatrick

---

## Slide 20: What It Takes to Draw a Line
**Pillars:** —
**Part:** —
**Label:** Final Thoughts

### What it takes to draw a line

- Gap decorations forced us to learn paint, layout, and style.
- The spec changed because implementation kept finding real problems.
- Developer feedback during dev trial shaped the final design.
- Standards work isn't overhead. It's how you get the design right.

**Links:**
- [w3.org/TR/css-gaps-1](https://www.w3.org/TR/css-gaps-1/)

---

## Slide 21: Call to Action
**Pillars:** —
**Part:** —
**Label:** Try It

### Go build something with it

- Gap decorations are available behind a flag today. Try it. Break it. File issues.
- The feature that started as a [2018 CSSWG issue](https://github.com/w3c/csswg-drafts/issues/2748) is real now. It's better because developers pushed back and helped shape it.
- `column-rule` · `row-rule` · gap decorations API

**Links:**
- [w3.org/TR/css-gaps-1](https://www.w3.org/TR/css-gaps-1/)

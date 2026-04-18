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

On the Web, container types help developers achieve their desired layout:

- **Flex**: 1D placement — items flow in a single direction
- **Grid**: 2D placement — items placed across rows and columns
- **Multicol**: triggered not by `display`, but by specifying the number of columns

---

## Slide 3: The Problem
**Pillars:** CSSWG
**Part:** 1 — The Origin
**Label:** The Problem

### Gutters exist — but only one container could style them

- These containers can have **gutters**, but only **Multicol** provided a way to style them: the `column-rule` property.
- `column-rule` lets you set style, color, and width — similar to borders.
- There was clamor for this to be extended to Flex and Grid.
- **June 7, 2018**: CSSWG #2748 — "Styling Gaps/Gutters" is filed. The idea sat for six years with significant discussion but no spec.

**Links:** [CSSWG #2748](https://github.com/w3c/csswg-drafts/issues/2748)

---

## Slide 4: The Spec
**Pillars:** CSSWG | Design
**Part:** 1 — The Origin
**Label:** The Spec

### From draft to editor's draft

- Mats Palmgreen from Mozilla had a first stab at writing a spec, which informed the current spec.
- Written by **Kevin Babbitt**, a member of our team at Microsoft.
- 4 key things Kevin had in mind when designing this spec:
  1. [TBD]
  2. [TBD]
  3. [TBD]
  4. [TBD]

**Links:**
- [CSSWG #10393](https://github.com/w3c/csswg-drafts/issues/10393)
- [PR #11115](https://github.com/w3c/csswg-drafts/pull/11115)

---

## Slide 5: Three Layers
**Pillars:** Design | Implementation
**Part:** 1 — The Origin
**Label:** The Feature in Blink

### Style → Layout → Paint

As Kevin was speccing, we started getting used to the machine that is Blink Core. Gap decorations touch three core layers:

- **Style**: parse properties — `column-rule`, `row-rule`, shorthands, repeaters, lists
- **Layout**: compute gap geometry for grid, flex, and multicol
- **Paint**: draw decorations with correct clipping, overflow, and scrolling

Every CL touches at least one of these. Some touch all three.

**Diagram:** BLINK RENDERING PIPELINE — Style → Layout → Paint layer stack

---

## Slide 6: Transition
**Pillars:** —
**Part:** Transition

With initial spec and implementation, interesting questions were raised — which ushered us into the world of Standards.

---

## Slide 7: Working with Standards
**Pillars:** CSSWG | Design
**Part:** 2 — Iteration
**Label:** Working with Standards

### The spec evolved alongside the code

- Implementation surfaced questions → We filed issues → We worked with the CSSWG to resolve them → We updated the code.
- Our goal was to position the WG as a **partner**, not a gatekeeper.
- Key issues:
  - #11492: Auto repeater behavior (✓ Jan 31, 2025)
  - #11494: Computed value with none/hidden (✓ Jan 31, 2025)
  - #11496: Shorthand grammar (✓ Apr 3, 2025)
  - #12024: Outsets at edges (✓ Aug 20, 2025)
  - #12540: Renamed rule-paint-order → rule-overlap (✓ Aug 6, 2025)
- **30 issues filed. 24 resolved.**
- The other side: #11491 bikeshedding rule-break names took **14 months** to resolve with no change. The deliberation is frustrating — but it's what produces a spec multiple engines can implement consistently.

**Links:**
- [#11492](https://github.com/w3c/csswg-drafts/issues/11492)
- [#11494](https://github.com/w3c/csswg-drafts/issues/11494)
- [#11496](https://github.com/w3c/csswg-drafts/issues/11496)
- [#12024](https://github.com/w3c/csswg-drafts/issues/12024)
- [#12540](https://github.com/w3c/csswg-drafts/issues/12540)
- [#11491](https://github.com/w3c/csswg-drafts/issues/11491)

---

## Slide 8: The Great Rewrite — The Old Model
**Pillars:** Implementation
**Part:** 2 — Iteration
**Label:** The Great Rewrite

### The Old Model

- **Intersections as first-class objects.** Each gap had a list of intersection points.
- 2D vectors of gaps × intersections.
- Storage: O(m × n) — every intersection stored as an (x, y) pair.
- Simple for grid, but expensive at scale.

**Memory at O(m × n):**
| Grid Size | Memory |
|-----------|--------|
| 100 × 100 | 160 KB |
| 1,000 × 1,000 | 16 MB |
| 5,000 × 5,000 | 400 MB |

---

## Slide 9: The Great Rewrite — Why Grid Forced the Issue
**Pillars:** Implementation
**Part:** 2 — Iteration
**Label:** The Great Rewrite

### Why Grid forced the issue

- Grid tracks use **compressed abstractions** — `GridRanges` and `GridSets`.
- Grid avoids expanding tracks into individual positions. This is by design.
- But the old gap decoration model needed expansion via `ComputeExpandedPositions`.
- We were **fighting the grid engine's philosophy** — forcing it to do what it was specifically designed not to do.
- The mismatch was untenable. We needed a model that respected how grid (and flex, and multicol) actually work internally.

---

## Slide 10: The Great Rewrite — Anchor/Dependent Model
**Pillars:** Implementation
**Part:** 2 — Iteration
**Label:** The Great Rewrite

### The Anchor / Dependent Model

- **Two-direction model.** Anchor Gaps (primary) + Dependent Gaps (orthogonal).
- Intersections computed **on-demand**, not stored.
- Storage: O(m + n)
- CL 6819000: introduced separate Main and Cross gap classes
- ~75% memory reduction (grid), ~50% reduction (flex)

**Diagram:** Before: 12 pairs — every intersection stored as (x, y). After: 2 + 2 offsets — column offsets + row offsets, stored separately.

**Links:**
- [CL 6819000](https://chromium-review.googlesource.com/c/chromium/src/+/6819000)

---

## Slide 11: Do Not Expand (Except Animations)
**Pillars:** Implementation
**Part:** 2 — Iteration
**Label:** Property Lists

### Do not expand — except for animations

- `GapDataListIterator` traverses property lists **without expanding**.
- Three regions: **Leading**, **Auto**, **Trailing**.
- But animations (CSSWG #12431) need expansion. LCM alignment for interpolating lists of different lengths.
- Led to V2 interpolation: CL 6961880, CL 7078855

**Animation demo caption:** column-rule-color, column-rule-width: animating…

**Links:**
- [CL 6773829](https://chromium-review.googlesource.com/c/chromium/src/+/6773829)
- [CL 6961880](https://chromium-review.googlesource.com/c/chromium/src/+/6961880)
- [CL 7078855](https://chromium-review.googlesource.com/c/chromium/src/+/7078855)
- [CSSWG #12431](https://github.com/w3c/csswg-drafts/issues/12431)

---

## Slide 12: Paint Land — Fun but Subtle
**Pillars:** Implementation
**Part:** 2 — Iteration
**Label:** Into Unfamiliar Territory

### Paint Land — fun but subtle

Geometry from layout → rects → paint ops (`BoxFragmentPainter`). Subtleties we had to solve:

- **Ink overflow** — decorations extending past content box
- **Overflow & clipping** — scrollable containers
- **Paint invalidation** — knowing when to repaint
- **Resize repaint** — responsive reflow
- **Multicol paint** — fragmented columns

Shoutout to **Philip Rogers (pdr)**, a paint owner, who guided us through **120+ review comments**.

**Paint diagram:** Before: decorations clipped on scroll. After: ink overflow, scroll, resize — all correct.

**Links:**
- [CL 6101656](https://chromium-review.googlesource.com/c/chromium/src/+/6101656)
- [CL 6451833](https://chromium-review.googlesource.com/c/chromium/src/+/6451833)
- [CL 6506379](https://chromium-review.googlesource.com/c/chromium/src/+/6506379)
- [CL 6625941](https://chromium-review.googlesource.com/c/chromium/src/+/6625941)
- [CL 6618134](https://chromium-review.googlesource.com/c/chromium/src/+/6618134)
- [CL 6907139](https://chromium-review.googlesource.com/c/chromium/src/+/6907139)
- [CL 6848348](https://chromium-review.googlesource.com/c/chromium/src/+/6848348)

---

## Slide 13: Transition
**Pillars:** —
**Part:** Transition

### But who are we building for?

Dev trial launched in Chrome/Edge 139, June 2025. We put the feature behind a flag and handed it to the people who'd actually use it.

---

## Slide 14: Feedback That Changed the Spec
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
| I18N REVIEW | Internationalization concerns — logical property naming and direction-aware behavior for RTL and vertical writing modes | ✓ ISSUE #1064 |
| NAMING FEEDBACK | Property naming clarity — developer confusion around rule-break, rule-overlap, and shorthand grammar | ✓ INCORPORATED |

**Pushed to future:**

| Source | Feedback | Status |
|--------|----------|--------|
| SMASHINGCONF FREIBURG | Gradients and images as decorations — like border-image but for gaps. Dots, symbols, patterns between items. | ▶ DEFERRED |
| ALICO-CRA | Ornaments at crossing points — styling where row and column rules intersect | ▶ DEFERRED |
| PER-SIDE STYLING | Per-side independent styling — different decoration styles on each side of an item, like border shorthand granularity | ▶ DEFERRED |

**Quote:** "This made my day!" — RexGalilae, on inset changes

"Not 'no' — 'not yet.' They become the roadmap."

**Links:**
- [MSEdge #1099](https://github.com/MicrosoftEdge/MSEdgeExplainers/issues/1099)
- [MSEdge #996](https://github.com/MicrosoftEdge/MSEdgeExplainers/issues/996)
- [MSEdge #1064](https://github.com/MicrosoftEdge/MSEdgeExplainers/issues/1064)
- [MSEdge #988](https://github.com/MicrosoftEdge/MSEdgeExplainers/issues/988)
- [MSEdge #984](https://github.com/MicrosoftEdge/MSEdgeExplainers/issues/984)
- [MSEdge #985](https://github.com/MicrosoftEdge/MSEdgeExplainers/issues/985)

---

## Slide 15: From Intersections to Segments
**Pillars:** CSSWG | Design
**Part:** 3 — Resolving Loose Ends
**Label:** The Model Was Grid-specific

### From intersections to segments

- The intersection model was biased toward grid — it didn't generalize for multicol and flex.
- The spec moved to a **segments-based model**. The biggest spec rewrite since the initial draft.

**Diagram:** Old: Intersections (grid-style cross marks at every junction). New: Segments (paired segment endpoints per rule line).

**Links:**
- [#12784](https://github.com/w3c/csswg-drafts/issues/12784)
- [PR #13299](https://github.com/w3c/csswg-drafts/pull/13299)

---

## Slide 16: January 2026 Face-to-Face
**Pillars:** CSSWG | Design | Implementation
**Part:** 3 — Resolving Loose Ends
**Label:** January 28, 2026 CSSWG Face-To-Face

### ~8 issues resolved in one breakout session

1. Rename `spanning-item` → `normal` [#13127](https://github.com/w3c/csswg-drafts/issues/13127)
2. Initial inset value → `0`. Devs said rule-break changes had no visible effect. [#13137](https://github.com/w3c/csswg-drafts/issues/13137)
3. Alignment space contributes to gap size. Whiteboard session with Alison Maher. [#12922](https://github.com/w3c/csswg-drafts/issues/12922)
4. Gap suppression with spanners — no change needed. [#13362](https://github.com/w3c/csswg-drafts/issues/13362)

**11 CLs + 2 spec PRs within 5 weeks.**
When you get the right people in a room, things move fast.

---

## Slide 17: By the Numbers
**Pillars:** —
**Part:** —
**Label:** By the Numbers

### Two years of work

| Metric | Count |
|--------|-------|
| Chromium CLs | 191 |
| Spec PRs merged | 22 |
| CSSWG issues filed | 30 |
| Contributors | 5 |
| Layout types · Blink layers | 3 · 3 |

Sam Davis Omekara · Javier Contreras · Kevin Babbitt · Alison Maher · Kurt Catti-Schmidt

---

## Slide 18: The Feature That Touches Everything
**Pillars:** —
**Part:** —
**Label:** Final Thoughts

### The feature that touches everything

- Gap decorations forced us to learn paint, layout, and style in depth
- The spec changed because implementation revealed real problems
- Developer feedback during dev trial directly shaped the final design
- Standards work isn't overhead. It's how you get the design right.

**Links:**
- [w3.org/TR/css-gaps-1](https://www.w3.org/TR/css-gaps-1/)

---

## Slide 19: Call to Action
**Pillars:** —
**Part:** —
**Label:** Try It

### Go build something with it

- Gap decorations are available behind a flag today. Try it. Break it. File issues.
- The feature that started as a [2018 CSSWG issue](https://github.com/w3c/csswg-drafts/issues/2748) is now real — and it's better because developers pushed back and helped shape it.
- `column-rule` · `row-rule` · gap decorations API

**Links:**
- [w3.org/TR/css-gaps-1](https://www.w3.org/TR/css-gaps-1/)

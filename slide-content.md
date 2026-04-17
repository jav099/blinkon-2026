# Gap Decorations v2: Slide Content

## Slide 1: Title
**Pillars:** —
**Part:** —

**BlinkOn 2026**

# StoryTime: CSS Gap Decorations
From spec proposal to cross-layer implementation in Blink

- 191 CLs
- 22 Spec PRs
- 30 CSSWG Issues

Javier Contreras · Sam Davis Omekara
Microsoft Web Platform

---

## Slide 2: Containers
**Pillars:** —
**Part:** 1 — The Origin.

### The building blocks of layout

- On the Web, container types like **Flex** and **Grid** help developers achieve their desired layout.
- **Flex**: 1D placement — items flow in a single direction
- **Grid**: 2D placement — items placed across rows and columns
- **Multicol**: triggered not by `display`, but by specifying the number of columns

---

## Slide 3: The Problem
**Pillars:** CSSWG
**Part:** 1 — The Origin.

### Gutters exist — but only one container could style them

- These containers can have **gutters** (gaps between items), but only **Multicol** provided a way to style them: the `column-rule` property.
- `column-rule` lets you set style, color, and width — similar to borders.
- There was clamor for this to be extended to Flex and Grid.
- **June 7, 2018**: CSSWG #2748 — "Styling Gaps/Gutters" is filed. The idea sat for six years with significant discussion but no spec.

**Links:** [CSSWG #2748](https://github.com/w3c/csswg-drafts/issues/2748)

---

## Slide 4: The Spec
**Pillars:** CSSWG | Design
**Part:** 1 — The Origin.

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
**Part:** 1 — The Origin.
**Subtitle:** The Feature in Blink

### Style → Layout → Paint

As Kevin was speccing, we started getting used to the machine that is Blink Core. Gap decorations touch three core layers:

- **Style**: parse properties — column-rule, row-rule, shorthands, repeaters, lists
- **Layout**: compute gap geometry for grid, flex, and multicol
- **Paint**: draw decorations with correct clipping, overflow, and scrolling

Every CL touches at least one of these. Some touch all three.

---

## Slide 6: Transition
**Pillars:** —
**Part:** Transition

With initial spec and implementation, interesting questions were raised — which ushered us into the world of Standards.

---

## Slide 7: Working with Standards
**Pillars:** CSSWG | Design
**Part:** 2 — Iteration.
**Subtitle:** Working with Standards

### The spec evolved alongside the code

- Implement → discover → file issue → resolve → implement again
- The WG is a partner, not a blocker
- Key issues:
  - #11492: Auto repeater behavior
  - #11494: Computed value with none/hidden
  - #11496: Shorthand grammar
  - #12024: Outsets at edges
- **30 issues filed. 24 resolved.**
- The other side: #11491 bikeshedding took **14 months**. Some issues need multiple F2F meetings.
- The deliberation is frustrating but produces a spec multiple engines can implement consistently.

**Links:**
- [#11492](https://github.com/w3c/csswg-drafts/issues/11492)
- [#11494](https://github.com/w3c/csswg-drafts/issues/11494)
- [#11496](https://github.com/w3c/csswg-drafts/issues/11496)
- [#12024](https://github.com/w3c/csswg-drafts/issues/12024)
- [#11491](https://github.com/w3c/csswg-drafts/issues/11491)

---

## Slide 8: The Great Rewrite — The Old Model
**Pillars:** Implementation
**Part:** 2 — Iteration.
**Subtitle:** The Great Rewrite

### The old model

- CSSWG resolved to suppress gaps at fragmentation breaks → pushed us to reconsider
- Old model: intersections as first-class objects. Each gap stored a list of intersection points.
- Gap Geometry: 2D vector of column gaps + 2D vector of row gaps with intersections
- O(m × n) storage — twice over
- 100×100 grid: ~160KB. 1000×1000: ~16MB. 5000×5000: ~400MB.

---

## Slide 9: The Great Rewrite — Why Grid Forced the Issue
**Pillars:** Implementation
**Part:** 2 — Iteration.
**Subtitle:** The Great Rewrite

### Why grid forced the issue

- Grid tracks use compressed abstractions: GridRanges and GridSets
- Grid's philosophy: avoid expanding repeater tracks unnecessarily
- But gap decorations required calling `ComputeExpandedPositions` to know where tracks ended
- We were fighting the grid engine's own design philosophy
- After benchmarking (07/31 meeting): memory footprint too much
- Goal: reduce footprint without drastically affecting paint code

---

## Slide 10: The Great Rewrite — Anchor/Dependent Model
**Pillars:** Implementation
**Part:** 2 — Iteration.
**Subtitle:** The Great Rewrite

### Anchor/Dependent model

- New two-direction model: Anchor Gaps (primary axis) + Dependent Gaps (orthogonal axis)
- Intersections computed on-demand, not stored
- Grid: every anchor associated with every dependent. Flex: only associated gaps that actually intersect.
- Two 1D vectors instead of two 2D vectors. O(m + n) storage.
- ~75% memory reduction for grid, ~50% for flex
- Spanning items: store span ranges (start, end), merge/sort, binary search at paint time

---

## Slide 11: Do Not Expand (Except Animations)
**Pillars:** Implementation
**Part:** 2 — Iteration.

### Do not expand (except animations)

- Same "don't expand" philosophy applied to gap decoration properties
- Built `GapDataListIterator` (CL 6773829): traverses GapDataList without expanding
- Three logical regions: Leading, Auto, Trailing
- But animations (CSSWG #12431) needed expansion at computed value time for interpolation
- V2 interpolation: expand only when animating, keep compressed otherwise

**Links:**
- [CL 6773829](https://chromium-review.googlesource.com/c/chromium/src/+/6773829)
- [CSSWG #12431](https://github.com/w3c/csswg-drafts/issues/12431)

---

## Slide 12: Paint Land — Fun but Subtle
**Pillars:** Implementation
**Part:** 2 — Iteration.

### Paint land — fun but subtle

- Core idea: geometry from layout → construct rects → call paint ops (BoxFragmentPainter)
- Subtleties:
  - Ink overflow (CL 6451833)
  - Overflow/clipping (CL 6506379)
  - Paint invalidation (CL 6625941)
  - Resize repaint (CL 6618134)
  - Multicol paint (CL 6907139)
- Optimized model: generate intersections at paint time (CL 6848348)
- Shoutout to Philip Rogers (PDR) for paint mentorship

**Links:**
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

"But who are we building for?"

Dev trial launched in Chrome/Edge 139, June 2025. Put feature behind a flag.

---

## Slide 14: Feedback That Changed the Spec
**Pillars:** CSSWG | Design
**Part:** 3 — Resolving Loose Ends.

### Feedback that changed the spec

- 12 issues from 11 developers

**Incorporated:**
- Ahmad Shadeed: empty grid areas → new `rule-visibility-items` property
- RexGalilae: asymmetric insets → restructured `rule-inset` ("This made my day!")
- i18n concerns and naming feedback

**Pushed to future:**
- Images/gradients as decorations
- Ornaments at crossing points
- Per-side styling
- Advanced pattern repeaters

"Not 'no' — 'not yet.' They become the roadmap."

**Links:**
- [MSEdge #1099](https://github.com/MicrosoftEdge/MSEdgeExplainers/issues/1099)
- [MSEdge #996](https://github.com/MicrosoftEdge/MSEdgeExplainers/issues/996)
- [MSEdge #1155](https://github.com/MicrosoftEdge/MSEdgeExplainers/issues/1155)
- [MSEdge #984](https://github.com/MicrosoftEdge/MSEdgeExplainers/issues/984)
- [MSEdge #985](https://github.com/MicrosoftEdge/MSEdgeExplainers/issues/985)

---

## Slide 15: From Intersections to Segments
**Pillars:** CSSWG | Design
**Part:** 3 — Resolving Loose Ends.

### From intersections to segments

- Intersection model was biased toward grid — didn't generalize for multicol/flex
- Spec moved from intersection-based to segments-based model
- Biggest spec rewrite since initial draft — made model work across all three container types

**Links:**
- [#12784](https://github.com/w3c/csswg-drafts/issues/12784)
- [PR #13299](https://github.com/w3c/csswg-drafts/pull/13299)

---

## Slide 16: January 2026 Face-to-Face
**Pillars:** CSSWG | Design | Implementation
**Part:** 3 — Resolving Loose Ends.

### ~8 issues resolved in one breakout session

- `spanning-item` → `normal`, #13127
- Initial inset → `0`, #13137
- Alignment space + gap size — whiteboard session with Alison Maher, #12922
- Gap suppression with spanners, #13362
- **11 CLs + 2 spec PRs within 5 weeks**
- "When you get the right people in a room, things move fast."

**Links:**
- [#13127](https://github.com/w3c/csswg-drafts/issues/13127)
- [#13137](https://github.com/w3c/csswg-drafts/issues/13137)
- [#12922](https://github.com/w3c/csswg-drafts/issues/12922)
- [#13362](https://github.com/w3c/csswg-drafts/issues/13362)

---

## Slide 17: By the Numbers
**Pillars:** —
**Part:** —

### Two years of work

| Metric | Count |
|---|---|
| Chromium CLs | 191 |
| Spec PRs | 22 |
| CSSWG Issues | 30 |
| Contributors | 5 |
| Layout Types | 3 |
| Blink Layers | 3 |

Sam Davis Omekara · Javier Contreras · Kevin Babbitt · Alison Maher · Kurt Catti-Schmidt

---

## Slide 18: The Feature That Touches Everything
**Pillars:** —
**Part:** —

### The feature that touches everything

- Gap decorations forced us to learn paint, layout, and style in depth
- The spec changed because implementation revealed real problems
- Developer feedback during dev trial directly shaped the final design
- Standards work isn't overhead. It's how you get the design right.

---

## Slide 19: Call to Action
**Pillars:** —
**Part:** —

### Try it today

- Gap decorations are available behind a flag today. Try it. Break it. File issues.
- The feature that started as a 2018 CSSWG issue is now real.
- `column-rule`, `row-rule`, and the full gap decorations API — go build something with it.

**Links:**
- [w3.org/TR/css-gaps-1](https://www.w3.org/TR/css-gaps-1/)

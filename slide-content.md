# Gap Decorations v2: Slide Content

## Slide 1: Title
**Pillars:** —
**Part:** —

**BlinkOn 2026**

# The Story of CSS Gap Decorations
From spec proposal to cross-layer implementation in Blink

- 191 CLs
- 22 Spec PRs
- 30 CSSWG Issues

Javier Contreras · Sam Davis Omekara

---

## Slide 2: The First Thread
**Pillars:** CSSWG
**Part:** 1 — The Origin.

### Six years before a single line of code

- **June 7, 2018**: The initial CSSWG #2748: "Styling Gaps/Gutters" issue is opened.
- The idea sat in the issue tracker for six years
- No spec, no implementation, no resolution, just brainstorming and interest from the community.

> "Proposed adding styling capabilities to CSS gaps/gutters."
> — fantasai, CSSWG #2748

**Links:** [CSSWG #2748](https://github.com/w3c/csswg-drafts/issues/2748)

---

## Slide 3: The Proposal
**Pillars:** CSSWG | Design
**Part:** 1 — The Origin.

### Microsoft picks up the thread

- **June 5, 2024**: CSSWG #10393, formal proposal for CSS Gap Decorations Level 1
- **June 12, 2024**: CSSWG resolves to adopt the spec and assigns one of our teammates as editor.
- **Dec 13, 2024**: PR #11115, first editor's draft published.
- **Nov 6, 2024**: Sam Davis lands the first CL. Implementation begins.

**Links:**
- [CSSWG #10393](https://github.com/w3c/csswg-drafts/issues/10393)
- [PR #11115](https://github.com/w3c/csswg-drafts/pull/11115)
- [CL 5990892](https://chromium-review.googlesource.com/c/chromium/src/+/5990892)

---

## Slide 4: Three Layers
**Pillars:** Design | Implementation
**Part:** 1 — The Origin.
**Subtitle:** The Feature in Blink

### Style → Layout → Paint

Gap decorations span three layers of Blink's rendering pipeline:

- **Style**: parse properties: column-rule, row-rule, shorthands, repeaters, lists
- **Layout**: compute gap geometry for grid, flex, and multicol
- **Paint**: draw decorations with correct clipping, overflow, and scrolling

---

## Slide 5: Transition
**Pillars:** —
**Part:** Transition

The initial implementation led to open questions, on both the design and implementation sides.

---

## Slide 6: Working with Standards
**Pillars:** CSSWG | Design
**Part:** 2 — Iteration.
**Subtitle:** Working with Standards

### The spec evolved alongside the code

Implementation surfaced questions -> We filed issues -> We worked with the CSSWG to resolve them -> We updated the code.

- #11492: Auto repeater behavior (resolved Jan 31, 2025)
- #11494: Computed value with none/hidden (resolved Jan 31, 2025)
- #11496: Shorthand grammar (resolved Apr 3, 2025)
- #12024: Outsets at edges (resolved Aug 20, 2025)
- #12540: Renamed rule-paint-order → rule-overlap (resolved Aug 6, 2025)

**30 issues filed. 24 resolved.** This feedback loop between implementation and spec is the engine that drove progress. We couldn't have built the CSSWG without it.

**Links:**
- [#11492](https://github.com/w3c/csswg-drafts/issues/11492)
- [#11494](https://github.com/w3c/csswg-drafts/issues/11494)
- [#11496](https://github.com/w3c/csswg-drafts/issues/11496)
- [#12024](https://github.com/w3c/csswg-drafts/issues/12024)
- [#12540](https://github.com/w3c/csswg-drafts/issues/12540)

---

## Slide 7: Standards Work Is Slow by Design
**Pillars:** CSSWG
**Part:** 2 — Iteration.
**Subtitle:** The other side of the coin

### Standards work is slow by design

The other side of the standards coin:

- #11491: bikeshedding rule-break names took **14 months** to resolve with no change.
- Some issues need multiple face-to-face meetings to resolve.
- Implementation can be blocked on a naming discussion.

The deliberation is frustrating, but it's also what produces a spec that **multiple engines can implement consistently** and as **future-proof** as possible.

Visual: timeline bars showing deliberation length for 3 issues (#11491: 14 months, #12848: 2 months, #12431: 1 month)

**Links:**
- [#11491](https://github.com/w3c/csswg-drafts/issues/11491)
- [#12848](https://github.com/w3c/csswg-drafts/issues/12848)
- [#12431](https://github.com/w3c/csswg-drafts/issues/12431)

---

## Slide 8: The MainGap / CrossGap Split
**Pillars:** Implementation
**Part:** 2 — Iteration.
**Subtitle:** Architecture

### The MainGap / CrossGap split

- [CL 6819000](https://chromium-review.googlesource.com/c/chromium/src/+/6819000) (Aug 2025): introduced separate Main and Cross gap classes.
- The original model stored every intersection as an (x, y) pair.
- Optimization::
  - Better performance: O(n × m) → O(n + m)
  - Grid track expansion and fragmentation support
  - Cleaner multicol integration

Visual: Before/after 3×3 grid diagrams.
- Before: 12 (x,y) intersection pairs marked with crosses at every junction.
- After: 2 column offsets (blue triangles) + 2 row offsets (green triangles), stored separately.

**Links:**
- [CL 6819000](https://chromium-review.googlesource.com/c/chromium/src/+/6819000)
- [CL 6854089](https://chromium-review.googlesource.com/c/chromium/src/+/6854089)
- [CL 6850590](https://chromium-review.googlesource.com/c/chromium/src/+/6850590)
- [CL 6885245](https://chromium-review.googlesource.com/c/chromium/src/+/6885245)

---

## Slide 9: Learning Paint from Scratch
**Pillars:** Implementation
**Part:** 2 — Iteration.
**Subtitle:** Into Unfamiliar Territory

### Learning paint from scratch

Nobody on the team had worked in Blink's paint code before. We needed decorations to clip, scroll, resize, and fragment correctly.

Philip Rogers (pdr), a paint owner, guided us through 4 CLs and **120+ review comments**. Every patchset was a lesson in how the rendering pipeline actually works.

Visual: before/after comparison, broken clipped grid vs. working gap decorations

**Links:**
- [CL 6451833](https://chromium-review.googlesource.com/c/chromium/src/+/6451833)
- [CL 6506379](https://chromium-review.googlesource.com/c/chromium/src/+/6506379)
- [CL 6618134](https://chromium-review.googlesource.com/c/chromium/src/+/6618134)
- [CL 6907139](https://chromium-review.googlesource.com/c/chromium/src/+/6907139)

---

## Slide 10: Lists, Repeaters, and LCM Interpolation
**Pillars:** CSSWG | Implementation
**Part:** 2 — Iteration.
**Subtitle:** More Unfamiliar Territory

### Lists, repeaters, and LCM interpolation

- Properties accept lists with auto repeaters (e,g. `column-rule-width: repeat(auto, 1opx, 20px)`)
- We initially assumed we would never need to expand repeaters
- #12431 resolved: animate using LCM alignment. Turns out we needed to expand for animating.
- Led to V2 interpolation: CL 6961880, CL 7078855

Visual: animated 3×3 grid with cycling column-rule colors and pulsing row-rule widths

**Links:**
- [CSSWG #12431](https://github.com/w3c/csswg-drafts/issues/12431)
- [CL 6961880](https://chromium-review.googlesource.com/c/chromium/src/+/6961880)
- [CL 7078855](https://chromium-review.googlesource.com/c/chromium/src/+/7078855)

---

## Slide 11: Transition
**Pillars:** —
**Part:** Transition

Dev trial launched in Chrome/Edge 139 (June 2025). Developer feedback reshaped the spec.

---

## Slide 12: Developer Feedback
**Pillars:** CSSWG | Design
**Part:** 3 — Resolving Loose Ends.

### Feedback that changed the spec

12 issues from 11 developers. 5 drove spec changes. 4 became future-level ideas.

**Addressed:**
1. Ahmad Shadeed: Decorations around empty grid areas. Result: new `rule-visibility-items` property.
2. RexGalilae: Asymmetric insets on each side. Result: restructured `rule-inset` properties. ("This made my day!")
3. o-t-w: Decorations with zero gap (border-collapse behavior). Result: 0px gap support in flex + grid.

**Deferred to future spec level:**
4. SmashingConf Freiburg: Gradients and images as decorations (like border-image for gaps).
5. alico-cra: Ornaments at crossing points (styling where row and column rules intersect).

**Links:**
- [MSEdge #1099](https://github.com/MicrosoftEdge/MSEdgeExplainers/issues/1099)
- [MSEdge #996](https://github.com/MicrosoftEdge/MSEdgeExplainers/issues/996)
- [MSEdge #1155](https://github.com/MicrosoftEdge/MSEdgeExplainers/issues/1155)
- [MSEdge #984](https://github.com/MicrosoftEdge/MSEdgeExplainers/issues/984)
- [MSEdge #985](https://github.com/MicrosoftEdge/MSEdgeExplainers/issues/985)

---

## Slide 13: From Intersections to Segments
**Pillars:** CSSWG | Design
**Part:** 3 — Resolving Loose Ends.
**Subtitle:** The Model Was Grid-specific.

### From intersections to segments

**Sep 11, 2025**: #12784, multicol intersection points don't generalize. We realized the model was biased towards grid.
Offline TPAC 2025 discussion led to a **major** spec overhaul.

Visual: Two side-by-side diagrams of a grid (cells 1-6, item 3 spanning 2 cols):
- **Old: Intersections**: column rules broken at each row gap junction, crosses at every intersection point (8 crosses)
- **New: Segments**: column rules are continuous segments spanning full height, crosses only at segment endpoints (6 crosses)

**Nov 2025**: We post the new proposal. **Feb 2026**: Officialy incorporated into spec. The biggest spec change since the initial draft.

**Links:**
- [#12784](https://github.com/w3c/csswg-drafts/issues/12784)
- [PR #13299](https://github.com/w3c/csswg-drafts/pull/13299)

---

## Slide 14: January 28, 2026 CSSWG Face-To-Face
**Pillars:** CSSWG | Design | Implementation
**Part:** 3 — Resolving Loose Ends.

### Four issues resolved in one session

1. Rename `spanning-item` → `normal`, #13127
2. Initial inset value → `0`, #13137. Devs said rule-break changes had no visible effect.
3. Alignment space contributes to gap size, #12922. Whiteboard session with Alison Maher.
4. Gap suppression with spanners, no change needed, #13362

**11 CLs + 2 spec PRs within 5 weeks.**
Participants: Microsoft, other browser vendors, web developers.

**Links:**
- [#13127](https://github.com/w3c/csswg-drafts/issues/13127)
- [#13137](https://github.com/w3c/csswg-drafts/issues/13137)
- [#12922](https://github.com/w3c/csswg-drafts/issues/12922)
- [#13362](https://github.com/w3c/csswg-drafts/issues/13362)

---

## Slide 15: By the Numbers
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

## Slide 16: Final Thoughts
**Pillars:** —
**Part:** —

### The feature that touches everything

- Gap decorations forced us to learn paint, layout, and style in depth
- The spec changed because implementation revealed real problems
- Developer feedback during dev trial directly shaped the final design
- Standards work isn't overhead. It's how you get the design right.

[w3.org/TR/css-gaps-1](https://www.w3.org/TR/css-gaps-1/)

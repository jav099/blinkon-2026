# Gap Decorations — Developer Feedback Tracker

Tracking external developer feedback received through [MSEdgeExplainers](https://github.com/MicrosoftEdge/MSEdgeExplainers/issues?q=label%3ACSSGapDecorations) and how it was addressed through CSSWG spec issues and Chromium CLs.

---

## Feedback That Drove Spec/Implementation Changes

These are cases where external developer feedback directly influenced the CSS Gap Decorations specification or implementation.

### 1. Gap decorations around empty grid areas

| | |
|---|---|
| **Feedback** | [MSEdge #1099](https://github.com/MicrosoftEdge/MSEdgeExplainers/issues/1099) — Ahmad Shadeed ([@shadeed](https://github.com/shadeed)) |
| **Date** | 2025-07-20 |
| **Request** | Decorations appear even for empty grid areas. In responsive layouts, cells are filled at small viewports but empty at large ones — decorations should adapt. |
| **Related feedback** | [MSEdge #1100](https://github.com/MicrosoftEdge/MSEdgeExplainers/issues/1100) (o-t-w), [MSEdge #1111](https://github.com/MicrosoftEdge/MSEdgeExplainers/issues/1111) (bigandy) |
| **Status** | **Addressed** |

**How it was addressed:**

- **CSSWG Issue** [#12602](https://github.com/w3c/csswg-drafts/issues/12602) — "Gap decorations next to empty grid areas" (created 2025-08-13)
- **RESOLVED** (2025-11-13): Add `rule-visibility-items: all | around | between` property with longhands
- **Spec PRs:**
  - [PR #13374](https://github.com/w3c/csswg-drafts/pull/13374) — Add section for rule-visibility-items (merged 2026-02-03)
  - [PR #13446](https://github.com/w3c/csswg-drafts/pull/13446) — Add more WPTs for empty cells (merged 2026-02-04)
  - [PR #13755](https://github.com/w3c/csswg-drafts/pull/13755) — Add 'normal' value to rule-visibility-items (merged 2026-04-02)
- **Chromium CLs:**
  - [CL 7080904](https://chromium-review.googlesource.com/c/chromium/src/+/7080904) — Parse column-rule-visibility-items (2025-10-24)
  - [CL 7081111](https://chromium-review.googlesource.com/c/chromium/src/+/7081111) — Parse row-rule-visibility-items (2025-10-27)
  - [CL 7082317](https://chromium-review.googlesource.com/c/chromium/src/+/7082317) — Implement layout for empty cells (2025-10-27)
  - [CL 7133218](https://chromium-review.googlesource.com/c/chromium/src/+/7133218) — Implement paint logic for empty cells (2025-11-12)
  - [CL 7524550](https://chromium-review.googlesource.com/c/chromium/src/+/7524550) — Implement rule-visibility-items shorthand (2026-01-28)
  - [CL 7540394](https://chromium-review.googlesource.com/c/chromium/src/+/7540394) — Add tests for subgrid and empty cells (2026-02-03)
  - [CL 7719758](https://chromium-review.googlesource.com/c/chromium/src/+/7719758) — Rename initial value from auto to normal (2026-04-06)

**Developer confirmation:** o-t-w tested in Canary and confirmed it "works well and is a great addition to the spec." bigandy proactively updated their CodePen to use the new property.

---

### 2. Asymmetric outsets / restructured inset properties

| | |
|---|---|
| **Feedback** | [MSEdge #996](https://github.com/MicrosoftEdge/MSEdgeExplainers/issues/996) — RexGalilae ([@RexGalilae](https://github.com/RexGalilae)) |
| **Date** | 2025-03-25 |
| **Request** | Proposed restructuring rule syntax and extending outset/offset properties to support asymmetric values (different insets on each side of a decoration). |
| **Status** | **Addressed** |

**How it was addressed:**

- **CSSWG Issues:**
  - [#12603](https://github.com/w3c/csswg-drafts/issues/12603) — "Asymmetric start and end offsets" (created 2025-08-13)
  - [#12848](https://github.com/w3c/csswg-drafts/issues/12848) — "Insets instead of outsets" (created 2025-09-23)
  - [#12024](https://github.com/w3c/csswg-drafts/issues/12024) — "Gap decoration outsets at the edges" (created 2025-03-28)
- **RESOLVED** (2025-08-20): Two longhands for inner/outer outsets; percentages resolve the same; shorthand lists inside first
- **RESOLVED** (2025-08-20): `rule-outset: <inner-start> <inner-end>? [/ <outer-start> <outer-end>?]?`
- **Spec PRs:**
  - [PR #13024](https://github.com/w3c/csswg-drafts/pull/13024) — Update outset properties (merged 2025-12-17)
  - [PR #13264](https://github.com/w3c/csswg-drafts/pull/13264) — Fix missing syntax for #inset (merged 2026-01-05)
- **Chromium CLs:**
  - [CL 7091234](https://chromium-review.googlesource.com/c/chromium/src/+/7091234) — Parse *-rule-edge/interior-start/end-outset (2025-10-31)
  - [CL 7107919](https://chromium-review.googlesource.com/c/chromium/src/+/7107919) — Make column-rule-outset into shorthand (2025-11-04)
  - [CL 7107409](https://chromium-review.googlesource.com/c/chromium/src/+/7107409) — Make row-rule-outset into shorthand (2025-11-04)
  - [CL 7127743](https://chromium-review.googlesource.com/c/chromium/src/+/7127743) — Rename outset to inset (2025-11-10)
  - [CL 7131883](https://chromium-review.googlesource.com/c/chromium/src/+/7131883) — Rule inset properties implementation (2025-11-11)
  - [CL 7591773](https://chromium-review.googlesource.com/c/chromium/src/+/7591773) — Implement *-rule-inset-start/end shorthands (2026-02-20)

**Developer confirmation:** RexGalilae responded: "This made my day!" and confirmed eagerness to test.

---

### 3. Logical property naming (row/column vs block/inline)

| | |
|---|---|
| **Feedback** | [MSEdge #1064](https://github.com/MicrosoftEdge/MSEdgeExplainers/issues/1064) — xaddict ([@xaddict](https://github.com/xaddict)) |
| **Date** | 2025-06-18 |
| **Request** | Use `block-rule`/`inline-rule` instead of `row-rule`/`column-rule` for better i18n and writing-mode support. |
| **Status** | **Addressed (clarified — no change needed)** |

**Response:** Kevin Babbitt explained that "row" and "column" are already logical terms per the Flexbox and Grid specifications — they follow writing mode, not physical direction. This is different from `top`/`left`/`right`/`bottom` which are physical. The naming deliberately reuses existing `column-rule` from Multi-column Layout for consistency.

**Related CSSWG work:**
- [CSSWG #13648](https://github.com/w3c/csswg-drafts/issues/13648) — More vertical-writing cases (i18n review, 2026-03-13)
- [CSSWG #13649](https://github.com/w3c/csswg-drafts/issues/13649) — Logical geometry (i18n review, 2026-03-13)
- [PR #13732](https://github.com/w3c/csswg-drafts/pull/13732) — Clarify application of writing-mode and direction (merged 2026-04-13)
- **RESOLVED** (2026-04-13): Accept edits defining writing-mode and direction interactions

---

### 4. Property naming: "gap-decorator" instead of "row-rule"

| | |
|---|---|
| **Feedback** | [MSEdge #988](https://github.com/MicrosoftEdge/MSEdgeExplainers/issues/988) — rhires ([@rhires](https://github.com/rhires)) |
| **Date** | 2025-03-20 |
| **Request** | Rename from `row-rule`/`column-rule` to `gap-decorator` or `gap-dec` for clarity — the gap is becoming a container. |
| **Status** | **Addressed (clarified — no change needed)** |

**Response:** The naming deliberately reuses the existing `column-rule-*` properties from CSS Multi-column Layout. Using "gap" would create confusion since `gap` properties control spacing, not decoration. CSS convention drops terms for shorthands (e.g., `column-rule` → `rule`).

---

### 5. Decorations without gaps (zero-gap decorations)

| | |
|---|---|
| **Feedback** | [MSEdge #1155](https://github.com/MicrosoftEdge/MSEdgeExplainers/issues/1155) — o-t-w ([@o-t-w](https://github.com/o-t-w)) |
| **Date** | 2025-09-11 |
| **Request** | Wanted border-collapse-like behavior — decorations visible but with zero gap, like table borders. |
| **Status** | **Addressed (already supported)** |

**Response:** Patrick Brosset demonstrated that the existing proposal already supports this use case. The reporter's issue was caused by `overflow: hidden` on the grid container, not a gap decorations limitation.

**Related Chromium CLs** (zero-gap support):
- [CL 7542894](https://chromium-review.googlesource.com/c/chromium/src/+/7542894) — Gap decorations should be applied in 0px flex gaps (2026-02-04)
- [CL 7541037](https://chromium-review.googlesource.com/c/chromium/src/+/7541037) — Gap decorations should be applied in 0px grid gaps (2026-02-05)

---

## Feedback Tracked as Future Ideas

These are cases where feedback was acknowledged but deferred to future spec levels.

### 6. Images and gradients as gap decorations

| | |
|---|---|
| **Feedback** | [MSEdge #984](https://github.com/MicrosoftEdge/MSEdgeExplainers/issues/984) (Th3S4mur41), [MSEdge #1161](https://github.com/MicrosoftEdge/MSEdgeExplainers/issues/1161) (captainbrosset) |
| **Date** | 2025-03-20, 2025-09-17 |
| **Request** | Support gradients and images (like `border-image`) as gap decorations. #1161 specifically requested drawing dots/symbols (feedback from SmashingConf Freiburg). |
| **Status** | **Deferred to future spec level** |

**Response:** More complex than `border-image` because gap decorations must handle T-intersections and cross-intersections (not just corners). Shipping solid colors as MVP first. A workaround using `rule-style` + insets to approximate dots was demonstrated. Added to explainer as a Future Idea.

---

### 7. Pseudo-element approach (::gap-rule)

| | |
|---|---|
| **Feedback** | [MSEdge #987](https://github.com/MicrosoftEdge/MSEdgeExplainers/issues/987) — spoike ([@spoike](https://github.com/spoike)) |
| **Date** | 2025-03-20 |
| **Request** | Use pseudo-elements like `::gap-main-axis` instead of properties, allowing `content` property for breadcrumb trails and other rich decorations. |
| **Status** | **Deferred — properties approach chosen** |

**Response:** Pseudo-elements add disproportionate computation and storage complexity. The property-based approach is more performant. For breadcrumbs, real flex items are better for accessibility (screen readers, focus, tooltips). Images in gap decorations planned as a future extension. Explainer updated with the tradeoff analysis.

---

### 8. Intersection decorations (ornaments at crossing points)

| | |
|---|---|
| **Feedback** | [MSEdge #985](https://github.com/MicrosoftEdge/MSEdgeExplainers/issues/985) — alico-cra ([@alico-cra](https://github.com/alico-cra)) |
| **Date** | 2025-03-20 |
| **Request** | Control over spacing and styling at intersections where row and column rules cross — padding on line ends, border-radius at intersection points. |
| **Status** | **Deferred to future spec level** |

**Response:** Acknowledged as timely given CSSWG discussions about border corners. Added to explainer as a Future Idea for intersection decorations.

---

### 9. More control over where rules appear (per-cell)

| | |
|---|---|
| **Feedback** | [MSEdge #1100](https://github.com/MicrosoftEdge/MSEdgeExplainers/issues/1100) — o-t-w ([@o-t-w](https://github.com/o-t-w)) |
| **Date** | 2025-07-21 |
| **Request** | Ability to selectively show/hide rules for specific columns, rows, or cells — not just globally. Example: wanting column-rules everywhere except the first row. |
| **Status** | **Partially addressed via `rule-visibility-items`; full per-cell control deferred** |

**Response:** The `rule-visibility-items` property addresses the empty-cell case. Full per-cell control (Gap Decoration Areas) is deferred to a future spec level. The reporter tested and confirmed the current solution "works well."

---

## Summary

| Category | Count | Details |
|----------|-------|---------|
| **Feedback addressed in spec** | 5 | Empty areas (#1099, #1100, #1111), asymmetric insets (#996), zero-gap (#1155), i18n (#1064), naming (#988) |
| **Deferred to future level** | 4 | Images (#984, #1161), pseudo-elements (#987), intersections (#985), per-cell control (#1100 partial) |
| **Total unique feedback issues** | 12 | From 11 different external developers |
| **CLs directly influenced** | 15+ | Primarily rule-visibility-items and inset property CLs |
| **CSSWG issues opened** | 6+ | #12602, #12603, #12848, #12024, #13648, #13649 |
| **Spec PRs merged** | 5+ | #13374, #13446, #13755, #13024, #13264, #13732 |

### Notable External Contributors

| Developer | Feedback | Impact |
|-----------|----------|--------|
| **Ahmad Shadeed** ([@shadeed](https://github.com/shadeed)) | Empty grid areas (#1099) | Drove `rule-visibility-items` property creation |
| **RexGalilae** ([@RexGalilae](https://github.com/RexGalilae)) | Asymmetric insets (#996) | Drove inset property restructuring |
| **Patrick Brosset** ([@captainbrosset](https://github.com/captainbrosset)) | Dots/symbols (#1161), helped triage multiple issues | SmashingConf feedback pipeline |
| **o-t-w** ([@o-t-w](https://github.com/o-t-w)) | Per-cell control (#1100), zero-gap (#1155) | Validated `rule-visibility-items` in Canary |
| **bigandy** ([@bigandy](https://github.com/bigandy)) | Subgrid gaps (#1111) | Real-world subgrid use case |
| **yisibl** ([@yisibl](https://github.com/yisibl)) | Testing availability (#986) | First external confirmation of Canary availability |
| **xfq** ([@xfq](https://github.com/xfq)) | i18n review (#13648, #13649) | Drove writing-mode/direction clarifications |

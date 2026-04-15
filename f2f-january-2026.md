# Gap Decorations — January 2026 CSSWG Face-to-Face Resolutions

The CSS Working Group held a face-to-face meeting on **January 28, 2026**, where four CSS Gap Decorations issues were discussed and resolved. This was a significant day for the spec — multiple foundational decisions about default behaviors, naming, and layout interactions were made in person.

**Participants in the discussions:** Javier Contreras (javierct), Sam Davis Omekara (oSamDavis), Kevin Babbitt (kbabbitt), Alison Maher (alisonmaher), Tab Atkins (TabAtkins), fantasai, astearns, dholbert, bramus, florian, hoch

**Whiteboard artifact:** [Flex gap decoration drawing](https://lists.w3.org/Archives/Public/www-archive/2026Jan/att-0002/flex-gap-decoration-drawing.jpg) (photo from the F2F session, posted by Alison Maher)

---

## Resolution 1: Rename `spanning-item` to `normal`

| | |
|---|---|
| **Issue** | [#13127](https://github.com/w3c/csswg-drafts/issues/13127) — Introduction of `auto` for `rule-break` properties |
| **Filed by** | Javier Contreras (2025-11-20) |
| **Resolved** | 2026-01-28 |
| **Closed** | 2026-02-13 |

### Background

The `rule-break` property controls how gap decorations interact with spanning items. The default value was `spanning-item`, which made sense for grid (where spanning items exist) but was meaningless for flex and multicol (where there are no spanning items). Javier proposed introducing an `auto` keyword that would resolve differently per container type.

### Discussion (from IRC log)

- **Javier:** Proposed `auto` because different containers have different sensible defaults — `spanning-item` in grid, `none` in flex/multicol.
- **fantasai:** Pointed out that since spanning items don't exist in flex/multicol, the values are already identical. Suggested renaming instead of adding a new keyword.
- **astearns:** Agreed that `spanning-item` is a confusing name because it doesn't mean anything outside of grid.
- **fantasai:** Proposed calling it `normal` — the standard CSS "do the sensible default" keyword.
- **dholbert:** Raised the question about multicol spanners — should `rule-break: none` allow decorations to draw through column spanners?
- **fantasai & Tab:** Agreed this is reasonable since multicol spanners are a different case from grid spanning items.

### Resolutions

1. **`RESOLVED: Rename 'spanning-item' to 'normal'`**
2. **`RESOLVED: Allow rule-break: none to draw through spanners in multicol`**

### Implementation

| What | Reference | Date |
|---|---|---|
| CL: Rename value in Chromium | [7533197](https://chromium-review.googlesource.com/c/chromium/src/+/7533197) | 2026-01-30 |
| CL: Remove auto, make normal default | [7533202](https://chromium-review.googlesource.com/c/chromium/src/+/7533202) | 2026-01-30 |
| CL: Re-allow decorations behind multicol spanners | [7537987](https://chromium-review.googlesource.com/c/chromium/src/+/7537987) | 2026-02-03 |
| CL: Re-allow row gap decorations to break in multicol | [7537839](https://chromium-review.googlesource.com/c/chromium/src/+/7537839) | 2026-02-04 |
| Spec PR: Change rule-break value | [PR #13475](https://github.com/w3c/csswg-drafts/pull/13475) | 2026-02-13 |

---

## Resolution 2: Change initial value of insets to 0

| | |
|---|---|
| **Issue** | [#13137](https://github.com/w3c/csswg-drafts/issues/13137) — Change the initial value for insets property to be 0 |
| **Filed by** | Sam Davis Omekara (2025-11-21) |
| **Resolved** | 2026-01-28 |
| **Status** | Still open (follow-up on `overlap-join` keyword resolved 2026-03-18) |

### Background

The initial value of interior insets was `-50%`, which caused T-junction segments to bleed halfway into crossing gaps — creating a "joined" appearance by default. This meant that changing `rule-break` values had no visible effect (because the decorations were already overlapping), which confused developers during testing.

### Discussion (from IRC log)

- **Javier:** Explained the problem — with `-50%` default, `rule-break` changes don't produce visual differences. Got feedback that this was confusing.
- **astearns:** Expressed concern that changing to `0` might mean authors need to set both `rule-break` *and* inset to get their desired look.
- **Tab:** Supported the change, noting it makes all rule-break values visually distinct.
- **Javier:** Showed examples demonstrating that `0` produces more intuitive defaults.
- **fantasai:** Agreed, suggesting a keyword for the old `-50%` behavior for authors who want the joined look.

### Resolution

**`RESOLVED: Change initial value to 0, with a keyword for the current default`**

### Follow-up (March 2026 F2F)

The keyword for the old behavior was discussed further at the March F2F:
- **`RESOLVED: Add an 'overlap-join' keyword to rule-inset`** (2026-03-18, [#13137](https://github.com/w3c/csswg-drafts/issues/13137))

### Implementation

| What | Reference | Date |
|---|---|---|
| CL: Update default value to 0 | [7533997](https://chromium-review.googlesource.com/c/chromium/src/+/7533997) | 2026-02-03 |
| CL: Introduce rule-visibility-items: auto | [7563618](https://chromium-review.googlesource.com/c/chromium/src/+/7563618) | 2026-02-11 |
| Spec PR: Make initial value '0' | [PR #13413](https://github.com/w3c/csswg-drafts/pull/13413) | 2026-02-03 |

---

## Resolution 3: Alignment space contributes to gap size

| | |
|---|---|
| **Issue** | [#12922](https://github.com/w3c/csswg-drafts/issues/12922) — Clarify if space from content distribution properties constitutes a "gutter" for decoration purposes |
| **Filed by** | Sam Davis Omekara (2025-10-09) |
| **Resolved** | 2026-01-28 |
| **Closed** | 2026-03-09 |

### Background

CSS alignment properties like `justify-content: space-between` can add extra space between items/tracks. The question was whether this alignment-derived space should count as part of the "gap" for gap decoration purposes, and how decorations should interact with flex containers where gutters can have varying widths.

### Discussion (from IRC log)

- **Sam:** Explained the tension — percentage insets are hard to resolve when alignment creates varying gutter sizes. Proposed affirming that alignment space grows gutters, and removing percentages in favor of a keyword.
- **fantasai:** Questioned the problem statement for flex containers where cross-axis intersections don't clearly apply.
- **Alison Maher:** Drew on the whiteboard to illustrate flex cross-gap intersection behavior — concluded that intersections should start when there's a gap on one or both sides.
- **fantasai:** After whiteboarding, agreed the only sensible definition is: intersection starts when one side has a gap, ends when neither side has a gap.
- **Tab:** Confirmed overlapping gaps get joined.

### Resolutions

1. **`RESOLVED: Alignment space inserted into gutters, i.e. between items/tracks/lines, contributes to the gap size`**
2. **`RESOLVED: Rules drawn between flex lines have an intersection that starts when there's a gap on one or both sides and end when there's no gap on either side`**

### Implementation

| What | Reference | Date |
|---|---|---|
| CL: Allow space from content-distribution in flex (cross) | [7585696](https://chromium-review.googlesource.com/c/chromium/src/+/7585696) | 2026-02-20 |
| CL: Allow space from content-distribution in flex (main) | [7589305](https://chromium-review.googlesource.com/c/chromium/src/+/7589305) | 2026-03-05 |
| CL: Suppress full effective gaps for flex | [7615749](https://chromium-review.googlesource.com/c/chromium/src/+/7615749) | 2026-03-06 |

---

## Resolution 4: Gap suppression with spanning items — no change needed

| | |
|---|---|
| **Issue** | [#13362](https://github.com/w3c/csswg-drafts/issues/13362) — Suppression of gaps/gutters across fragment breaks with spanning item |
| **Filed by** | Sam Davis Omekara (2026-01-16) |
| **Resolved** | 2026-01-28 |
| **Closed** | 2026-02-12 |

### Background

A previous resolution ([#11520](https://github.com/w3c/csswg-drafts/issues/11520), Jan 2025) established that gaps overlapping fragment breaks should be suppressed. Sam raised a potential caveat: in grid layouts, a spanning item crossing a gap at a fragment break might get partially clipped if the gap is suppressed.

### Discussion (from IRC log)

- **Sam:** Proposed an exception — don't suppress a gap when a spanning item crosses it at a fragment break.
- **fantasai:** Clarified that suppressing a gap doesn't mean clipping adjacent content. It means treating the gap as 0-height — content is not cut, only gap space is removed.
- **fantasai:** This is the same behavior as margin suppression during fragmentation — margins become 0 but spanning items are not clipped.
- **bramus:** Captured the consensus.

### Resolution

**`RESOLVED: Close, no change`**

The existing behavior was already correct — gap suppression at fragment breaks doesn't clip spanning items, it just treats the gap height as 0. No spec or implementation changes needed.

---

## Summary

| # | Issue | Resolution | Impact |
|---|---|---|---|
| 1 | [#13127](https://github.com/w3c/csswg-drafts/issues/13127) | Rename `spanning-item` → `normal`; allow `none` through multicol spanners | 5 CLs, 1 PR |
| 2 | [#13137](https://github.com/w3c/csswg-drafts/issues/13137) | Initial inset value `0` with keyword for old default | 3 CLs, 1 PR |
| 3 | [#12922](https://github.com/w3c/csswg-drafts/issues/12922) | Alignment space is part of the gap; flex intersections defined | 3 CLs |
| 4 | [#13362](https://github.com/w3c/csswg-drafts/issues/13362) | No change — existing behavior is correct | 0 CLs |

**Total implementation effort from F2F resolutions:** 11 CLs, 2 spec PRs landed within 5 weeks of the meeting.

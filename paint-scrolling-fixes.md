# Gap Decorations — Paint & Scrolling Bug Fixes (pdr reviewed)

CLs where we dealt with scrolling, painting, and overflow bugs that required review from Philip Rogers (pdr@chromium.org), a Blink paint/compositing owner.

These CLs were critical for correctness — gap decorations need to interact properly with the paint pipeline, ink overflow, scrolling/resize invalidation, and the multicol paint path.

---

## CL 1: Handle Ink Overflow for Gap Decorations

| | |
|---|---|
| **CL** | [6451833](https://chromium-review.googlesource.com/c/chromium/src/+/6451833) |
| **Owner** | Sam Davis Omekara |
| **Landed** | 2025-05-06 |
| **Reviewers** | Philip Rogers (+1), Ian Kilpatrick (+1) |
| **Size** | +605/-8 |
| **Patchsets** | 15 |
| **Comments** | 29 resolved, 46 messages |

**Problem:** Gap decorations could extend beyond a container's bounds (e.g., via outset/inset properties), but the paint system didn't know about this. Decorations would get clipped or cause visual artifacts during scrolling because ink overflow wasn't being computed.

**Fix:** Computed ink overflow bounds using gap intersection geometry, ensuring the paint system allocates enough space for decorations that extend beyond the container's content box. Added tests for grid; flex and multicol handled separately.

**Why pdr:** Ink overflow computation directly affects the paint invalidation and compositing layers — pdr owns this area.

**WPT upstream:** [web-platform-tests/wpt#52368](https://github.com/web-platform-tests/wpt/pull/52368)

---

## CL 2: Fix GapDecorations in Overflow Cases

| | |
|---|---|
| **CL** | [6506379](https://chromium-review.googlesource.com/c/chromium/src/+/6506379) |
| **Owner** | Javier Contreras |
| **Landed** | 2025-05-14 |
| **Reviewers** | Philip Rogers (+1), Sam Davis Omekara (+1) |
| **Size** | +775/-31 |
| **Patchsets** | 17 |
| **Comments** | 44 resolved, 79 messages |

**Problem:** When a container had `overflow: scroll` or `overflow: hidden`, gap decorations weren't painting correctly. The decorations would either disappear entirely, render at wrong positions relative to scroll offset, or fail to repaint when scrolling.

**Fix:** Updated the gap painting code to properly account for the container's scroll offset and overflow clipping. Ensured decorations are painted in the correct coordinate space relative to the scrollable area. This was a large change (+775 lines) because it required threading scroll information through the gap geometry pipeline.

**Why pdr:** Scroll-aware painting is core paint infrastructure. The CL touched `paint_info.h` and required understanding of how paint layers interact with scrollable containers.

**WPT upstream:** [web-platform-tests/wpt#52539](https://github.com/web-platform-tests/wpt/pull/52539)

---

## CL 3: Allow Gap Decorations to be Painted on Window Resize

| | |
|---|---|
| **CL** | [6618134](https://chromium-review.googlesource.com/c/chromium/src/+/6618134) |
| **Owner** | Javier Contreras |
| **Landed** | 2025-06-06 |
| **Reviewers** | Philip Rogers (+1), Sam Davis Omekara (+1) |
| **Size** | +66/-2 |
| **Patchsets** | 5 |
| **Comments** | 6 resolved, 31 messages |

**Problem:** Gap decorations wouldn't repaint when the browser window was resized. The decorations would stay stale from the initial paint, even though the container's geometry changed (e.g., a flex container reflowing items into different lines, or a grid changing track sizes).

**Fix:** Ensured that gap geometry changes trigger paint invalidation. When the container's layout changes due to a resize, the gap decoration geometry is recomputed and a repaint is requested. Small but critical fix (+66 lines).

**Why pdr:** Paint invalidation — knowing *when* to repaint without over-invalidating — is a core concern for rendering performance. pdr needed to verify this wouldn't cause excessive repaints.

**WPT upstream:** [web-platform-tests/wpt#53003](https://github.com/web-platform-tests/wpt/pull/53003)

---

## CL 4: Implement Paint for Multicol GapDecorations

| | |
|---|---|
| **CL** | [6907139](https://chromium-review.googlesource.com/c/chromium/src/+/6907139) |
| **Owner** | Javier Contreras |
| **Landed** | 2025-09-10 |
| **Reviewers** | Philip Rogers (+1), Alison Maher (+1), Sam Davis Omekara (+1) |
| **Size** | +195/-13 |
| **Patchsets** | 14 |
| **Comments** | 41 resolved, 47 messages |

**Problem:** The optimized gap decoration pipeline (CL 6885245) introduced new layout-side gap geometry for multicol, but the paint side hadn't been implemented yet. Multicol painting is fundamentally different from grid/flex because content is fragmented across columns with their own coordinate systems.

**Fix:** Implemented the paint path for the optimized multicol gap decorations. Generated intersection data from the new main/cross gap geometry, then painted decorations within each multicol fragment's coordinate space. This was the implementation counterpart to the layout-side CL.

**Why pdr:** Multicol painting involves fragment coordinate transforms and interaction with the column painting pipeline. Three reviewers (pdr, Alison Maher, Sam Davis Omekara) reflected the complexity spanning paint, layout, and gap decorations domains.

---

## Summary

| CL | Bug Type | Owner | Landed | Size | Review Rounds |
|---|---|---|---|---|---|
| [6451833](https://chromium-review.googlesource.com/c/chromium/src/+/6451833) | Ink overflow | Sam | 2025-05-06 | +605/-8 | 15 patchsets |
| [6506379](https://chromium-review.googlesource.com/c/chromium/src/+/6506379) | Scroll/overflow | Javier | 2025-05-14 | +775/-31 | 17 patchsets |
| [6618134](https://chromium-review.googlesource.com/c/chromium/src/+/6618134) | Resize repaint | Javier | 2025-06-06 | +66/-2 | 5 patchsets |
| [6907139](https://chromium-review.googlesource.com/c/chromium/src/+/6907139) | Multicol paint | Javier | 2025-09-10 | +195/-13 | 14 patchsets |

**Total:** 4 CLs, ~1,641 lines changed, 120+ review comments, 51 patchsets across all CLs.

All four CLs required paint infrastructure expertise (pdr) because gap decorations are a new visual feature that must integrate with Blink's existing paint invalidation, ink overflow, scroll compositing, and fragment painting systems.

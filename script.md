# Sam Spiel — Gap Decorations Story Script

> Working script for the BlinkOn 2026 talk. Each section maps to slides in `sam-spiel.html`.
> Edit freely — this drives the narrative.

---

## Opening — The Hook

**[Slide 1: Title]**

**

Hello Everyone, 

My name is Sam Davis Omekara Jr. and I'm here with Javier Contreras Tenorio. We've spent the last year or so
working on a web platform API & we're here to share mini stories that culminate into the feature.

---

## Act 1 — The Origin

**[Slide 2: Launch the Problem softly: Containers ]**

On the Web, we have container types such as flex and grid that help web developers put stuff together and achieve their desired layout. I guess the two most popular dare I say is flex and grid. In Flex, we have this sort of 1D
Placement and Grid we establish this sort of 2D placement. Then a not so popular one is one called multi col
which isn't triggered by the display property but somewhat by specifying the number of columns. 


**[Slide 3: ] Hard Launch the Problem**
The thing about these containers is that they could have gutters which creates a nice effect [Find the use of these containers] but only one of them provided a way to style them: Multi Col through the Rule property
So you could say column-rule and set particular values for the style, color and width similar to borders.
There was clamor for this to be extended to other containers and the csswg picked this up in issue 
2748 And the issue was filed in 2018 with significant clamor and discussions going on


**[Slide 4: Speccing Picks Up the Thread]**

Mats Palmgreen from Mozilla had a first stab at writing a spec for this, and this informed the current spec that
we have now today which was written by Kevin Babbitt. Since Kevin is a member of our team at Microsoft,
I decided to get 4 key things he had in mind when designing this spec:
1, 2, 3, 4. 

**[Slide 5: Style → Layout → Paint]**

As Kevin was specing, we weren't folding our hands concurrently we started getting used to the
machine that is Blink Core. The nice thing about Gap Decorations is that it touches three core
layers of Blink in Style, Layout and Paint ... 

- **Style**: Parse the properties — column-rule, row-rule, shorthands, repeaters, lists
- **Layout**: Compute gap geometry for grid, flex, and multicol
- **Paint**: Draw decorations with correct clipping, overflow, and scrolling

Every CL touches at least one of these. Some touch all three.

---

## Transition — Open Questions

**[Slide 6: Transition]**

With initial spec and implementation, interesting questions were raised — which ushered us into the world of Standards.

---

## Act 2 — Iteration

**[Slide 7: Soft Launch why working with Standards]**

[Add what standards are].
[Add the importance of standards ].  We filed issues. We worked closely with the CSSWG to resolve them:

- #11492: Auto repeater behavior
- #11494: Computed value with none/hidden
- #11496: Shorthand grammar
- #12024: Outsets at edges

30 issues filed. 24 resolved. This feedback loop between implementation and spec is the engine that drove progress.

Position the wg as a partner not a blocker ...

**[Slide 8: The Great Rewrite — The Old Model]**

A significant resolution we got from the CSSWG was to suppress gaps when they hit fragmentation breaks.
That kicked off parallel work but also pushed us to reconsider the entire implementation.

For context, the model at the time treated intersections as first-class objects.
An intersection is the point where a gap meets a container edge or another gap in the cross direction.
Each gap stored a list of these intersection points. So Gap Geometry was essentially:

- A 2D vector of column gaps with their intersections
- A 2D vector of row gaps with their intersections

For a grid with m columns and n rows, that's O(m × n) storage — twice over.
On a 100×100 grid, GapGeometry alone consumed ~160KB. At 1000×1000? ~16MB. At 5000×5000? ~400MB.
Gaps in Blink were historically "throw-away" values — never stored. We were suddenly storing everything.

**[Slide 9: The Great Rewrite — Why Grid Forced the Issue]**

The deeper problem was how grid works internally in Blink. Grid tracks use compressed abstractions
called GridRanges and GridSets — the engine avoids expanding repeater tracks unnecessarily.
But for gap decorations, we had to call `ComputeExpandedPositions` to know where each track ended
(marking the start of a gap). This happened twice per layout pass.

So we were fighting the grid engine's own design philosophy: grid tries hard NOT to expand,
and we were forcing it to expand just so we could figure out where gaps lived.

After benchmarking (a meeting on 07/31), we decided the memory footprint was too much.
Expansion wasn't inherently the problem — the *result* of expansion was. If we could win memory
while still expanding, we'd be in good shape. The goal: reduce the memory footprint without
drastically affecting paint code.

**[Slide 10: The Great Rewrite — Anchor/Dependent Model]**

The solution was to rethink the data model entirely. Instead of storing all intersections,
we designed a two-direction model: **Anchor Gaps** (primary axis) and **Dependent Gaps** (orthogonal axis).

- Anchor Gap: the primary gap direction (rows in grid, gap between lines in flex)
- Dependent Gap: the cross/orthogonal gaps (columns in grid, gap between items in flex)
- Intersections are **computed on-demand** — not stored — based on container edges plus orthogonal gaps

For grid, every anchor is associated with every dependent gap (things line up).
For flex, an anchor gap only associates with the dependent gaps that actually intersect it.

The result? Two 1D vectors instead of two 2D vectors. An iterator computes intersections at paint time.
Storage drops from O(m × n) to O(m + n). That's ~75% memory reduction for grid, ~50% for flex.

Then the question became: "But what about spanning items?" Items that span multiple tracks
cast a "shadow" on gaps. We store just the span ranges (start, end), merge and sort them,
and use binary search at paint time to check if an intersection is blocked. Minimal storage, fast lookup.


**[Slide 11: Do not expand for properties, except animations]**

The same "don't expand" philosophy from the geometry rewrite carried into how we handle
gap decoration properties. Properties like `column-rule` and `row-rule` accept lists with
auto repeaters — e.g., `column-rule: 1px solid red / auto`. We were initially expanding
these lists eagerly. But in the same spirit as grid, we realized: if we only consume these
properties at paint time, and we already know the number of gaps, we can keep the list compressed.

So we built a `GapDataListIterator` (CL 6773829) — a class that traverses a GapDataList
without expanding integer repeaters. The key insight: at paint time, the number of gaps is fixed,
so we can calculate how many slots auto repeaters need. The iterator segments the list into
three logical regions:

- **Leading**: entries before the auto repeater
- **Auto**: the auto-repeating values (fills remaining slots)
- **Trailing**: entries after the auto repeater

It walks through each region, advancing slot by slot, returning the correct decoration for each
gap without ever materializing the full expanded list.

But then — animations. Smart interpolation (CSSWG #12431) needed to interpolate between two
gap data lists at computed value time, not paint time. You can't iterate lazily when you need
to compare two lists element-by-element for interpolation. So for animation, we had to expand.
This led to the V2 interpolation work: expand only when animating, keep compressed otherwise.


**[Slide 12: Paint Land — Fun but Subtle]**

Paint was a whole new world for us. The core idea sounds simple: take the geometry captured
in layout and construct rects, then call paint ops on those rects. The first CL (6101656)
introduced `PaintColumnGaps` and `PaintRowGaps` in `BoxFragmentPainter` — using stored offsets
to build rects and pass them to paint operations.

But the subtleties came fast:

- **Ink Overflow** (CL 6451833): Gap decorations can extend beyond a container's bounds via outsets.
  We had to compute ink overflow bounds using gap intersections to ensure proper painting.
  Philip Rogers (PDR) and Ian Kilpatrick reviewed this — our first real paint lesson.

- **Overflow & Clipping** (CL 6506379): When a container had `overflow: hidden` or `scroll`,
  decorations painted into the overflow space where they should've been hidden. The fix required
  adding a `ScopedBoxContentsPaintState` — but even that wasn't enough. We were painting too far
  down the stack, in the background paint path, inheriting the wrong paint states. We had to move
  gap decoration painting *after* the background and set up our own recorder. PDR reviewed this one too.

- **Paint Invalidation** (CL 6625941): Decorations wouldn't repaint when the grid structure changed
  dynamically (e.g., new items added). We had to trigger `SetShouldDoFullPaintInvalidation`
  whenever children were added or removed.

- **Resize Repaint** (CL 6618134): Gap decorations wouldn't update on window resize. The issue was
  using `ScrollingBackgroundDisplayItemClient` which doesn't get marked for invalidation — so
  `UseCachedDrawingIfPossible` returned true and skipped repainting. Switching to the regular
  `DisplayItemClient` fixed it. Another PDR review.


Then with the optimized geometry model (CL 6848348), paint had to generate intersection points
*at paint time* rather than reading precomputed ones from layout. The general principle:
content-start → main × cross gap intersections → content-end. But flex cross gaps only have
two intersections, so the pattern doesn't fully apply — requiring dynamic end-offset calculation.

Shoutout to Philip Rogers (PDR) who took his time reviewing our paint CLs and teaching us
the subtleties of the paint system. Every review was a masterclass.


---

## Transition — But Who Are We Building For?

**[Slide 13: Transition]**

But who are we building for?

Dev trial launched in Chrome/Edge 139, June 2025. We put the feature behind a flag and handed it to the people who'd actually use it.

---

## Act 3 — Resolving Loose Ends

**[Slide 14: Feedback That Changed the Spec]**

We got feedback. 12 issues from 11 developers during the dev trial. Some of them changed the spec:

**Incorporated:**
1. Ahmad Shadeed: Decorations around empty grid areas → new `rule-visibility-items` property
2. RexGalilae: Asymmetric insets → restructured `rule-inset` properties ("This made my day!")
3. Internationalization concerns (#1064) → spec language updates
4. Naming feedback (#988) → property name refinements

**Pushed to future levels:**
- Images and gradients as gap decorations
- Ornaments at crossing points
- Per-side independent styling
- Advanced pattern repeaters

The ones we pushed to future levels aren't "no" — they're "not yet." They become the roadmap.

**[Slide 15: From Intersections to Segments]**

The spec's model was built around intersections — points where gaps cross. That worked great for grid
where everything lines up. But as we implemented multicol and flex, it became clear the intersection
model was biased toward grid. It didn't generalize well.

So the spec moved from an intersection-based model to a segments-based model. Instead of defining
where gaps *cross*, we define the segments that make up a gap decoration. This was the biggest
spec rewrite since the initial draft — and it made the model work cleanly across all three container types.

**[Slide 16: January 2026 Face-to-Face]**

Worth calling out: the January 2026 CSSWG face-to-face. We got about 8 issues resolved
in a single breakout session:

1. Rename `spanning-item` → `normal`
2. Initial inset value → `0`
3. Alignment space contributes to gap size (whiteboard session with Alison Maher)
4. Gap suppression with spanners — no change needed

11 CLs + 2 spec PRs within 5 weeks after that session. When you get the right people in a room,
things move fast.

---

## Closing

**[Slide 17: By the Numbers]**

| Metric | Count |
|---|---|
| Chromium CLs | 191 |
| Spec PRs | 22 |
| CSSWG Issues | 30 |
| Contributors | 5 |
| Layout Types | 3 |
| Blink Layers | 3 |

**[Slide 18: The Feature That Touches Everything]**

- Gap decorations forced us to learn paint, layout, and style in depth
- The spec changed because implementation revealed real problems
- Developer feedback during dev trial directly shaped the final design
- Standards work isn't overhead — it's how you get the design right

**[Slide 19: Call to Action]**

Gap decorations are available behind a flag today. Try it. Break it. File issues.

The feature that started as a 2018 CSSWG issue is now real — and it's better because
developers like you pushed back, filed feedback, and helped shape it.

`column-rule`, `row-rule`, and the full gap decorations API — go build something with it.

---

## Notes / Ideas to Explore

- [ ] Should the opening be more personal? (e.g., "my first CL was renaming files...")
- [ ] Do we need more live demo moments?
- [ ] Should Act 2 hit harder on the emotional side of learning paint from scratch?
- [ ] Is the closing strong enough, or do we need a callback to the opening?
- [ ] Where do Javier's contributions get highlighted vs. Sam's?

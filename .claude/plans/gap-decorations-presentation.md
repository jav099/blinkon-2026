# Plan: BlinkOn 21 Gap Decorations Presentation

## Context

We have comprehensive reference files documenting the CSS Gap Decorations feature story in `/Users/javiercon/Documents/blinkon/`. The user wants a slide-based HTML presentation that tells this story across 3 interweaving pillars: **Design** (spec), **Implementation** (Chromium), and **CSSWG Working Group**. This is for BlinkOn 21 (April 2026), targeting a technical audience of browser engineers.

## Critical Files

- `/Users/javiercon/Documents/blinkon/gap-decorations-history.md` — Full chronological log (CLs + PRs + Issues)
- `/Users/javiercon/Documents/blinkon/feedback-tracker.md` — External developer feedback cross-referenced with CLs/PRs
- `/Users/javiercon/Documents/blinkon/paint-scrolling-fixes.md` — Paint/scrolling bugs reviewed by pdr
- `/Users/javiercon/Documents/blinkon/f2f-january-2026.md` — January 2026 F2F resolutions
- `/Users/javiercon/Documents/blinkon/gap-decorations-timeline.html` — Existing timeline (reuse design system: colors, fonts, dark theme)

## Output

Single file: `/Users/javiercon/Documents/blinkon/gap-decorations-presentation.html`

## Design Approach

- **Aesthetic:** Reuse the dark blueprint theme from `gap-decorations-timeline.html` — same `#0a0e17` background, grid pattern, noise overlay, JetBrains Mono + DM Sans fonts, and the pillar color system (blue=Impl, green=Design, amber/purple=WG)
- **Navigation:** Arrow keys / click / Space, full-viewport slides with scroll-snap, slide counter
- **Pillar badges:** Each slide gets a colored pill showing which pillar(s) it belongs to
- **Diagrams:** All rendered in HTML/CSS — grid illustrations showing gap decorations, architecture diagrams, flow charts
- **Use the `frontend-design` skill** for the implementation to get a polished, conference-quality result

## Slide Plan (25 slides, interweaving chronologically)

### Act 1: Origins (Slides 1–4)

| # | Title | Pillar | Content |
|---|-------|--------|---------|
| 1 | **Title Slide** | All | "CSS Gap Decorations: From Spec to Ship". Stats: 191 CLs, 22 PRs, 30 Issues, 8 years. Mini grid demo with animated gap lines. |
| 2 | **The Spark (2018)** | 🟢 Design | fantasai files CSSWG #2748 "Styling Gaps/Gutters". The idea sits for 6 years. Show the original issue quote. |
| 3 | **The Proposal (2024)** | 🟢🟡 Design+WG | Kevin Babbitt proposes (#10393). CSSWG RESOLVES to adopt (Jun 12, 2024). "Import the draft with shortname css-gaps." |
| 4 | **First Lines of Code (Nov 2024)** | 🟢🔵 Design+Impl | Spec + code start in parallel. Editor's Draft published Dec 13; first CL (rename gap_color → gap_data) Nov 6. Dual timeline visual. |

### Act 2: Building the Engine (Slides 5–7)

| # | Title | Pillar | Content |
|---|-------|--------|---------|
| 5 | **The Scope** | 🔵 Impl | Gap decorations touch all three pillars of Blink: Style (CSS parsing, computed style, serialization), Layout (gap geometry, intersection points), Paint (rendering, ink overflow, invalidation). 3-layer diagram. |
| 6 | **The Intersection Model** | 🔵 Impl | First approach: build intersection points between gaps, then pair them to create decoration segments. Visual of a 3×3 grid with intersection points marked. CL 6175331 "Build Gap Intersection Points" (Feb 2025). |
| 7 | **Beyond Grid: Flex & Multicol** | 🔵 Impl | Mar 2025: Extended to flex (CL 6354145 + 6367772). Apr 2025: Extended to multicol (CL 6423352). "All three layout modes now supported." Visual showing grid + flex + multicol with decorations. |

### Act 3: Feedback & Iteration (Slides 8–9)

| # | Title | Pillar | Content |
|---|-------|--------|---------|
| 8 | **Developer Voices** | 🟢 Design | After dev trial, feedback floods in. Ahmad Shadeed: empty grid areas. RexGalilae: asymmetric insets. SmashingConf Freiburg: dots instead of lines. 12 feedback issues from 11 developers. Show actual quotes. |
| 9 | **The Feedback Loop** | 🟢🟡 Design+WG | Shadeed's empty areas feedback → CSSWG issue #12602 → RESOLVED: add `rule-visibility-items: all | around | between` → 7 CLs implement it → o-t-w tests in Canary: "works well and is a great addition". RexGalilae: "This made my day!" |

### Act 4: Architecture Evolution (Slides 10–13)

| # | Title | Pillar | Content |
|---|-------|--------|---------|
| 10 | **Accumulator Design** | 🔵 Impl | Apr 2025: New architecture pattern for efficient gap geometry. The accumulator walks items/tracks once, building intersections incrementally instead of recomputing. Applied to both flex and grid. |
| 11 | **Main/Cross Gap Split** | 🔵 Impl | Aug 2025 (CL 6819000): Separated main-axis and cross-axis gaps into dedicated classes. Why? (1) Performance — avoid recomputing everything when only one axis changes. (2) Grid requirement — track expansion and fragmentation need independent axis handling. Before/after architecture diagram. |
| 12 | **Lists & The Repeater Surprise** | 🔵🟡 Impl+WG | Properties accept lists: `column-rule-width: 1px, 3px, 1px` with auto repeaters. Initially kept repeaters compact (never expand). Then interpolation arrived (CSSWG #12431, RESOLVED: use LCM method). Had to expand repeaters for animation — the V2 interpolation rewrite. |
| 13 | **Painting in Unfamiliar Territory** | 🔵 Impl | 4 CLs touching paint infrastructure (pdr reviewed). Ink overflow, scroll/overflow, resize, multicol paint. 1,641 lines, 51 patchsets, 120+ review comments. Challenge: no one on the gap decorations team was familiar with Blink's paint/compositing code. Learned a new area of the codebase. |

### Act 5: The Working Group (Slides 14–18)

| # | Title | Pillar | Content |
|---|-------|--------|---------|
| 14 | **Dev Trial: Chrome/Edge 139** | All | June 2025: Flag goes experimental (CL 6514569). Chrome blog post published Jun 11. yisibl first to confirm availability. Feature goes public. |
| 15 | **TPAC 2025: The Pivot** | 🟡 WG | Issue #12784 "Gap intersections for multicol" — discussion with rachelandrew, mstensho, alisonmaher, tabatkins, emilio, Loirooriol. Offline TPAC discussion → Kevin posts the new model: intersection points → segment endpoints. |
| 16 | **Intersections → Segments** | 🟢🟡 Design+WG | Fundamental spec overhaul. Old model: define intersection POINTS, pair them. New model: define SEGMENTS directly with endpoints. Simpler, more flexible, handles multicol naturally. Issue #13366 → PR #13299 (merged Feb 2026). Before/after spec model diagram. |
| 17 | **January F2F: Four in a Day** | 🟡 WG | Jan 28, 2026: 4 issues resolved. (1) spanning-item → normal (fantasai). (2) Inset default → 0 (developer confusion feedback). (3) Alignment space = gap size (whiteboard session). (4) Gap suppression — no change. 11 CLs + 2 PRs within 5 weeks. |
| 18 | **The Whiteboard Moment** | 🟡🔵 WG+Impl | Alison Maher's whiteboard drawing for flex gap decoration intersections. fantasai: "intersection starts when one side has a gap, ends when neither side has." Show the actual whiteboard photo link. |

### Act 6: Closing (Slides 19–25)

| # | Title | Pillar | Content |
|---|-------|--------|---------|
| 19 | **Community → Spec → Code** | All | Full feedback loop diagram: Developer files issue → WG discusses → RESOLVED → Spec PR → Chromium CLs → Feature lands → Developer confirms. Show the RexGalilae insets story end-to-end. |
| 20 | **The Cleanup** | 🔵 Impl | Sep 2025: Merged both flags into one. Removed old pipeline from Flex and Multicol. The satisfaction of deleting code you wrote 6 months ago because you built something better. |
| 21 | **Developers Confirm** | 🟢 Design | o-t-w: "works well and is a great addition to the spec." bigandy: proactively updated CodePen. RexGalilae: "This made my day!" Feedback loop closes. |
| 22 | **By the Numbers** | All | Giant stats slide. 191 CLs. 22 Spec PRs. 30 CSSWG Issues (24 with resolutions). 12 developer feedback issues. 8 years from idea to ship. 5+ contributors. ~18 months of active development. |
| 23 | **What We Learned** | All | One lesson per pillar. Design: spec and implementation must evolve together. Implementation: gap decorations touch style, layout, AND paint — be ready to learn unfamiliar code. WG: in-person meetings (F2F, TPAC) produce breakthroughs that async discussion can't. |
| 24 | **What's Next** | All | Future ideas: images/gradients as decorations, intersection ornaments, gap decoration areas (per-cell control), pseudo-element approach. These came directly from developer feedback. |
| 25 | **Thank You** | All | Credits: Sam Davis Omekara, Javier Contreras, Kevin Babbitt, Alison Maher, Kurt Catti-Schmidt. Links to spec, explainer, Chrome blog post, timeline. |

## Key Story: Intersections → Segments (the CSSWG discussion that changed everything)

The most important WG story beat: Issue **#12784** "Gap intersections for multicol containers" (filed by jav099, Sep 2025). Discussion with rachelandrew, mstensho (multicol experts), alisonmaher, tabatkins, emilio, Loirooriol. The question was about how intersection points should work in multicol (which has a fundamentally different layout model from grid).

This led to an **offline discussion at TPAC** (Nov 2025) where Kevin Babbitt, alisonmaher, tabatkins, emilio, and Loirooriol realized the entire intersection-based model was the wrong abstraction. Kevin posted the new proposal on Nov 11, 2025: move from "intersection points" to "segment endpoints" (#13366). PR #13299 implemented the spec overhaul, merged Feb 2026.

## Technical Implementation

- Self-contained single HTML file
- Full-viewport slides using CSS scroll-snap or transform-based slide switching
- Keyboard navigation: Left/Right arrows, Space/Enter
- Slide counter: "N / 25"
- All diagrams rendered in HTML/CSS (grid borders, boxes, flowcharts) — no external images
- Reuse exact CSS variables and fonts from `gap-decorations-timeline.html`
- Invoke the `frontend-design` skill for polished implementation

## Verification

1. Open `gap-decorations-presentation.html` in browser
2. Navigate through all 25 slides with arrow keys
3. Verify all links are correct (CL links, PR links, issue links)
4. Check visual consistency with existing timeline
5. Test responsive behavior (presentation should look good on a projector resolution ~1920x1080)

# Gap Decorations Feature History

All landed Chromium CLs (hashtag: `gap-decorations`), merged CSSWG PRs (`css-gaps-1`), and significant CSSWG issue events ordered by date.

**Legend:**
- **CL** = Chromium Gerrit changelist (implementation)
- **PR** = W3C CSSWG specification PR (spec authoring)
- **ISSUE** = W3C CSSWG issue event (discussion, resolution)

---

## 2018

### 2018-06-07
- **ISSUE [#2748](https://github.com/w3c/csswg-drafts/issues/2748) created** — Styling Gaps/Gutters *(fantasai)*
  > The original meta-issue that started it all. Proposed adding styling capabilities to CSS gaps/gutters, sparking the discussion that eventually led to the CSS Gap Decorations specification.

---

## 2024

### 2024-06-05
- **ISSUE [#10393](https://github.com/w3c/csswg-drafts/issues/10393) created** — Proposal: CSS Gap Decorations Level 1 *(kbabbitt)*
  > The formal proposal that kicked off the CSS Gap Decorations specification. Proposed a dedicated Level 1 spec for styling gaps in grid, flex, and multicol containers.

### 2024-06-12
- **ISSUE [#10393](https://github.com/w3c/csswg-drafts/issues/10393) RESOLVED** — "Import the draft with shortname css-gaps and title CSS Gap Decorations with kbabbitt as editor"
  > The CSSWG officially adopted the gap decorations proposal into the working group.

### 2024-11-06
- **CL [5990892](https://chromium-review.googlesource.com/c/chromium/src/+/5990892)** — Rename 'gap_color*' files to 'gap_data*' *(Sam Davis Omekara)*
  > Renamed files to reflect broader scope beyond just color.

### 2024-11-07
- **CL [5996090](https://chromium-review.googlesource.com/c/chromium/src/+/5996090)** — Rename `GapColor*` classes to `GapData*` *(Sam Davis Omekara)*
  > Class renaming to align with the broader gap data model.

### 2024-11-12
- **CL [5962852](https://chromium-review.googlesource.com/c/chromium/src/+/5962852)** — Templatize `GapData` and related classes *(Sam Davis Omekara)*
  > Generalized data structures via templates for reuse across properties.
- **CL [5990251](https://chromium-review.googlesource.com/c/chromium/src/+/5990251)** — Generalize `column-rule-color` parsing code *(Sam Davis Omekara)*
  > Refactored parsing to support multiple gap decoration properties.

### 2024-11-14
- **CL [5963080](https://chromium-review.googlesource.com/c/chromium/src/+/5963080)** — Parsing the `column-rule-width` property *(Sam Davis Omekara)*
  > Added CSS property parsing for column-rule-width.

### 2024-11-17
- **CL [6026236](https://chromium-review.googlesource.com/c/chromium/src/+/6026236)** — Mark failing tests as Timeout *(Sam Davis Omekara)*
  > Test infrastructure maintenance.

### 2024-11-19
- **CL [5967393](https://chromium-review.googlesource.com/c/chromium/src/+/5967393)** — Introduce ColumnRuleWidth on ComputedStyle *(Sam Davis Omekara)*
  > Added column-rule-width to computed style infrastructure.

### 2024-12-05
- **CL [6065161](https://chromium-review.googlesource.com/c/chromium/src/+/6065161)** — Introduce ColumnRuleStyle on ComputedStyle *(Sam Davis Omekara)*
  > Added column-rule-style to computed style infrastructure.

### 2024-12-09
- **CL [6065079](https://chromium-review.googlesource.com/c/chromium/src/+/6065079)** — Parse the `column-rule-style` property *(Sam Davis Omekara)*
  > Added CSS property parsing for column-rule-style.

### 2024-12-13
- **PR [#11115](https://github.com/w3c/csswg-drafts/pull/11115)** — Initial editor's draft *(kbabbitt)*
  > Published the first official editor's draft of css-gaps-1. Major changes from the unofficial draft: deferred gap decoration areas out of Level 1, dropped direction-relative properties, generalized rule-break to all container types.
- **CL [6093899](https://chromium-review.googlesource.com/c/chromium/src/+/6093899)** — Update spec links to point to Editor's Draft *(Kevin Babbitt)*
  > Updated implementation references to the newly published spec.

### 2024-12-17
- **CL [6081617](https://chromium-review.googlesource.com/c/chromium/src/+/6081617)** — Introduce Gap Fragment Data Structure *(Sam Davis Omekara)*
  > Core data structure for representing gap fragments during layout.

---

## 2025

### 2025-01-13
- **ISSUE [#11491](https://github.com/w3c/csswg-drafts/issues/11491) created** — Bikeshedding names for ways to break gap decorations *(kbabbitt)*
  > Discussion on naming for the rule-break property values that control how gap decorations interact with spanning items and fragmentation.
- **ISSUE [#11492](https://github.com/w3c/csswg-drafts/issues/11492) created** — Auto repeater behavior when number of gaps is less than number of values *(kbabbitt)*
  > How should the auto repeater behave when there are fewer gaps than specified decoration values?
- **ISSUE [#11494](https://github.com/w3c/csswg-drafts/issues/11494) created** — Computed value of column-rule-width with none|hidden style and lists of values *(kbabbitt)*
  > Whether the none/hidden special behavior for computed width should apply when lists of values are used.
- **ISSUE [#11496](https://github.com/w3c/csswg-drafts/issues/11496) created** — Grammar for expanded column-rule shorthand *(kbabbitt)*
  > How to define the grammar for the expanded column-rule shorthand that accepts lists of values.

### 2025-01-17
- **ISSUE [#11520](https://github.com/w3c/csswg-drafts/issues/11520) created** — Suppression of gaps/gutters across fragment breaks *(kbabbitt)*
  > How gaps and their decorations should behave when they coincide with page/column/region breaks.

### 2025-01-28
- **CL [6092603](https://chromium-review.googlesource.com/c/chromium/src/+/6092603)** — Populate Gap Data Structure with Grid Gap Offsets *(Sam Davis Omekara)*
  > Wired grid layout to populate gap fragment data with actual offsets.

### 2025-01-30
- **CL [6101656](https://chromium-review.googlesource.com/c/chromium/src/+/6101656)** — Setup paint for simple gap cases *(Sam Davis Omekara)*
  > First painting code for basic gap decoration rendering.

### 2025-01-31
- **ISSUE [#11492](https://github.com/w3c/csswg-drafts/issues/11492) RESOLVED** — "Accept the current spec text" *(Auto repeater behavior)*
  > Resolved to accept the current spec text for auto repeater behavior when there are fewer gaps than values.
- **ISSUE [#11494](https://github.com/w3c/csswg-drafts/issues/11494) RESOLVED** — "Remove none/hidden special behavior from computed value stage for column-rule-*, border-*, outline-*; keep resolved-value hack for border-width only"
  > Removed the special computed value behavior for none/hidden styles from gap decoration and related properties, preserving it only for border-width.
- **ISSUE [#11520](https://github.com/w3c/csswg-drafts/issues/11520) RESOLVED** — "If the gap overlaps a page break, or is the last content before a break, suppress it (across all container types)"
  > Established the fragmentation suppression rule for gaps at fragment boundaries.

### 2025-02-03
- **CL [6216649](https://chromium-review.googlesource.com/c/chromium/src/+/6216649)** — Parse `column-rule-break` property *(Sam Davis Omekara)*
  > Added parsing for the column-rule-break property.
- **PR [#11637](https://github.com/w3c/csswg-drafts/pull/11637)** — Fix typo *(cdoublev)*
  > Fixed a typo in the initial value of column-rule-color/row-rule-color.

### 2025-02-04
- **CL [6217445](https://chromium-review.googlesource.com/c/chromium/src/+/6217445)** — Parse `row-rule-break` property *(Sam Davis Omekara)*
  > Added parsing for the row-rule-break property.

### 2025-02-07
- **CL [6235926](https://chromium-review.googlesource.com/c/chromium/src/+/6235926)** — Parse `column-rule-outset` property *(Sam Davis Omekara)*
  > Added parsing for column-rule-outset.
- **CL [6236266](https://chromium-review.googlesource.com/c/chromium/src/+/6236266)** — Parse `row-rule-outset` property *(Sam Davis Omekara)*
  > Added parsing for row-rule-outset.

### 2025-02-12
- **CL [6175331](https://chromium-review.googlesource.com/c/chromium/src/+/6175331)** — Build Gap Intersection Points *(Sam Davis Omekara)*
  > Core algorithm for computing gap intersection geometry.
- **CL [6255757](https://chromium-review.googlesource.com/c/chromium/src/+/6255757)** — Add same type for *-rule-break properties *(Sam Davis Omekara)*
  > Type system alignment for rule-break properties.

### 2025-02-20
- **CL [6255214](https://chromium-review.googlesource.com/c/chromium/src/+/6255214)** — Remove Code Related to Row Gap Fragmentation *(Sam Davis Omekara)*
  > Cleanup of obsolete fragmentation code.

### 2025-02-21
- **CL [6283305](https://chromium-review.googlesource.com/c/chromium/src/+/6283305)** — Parse `row-rule-style` property *(Javier Contreras)*
  > Added parsing for row-rule-style.
- **PR [#11720](https://github.com/w3c/csswg-drafts/pull/11720)** — Fixes for gap intersection point definition and pairing algorithm *(kbabbitt)*
  > Updated illustration for gap intersection points with spanning items in grid. Fixed the pairing algorithm to apply rule-break as intended.

### 2025-02-22
- **PR [#11388](https://github.com/w3c/csswg-drafts/pull/11388)** — Namespace repeat() definitions *(kbabbitt)*
  > Editorial: namespaced repeat() definitions across css-grid-2 and css-gaps-1.

### 2025-02-26
- **CL [6290208](https://chromium-review.googlesource.com/c/chromium/src/+/6290208)** — Parse `gap-rule-paint-order` property *(Javier Contreras)*
  > Added parsing for gap-rule-paint-order (later renamed to rule-overlap).

### 2025-02-27
- **CL [6282766](https://chromium-review.googlesource.com/c/chromium/src/+/6282766)** — Parse `row-rule-width` property *(Javier Contreras)*
  > Added parsing for row-rule-width.

### 2025-03-04
- **CL [6288989](https://chromium-review.googlesource.com/c/chromium/src/+/6288989)** — Parse `row-rule-color` property *(Javier Contreras)*
  > Added parsing for row-rule-color.
- **CL [6300029](https://chromium-review.googlesource.com/c/chromium/src/+/6300029)** — Using Row rules for painting *(Javier Contreras)*
  > Enabled row rule properties in the paint pipeline.
- **CL [6311761](https://chromium-review.googlesource.com/c/chromium/src/+/6311761)** — Introduce GapIntersection class *(Sam Davis Omekara)*
  > New class to represent intersection points between gap segments.
- **ISSUE [#11814](https://github.com/w3c/csswg-drafts/issues/11814) created** — Gap decorations with collapsed gutters *(Loirooriol)*
  > How should gap decorations behave when gutters collapse (e.g., in grid with collapsed tracks)?

### 2025-03-05
- **CL [6313080](https://chromium-review.googlesource.com/c/chromium/src/+/6313080)** — Build GapIntersectionLists on GapGeometry *(Sam Davis Omekara)*
  > Connected intersection point computation to the geometry model.

### 2025-03-10
- **CL [6313856](https://chromium-review.googlesource.com/c/chromium/src/+/6313856)** — Mark blocked status of intersection points *(Sam Davis Omekara)*
  > Tracks which intersection points are blocked by spanning items.

### 2025-03-11
- **ISSUE [#11911](https://github.com/w3c/csswg-drafts/issues/11911) created** — Gap definition for Flex in cross-axis direction *(jav099)*
  > How to define gaps/gutters in the flex cross-axis direction for gap decoration purposes.

### 2025-03-12
- **CL [6321085](https://chromium-review.googlesource.com/c/chromium/src/+/6321085)** — Use GapIntersections to paint simple gap cases *(Sam Davis Omekara)*
  > Switched painting to use the new intersection-based model.

### 2025-03-13
- **CL [6348700](https://chromium-review.googlesource.com/c/chromium/src/+/6348700)** — Rename column/row offset to inline/block offset *(Sam Davis Omekara)*
  > Terminology alignment with CSS logical properties.

### 2025-03-18
- **CL [6362837](https://chromium-review.googlesource.com/c/chromium/src/+/6362837)** — Remove old grid gaps painting code *(Sam Davis Omekara)*
  > Removed legacy painting path in favor of new system.
- **CL [6361408](https://chromium-review.googlesource.com/c/chromium/src/+/6361408)** — Remove `GapBoundary` code from grid layout algorithm *(Sam Davis Omekara)*
  > Cleanup of obsolete boundary code.

### 2025-03-20
- **CL [6370103](https://chromium-review.googlesource.com/c/chromium/src/+/6370103)** — Fix bug with `HasColumnRule` for Flex *(Javier Contreras)*
  > Bug fix for flex container column rule detection.
- **CL [6341671](https://chromium-review.googlesource.com/c/chromium/src/+/6341671)** — Factor in `rule-break` and `rule-outset` in paint *(Sam Davis Omekara)*
  > Paint now respects rule-break and rule-outset properties.
- **CL [6354145](https://chromium-review.googlesource.com/c/chromium/src/+/6354145)** — Populate gap intersections data structure for flex *(Javier Contreras)*
  > Extended intersection model to flexbox containers.

### 2025-03-27
- **CL [6363374](https://chromium-review.googlesource.com/c/chromium/src/+/6363374)** — Remove GapBoundary code from GapFragmentData *(Sam Davis Omekara)*
  > Final cleanup of the legacy GapBoundary system.

### 2025-03-28
- **CL [6367772](https://chromium-review.googlesource.com/c/chromium/src/+/6367772)** — Paint gap decorations for flex *(Javier Contreras)*
  > First painting support for gap decorations in flex containers.
- **ISSUE [#12024](https://github.com/w3c/csswg-drafts/issues/12024) created** — Gap decoration outsets at the edges *(alisonmaher)*
  > How outsets should behave at container edges vs. between interior gaps.

### 2025-03-31
- **CL [6409171](https://chromium-review.googlesource.com/c/chromium/src/+/6409171)** — Address `rare_data_` null issue *(Javier Contreras)*
  > Fixed null dereference crash.

### 2025-04-02
- **CL [6383430](https://chromium-review.googlesource.com/c/chromium/src/+/6383430)** — Painting gaps with multiple styles and widths *(Sam Davis Omekara)*
  > Support for different decoration styles per gap segment.

### 2025-04-03
- **ISSUE [#11496](https://github.com/w3c/csswg-drafts/issues/11496) RESOLVED** — "Accept the PR to solve this issue" *(Grammar for expanded column-rule shorthand)*
  > Accepted the proposed grammar for the expanded column-rule shorthand.
- **ISSUE [#11814](https://github.com/w3c/csswg-drafts/issues/11814) RESOLVED** — "Suppress all but one decoration if gaps are coincident due to track collapsing, and use the same rules as border collapsing"
  > First resolution: apply border-collapsing-like rules when tracks collapse and gaps become coincident.

### 2025-04-10
- **CL [6442638](https://chromium-review.googlesource.com/c/chromium/src/+/6442638)** — Making `PaintGaps` conform to collapsing border model *(Javier Contreras)*
  > Aligned gap painting with CSS border collapsing rules.

### 2025-04-12
- **PR [#12051](https://github.com/w3c/csswg-drafts/pull/12051)** — Use commas instead of slashes for `*-rule` shorthands *(kbabbitt)*
  > Changed shorthand separator syntax from slashes to commas per WG resolution.

### 2025-04-14
- **CL [6405592](https://chromium-review.googlesource.com/c/chromium/src/+/6405592)** — Address bug for GapDecorations in column flexbox *(Javier Contreras)*
  > Fixed gap decorations in column-direction flex containers.
- **CL [6418283](https://chromium-review.googlesource.com/c/chromium/src/+/6418283)** — Parse `rule-color` shorthand *(Sam Davis Omekara)*
  > Added shorthand parsing for rule-color.

### 2025-04-15
- **CL [6394897](https://chromium-review.googlesource.com/c/chromium/src/+/6394897)** — Implement `gap-rule-paint-order` *(Javier Contreras)*
  > Implemented the paint ordering property for overlapping decorations.
- **ISSUE [#12084](https://github.com/w3c/csswg-drafts/issues/12084) created** — Gap intersection point definition for multi-col *(jav099)*
  > How gap intersection points should be defined for multicol containers, which have a different layout model from grid and flex.

### 2025-04-16
- **CL [6450078](https://chromium-review.googlesource.com/c/chromium/src/+/6450078)** — Split `gap_fragment_data` into .cc and .h file *(Javier Contreras)*
  > Code organization refactor.

### 2025-04-17
- **CL [6419062](https://chromium-review.googlesource.com/c/chromium/src/+/6419062)** — Parse rule-width shorthand *(Sam Davis Omekara)*
  > Added shorthand parsing for rule-width.
- **CL [6393952](https://chromium-review.googlesource.com/c/chromium/src/+/6393952)** — Use multiple colors to paint *(Sam Davis Omekara)*
  > Painting now supports multiple colors per gap decoration.

### 2025-04-18
- **CL [6469189](https://chromium-review.googlesource.com/c/chromium/src/+/6469189)** — Implementing accumulator design for flex gaps *(Javier Contreras)*
  > New accumulator pattern for flex gap geometry computation.
- **CL [6473027](https://chromium-review.googlesource.com/c/chromium/src/+/6473027)** — Serialization test *(Javier Contreras)*
  > Test coverage for CSS serialization.

### 2025-04-21
- **CL [6460775](https://chromium-review.googlesource.com/c/chromium/src/+/6460775)** — Implementing Accumulator Design in Grid Gap Logic *(Sam Davis Omekara)*
  > Extended accumulator pattern to grid layout.
- **CL [6472887](https://chromium-review.googlesource.com/c/chromium/src/+/6472887)** — Minor cleanups in BuildGapIntersectionPoints *(Kurt Catti-Schmidt)*
  > Code quality improvements.
- **CL [6418570](https://chromium-review.googlesource.com/c/chromium/src/+/6418570)** — Parse rule-style shorthand *(Sam Davis Omekara)*
  > Added shorthand parsing for rule-style.

### 2025-04-25
- **CL [6490052](https://chromium-review.googlesource.com/c/chromium/src/+/6490052)** — Move WPTs out of tentative *(Kevin Babbitt)*
  > Promoted web platform tests from tentative to official.

### 2025-04-29
- **CL [6423352](https://chromium-review.googlesource.com/c/chromium/src/+/6423352)** — Populate gap intersections for multi-col *(Javier Contreras)*
  > Extended intersection model to multi-column layout.

### 2025-04-30
- **CL [6501832](https://chromium-review.googlesource.com/c/chromium/src/+/6501832)** — Multicol: Make sure we don't create empty gap geometry *(Javier Contreras)*
  > Guard against empty geometry in multicol containers.

### 2025-05-01
- **CL [6499087](https://chromium-review.googlesource.com/c/chromium/src/+/6499087)** — Multicol: Add extra tests and move out of tentative *(Javier Contreras)*
  > Additional multicol test coverage.
- **PR [#11388](https://github.com/w3c/csswg-drafts/pull/11388)** — Namespace repeat() definitions *(kbabbitt)*
  > Editorial cleanup shared with css-grid-2.

### 2025-05-06
- **CL [6451833](https://chromium-review.googlesource.com/c/chromium/src/+/6451833)** — Handle Ink Overflow for Gap Decorations *(Sam Davis Omekara)*
  > Gap decorations now properly affect ink overflow calculations.
- **PR [#12166](https://github.com/w3c/csswg-drafts/pull/12166)** — Update to color properties in Forced Colors Mode *(alisonmaher)*
  > Updated forced colors mode to include gap decoration color properties.

### 2025-05-09
- **CL [6527789](https://chromium-review.googlesource.com/c/chromium/src/+/6527789)** — Use correct cross gap size when painting *(Javier Contreras)*
  > Fixed cross-axis gap sizing in paint.

### 2025-05-10
- **CL [6533577](https://chromium-review.googlesource.com/c/chromium/src/+/6533577)** — Avoid 0px behavior for list of column-rule-widths *(Sam Davis Omekara)*
  > Fixed edge case where 0px width in a list caused incorrect rendering.

### 2025-05-13
- **CL [6423813](https://chromium-review.googlesource.com/c/chromium/src/+/6423813)** — Parse column-rule shorthands *(Sam Davis Omekara)*
  > Added parsing for the column-rule shorthand property.
- **CL [6534250](https://chromium-review.googlesource.com/c/chromium/src/+/6534250)** — Remove redundant serialization tests *(Sam Davis Omekara)*
  > Test cleanup.

### 2025-05-14
- **CL [6506379](https://chromium-review.googlesource.com/c/chromium/src/+/6506379)** — Fix GapDecorations in overflow cases *(Javier Contreras)*
  > Fixed rendering when container has overflow.

### 2025-05-15
- **CL [6532734](https://chromium-review.googlesource.com/c/chromium/src/+/6532734)** — Avoid invalidating col rules when GapDecorations is on *(Javier Contreras)*
  > Performance optimization to avoid unnecessary invalidations.
- **ISSUE [#12201](https://github.com/w3c/csswg-drafts/issues/12201) created** — Serializing column-rule shorthand from separate longhands *(oSamDavis)*
  > How to serialize the column-rule shorthand when the individual longhands have different list lengths.

### 2025-05-19
- **CL [6533637](https://chromium-review.googlesource.com/c/chromium/src/+/6533637)** — Investigate Performance Bug *(Sam Davis Omekara)*
  > Performance analysis and optimization.
- **CL [6549136](https://chromium-review.googlesource.com/c/chromium/src/+/6549136)** — Restrict row rules to multicol, grid and flex *(Sam Davis Omekara)*
  > Row rules only apply to relevant container types.
- **CL [6520839](https://chromium-review.googlesource.com/c/chromium/src/+/6520839)** — Implement Serialization for ColumnRule *(Sam Davis Omekara)*
  > CSS serialization for column-rule property.
- **CL [6548202](https://chromium-review.googlesource.com/c/chromium/src/+/6548202)** — Invalidate Layout when *RuleStyle changes *(Javier Contreras)*
  > Proper layout invalidation on style changes.
- **CL [6520421](https://chromium-review.googlesource.com/c/chromium/src/+/6520421)** — Implement row-rule shorthand *(Sam Davis Omekara)*
  > Added the row-rule shorthand property.
- **CL [6536229](https://chromium-review.googlesource.com/c/chromium/src/+/6536229)** — Rule color is transparent fix for repeater values *(Javier Contreras)*
  > Fixed transparent color handling in repeating values.

### 2025-05-21
- **CL [6542490](https://chromium-review.googlesource.com/c/chromium/src/+/6542490)** — Mark interpolation as false for `RowRuleWidth` *(Javier Contreras)*
  > Disabled incorrect interpolation behavior.
- **CL [6553722](https://chromium-review.googlesource.com/c/chromium/src/+/6553722)** — Fix VisitedPropertiesCanParseValues Test *(Sam Davis Omekara)*
  > Test fix for visited properties parsing.
- **CL [6532847](https://chromium-review.googlesource.com/c/chromium/src/+/6532847)** — Fix `MaybeDependsOnCurrentColor` for repeater values *(Javier Contreras)*
  > Fixed currentColor dependency tracking for repeating values.

### 2025-05-22
- **CL [6543211](https://chromium-review.googlesource.com/c/chromium/src/+/6543211)** — Implement Bidirectional rule shorthand *(Sam Davis Omekara)*
  > Added the bidirectional rule shorthand property.

### 2025-05-28
- **CL [6586118](https://chromium-review.googlesource.com/c/chromium/src/+/6586118)** — Fix grid gap decorations track repeater bug *(Sam Davis Omekara)*
  > Fixed incorrect track repeater behavior in grid.

### 2025-05-29
- **CL [6593336](https://chromium-review.googlesource.com/c/chromium/src/+/6593336)** — Refactor GapAccumulator design in Grid *(Sam Davis Omekara)*
  > Architecture improvement for grid gap accumulation.
- **CL [6604257](https://chromium-review.googlesource.com/c/chromium/src/+/6604257)** — Return CSSValueList when GapDecorations is enabled *(Javier Contreras)*
  > Correct computed value representation.

### 2025-05-30
- **CL [6514569](https://chromium-review.googlesource.com/c/chromium/src/+/6514569)** — Mark GapDecorations flag as experimental *(Javier Contreras)*
  > Feature flag gating for experimental status.

### 2025-06-02
- **CL [6612702](https://chromium-review.googlesource.com/c/chromium/src/+/6612702)** — Allow row-rule-color go through the fast parser path *(Sam Davis Omekara)*
  > Performance optimization for parsing.

### 2025-06-03
- **CL [6614377](https://chromium-review.googlesource.com/c/chromium/src/+/6614377)** — Fix crash when attempting to serialize `initial` *(Sam Davis Omekara)*
  > Crash fix for initial value serialization.
- **CL [6619268](https://chromium-review.googlesource.com/c/chromium/src/+/6619268)** — Fix empty GapGeometry crash *(Sam Davis Omekara)*
  > Crash fix for empty geometry edge case.

### 2025-06-04
- **CL [6614048](https://chromium-review.googlesource.com/c/chromium/src/+/6614048)** — Avoid resolving StyleColor when not needed *(Javier Contreras)*
  > Performance optimization.
- **CL [6613102](https://chromium-review.googlesource.com/c/chromium/src/+/6613102)** — Address null-dereference in `PaintGapDecorations` *(Javier Contreras)*
  > Crash fix for null pointer in painting.

### 2025-06-05
- **CL [6622307](https://chromium-review.googlesource.com/c/chromium/src/+/6622307)** — Set GapGeometry for SimplifiedLayoutAlgorithm *(Sam Davis Omekara)*
  > Extended gap geometry to the simplified layout path.

### 2025-06-06
- **CL [6625240](https://chromium-review.googlesource.com/c/chromium/src/+/6625240)** — Fix Flex gaps not painted with only one flex line *(Javier Contreras)*
  > Fixed edge case where single flex line containers had no decorations.
- **CL [6618134](https://chromium-review.googlesource.com/c/chromium/src/+/6618134)** — Allow gap decorations to be painted on window resize *(Javier Contreras)*
  > Fixed repaint on resize.

### 2025-06-10
- **CL [6625941](https://chromium-review.googlesource.com/c/chromium/src/+/6625941)** — Invalidate Paint when GapGeometry changes *(Sam Davis Omekara)*
  > Proper paint invalidation tracking.

### 2025-06-11
- **CL [6635191](https://chromium-review.googlesource.com/c/chromium/src/+/6635191)** — Marking row-rule-color as interpolable: false *(Javier Contreras)*
  > Disabled incorrect interpolation.
- **CL [6625244](https://chromium-review.googlesource.com/c/chromium/src/+/6625244)** — Restrict width animation to just single values *(Javier Contreras)*
  > Limited animation support to single values.
- **CL [6630612](https://chromium-review.googlesource.com/c/chromium/src/+/6630612)** — Avoid empty GapGeometry for flex *(Sam Davis Omekara)*
  > Guard against empty geometry in flex.
- **ISSUE [#12326](https://github.com/w3c/csswg-drafts/issues/12326) created** — Inheritance of gap decorations within subgrid *(oSamDavis)*
  > Whether and how gap decoration properties should be inherited from parent grids into subgrids.

### 2025-06-13
- **CL [6635516](https://chromium-review.googlesource.com/c/chromium/src/+/6635516)** — Add initial tests for subgrid support *(Sam Davis Omekara)*
  > First test coverage for subgrid gap decorations.

### 2025-06-16
- **CL [6642524](https://chromium-review.googlesource.com/c/chromium/src/+/6642524)** — Add more tests for subgrid *(Sam Davis Omekara)*
  > Extended subgrid test coverage.

### 2025-06-20
- **CL [6622884](https://chromium-review.googlesource.com/c/chromium/src/+/6622884)** — VirtualTest suite with CSSGapDecorations disabled *(Javier Contreras)*
  > Test infrastructure for feature flag off state.

### 2025-06-27
- **CL [6657779](https://chromium-review.googlesource.com/c/chromium/src/+/6657779)** — Suppress Gaps in Row Flex Containers *(Sam Davis Omekara)*
  > Gap suppression logic for row flex containers.

### 2025-07-01
- **ISSUE [#12431](https://github.com/w3c/csswg-drafts/issues/12431) created** — Define interpolation behavior *(kbabbitt)*
  > How gap decoration properties should animate and transition, including handling of list-valued properties with repeaters.

### 2025-07-03
- **CL [6643003](https://chromium-review.googlesource.com/c/chromium/src/+/6643003)** — Base interpolation for *-rule-width *(Javier Contreras)*
  > Initial animation/interpolation support for rule widths.

### 2025-07-07
- **CL [6689025](https://chromium-review.googlesource.com/c/chromium/src/+/6689025)** — Don't go into `PaintColumnRules` with GapDecorations *(Javier Contreras)*
  > Prevented legacy column rule painting when gap decorations are active.

### 2025-07-12
- **CL [6723154](https://chromium-review.googlesource.com/c/chromium/src/+/6723154)** — Interpolation for *-rule-outset *(Javier Contreras)*
  > Animation support for rule-outset properties.

### 2025-07-14
- **CL [6685488](https://chromium-review.googlesource.com/c/chromium/src/+/6685488)** — Interpolation with repeaters for *-rule-width *(Javier Contreras)*
  > Repeating value interpolation for rule widths.

### 2025-07-15
- **CL [6701172](https://chromium-review.googlesource.com/c/chromium/src/+/6701172)** — Base interpolation for *-rule-color *(Javier Contreras)*
  > Initial animation support for rule colors.

### 2025-07-17
- **CL [6666279](https://chromium-review.googlesource.com/c/chromium/src/+/6666279)** — Suppress gaps for column flex containers *(Sam Davis Omekara)*
  > Gap suppression logic for column flex containers.

### 2025-07-25
- **ISSUE [#12527](https://github.com/w3c/csswg-drafts/issues/12527) created** — Values after an auto repeater when there's fewer values than gaps *(kbabbitt)*
  > How values specified after an auto repeater should be assigned when there are fewer total values than gaps.

### 2025-07-29
- **ISSUE [#12540](https://github.com/w3c/csswg-drafts/issues/12540) created** — Bikeshedding rule-paint-order *(kbabbitt)*
  > Naming discussion for the property controlling how overlapping gap decorations are painted.

### 2025-08-01
- **CL [6773829](https://chromium-review.googlesource.com/c/chromium/src/+/6773829)** — Avoid expanding GapDataList *(Sam Davis Omekara)*
  > Performance: avoided unnecessary list expansions.

### 2025-08-06
- **ISSUE [#12084](https://github.com/w3c/csswg-drafts/issues/12084) RESOLVED** — "Update definition of gap with gutter terminology"
  > Aligned the gap intersection point definition for multicol with updated gutter terminology.
- **ISSUE [#12431](https://github.com/w3c/csswg-drafts/issues/12431) RESOLVED** — "Define gap decoration animations using LCM methods"
  > Gap decoration animations use least common multiple to align list lengths during interpolation.
- **ISSUE [#12527](https://github.com/w3c/csswg-drafts/issues/12527) RESOLVED** — "Apply all decorations from left to right"
  > Values after an auto repeater are applied in forward (left-to-right) order.
- **ISSUE [#12540](https://github.com/w3c/csswg-drafts/issues/12540) RESOLVED** — "Change name to rule-overlap"
  > Renamed rule-paint-order to rule-overlap.

### 2025-08-07
- **CL [6821420](https://chromium-review.googlesource.com/c/chromium/src/+/6821420)** — Add README for Main-Cross Gap Geometry *(Sam Davis Omekara)*
  > Documentation for the new geometry model.

### 2025-08-08
- **CL [6829006](https://chromium-review.googlesource.com/c/chromium/src/+/6829006)** — Change `rule-paint-order` to `rule-overlap` *(Javier Contreras)*
  > Renamed property per CSSWG resolution.
- **CL [6734136](https://chromium-review.googlesource.com/c/chromium/src/+/6734136)** — Interpolation with repeaters for *-rule-color *(Javier Contreras)*
  > Repeating value interpolation for rule colors.
- **CL [6777697](https://chromium-review.googlesource.com/c/chromium/src/+/6777697)** — Fix underlying GapDataListInterpolationType Checkers *(Javier Contreras)*
  > Fixed interpolation type checking.
- **PR [#12584](https://github.com/w3c/csswg-drafts/pull/12584)** — Update gap intersection definition *(jav099)*
  > Updated the definition of gap intersection points to address flex container alignment with multicol and grid definitions.
- **PR [#12585](https://github.com/w3c/csswg-drafts/pull/12585)** — Change `rule-paint-order` to `rule-overlap` *(jav099)*
  > Renamed per CSSWG resolution in issue #12540.

### 2025-08-09
- **CL [6825799](https://chromium-review.googlesource.com/c/chromium/src/+/6825799)** — Introduce flag and VirtualTestSuite for optimization *(Sam Davis Omekara)*
  > Feature flag for the optimized gap geometry pipeline.

### 2025-08-11
- **PR [#12591](https://github.com/w3c/csswg-drafts/pull/12591)** — Update trailing gap decoration assignment to use values in forward order *(oSamDavis)*
  > Values now assigned left-to-right per resolution in #12527.

### 2025-08-13
- **ISSUE [#12602](https://github.com/w3c/csswg-drafts/issues/12602) created** — Gap decorations next to empty grid areas *(kbabbitt)*
  > How gap decorations should behave adjacent to empty or unoccupied grid areas.
- **ISSUE [#12603](https://github.com/w3c/csswg-drafts/issues/12603) created** — Asymmetric start and end offsets *(kbabbitt)*
  > Whether gap decoration outsets should support different values for start and end sides.

### 2025-08-14
- **CL [6819000](https://chromium-review.googlesource.com/c/chromium/src/+/6819000)** — Introduce Main and Cross gap classes *(Javier Contreras)*
  > New architecture separating main-axis and cross-axis gaps.
- **CL [6489213](https://chromium-review.googlesource.com/c/chromium/src/+/6489213)** — Disentangle Computed -Width Values from -Style *(Sam Davis Omekara)*
  > Separated width computed values from style values for correctness.

### 2025-08-19
- **CL [6839339](https://chromium-review.googlesource.com/c/chromium/src/+/6839339)** — Add UseCounter for Disentanglement change *(Sam Davis Omekara)*
  > Metrics tracking for the width/style separation.
- **CL [6854089](https://chromium-review.googlesource.com/c/chromium/src/+/6854089)** — Build Main Gaps for Grid *(Sam Davis Omekara)*
  > Optimized gap construction for grid main axis.
- **CL [6850244](https://chromium-review.googlesource.com/c/chromium/src/+/6850244)** — Build Cross Gaps for Grid *(Sam Davis Omekara)*
  > Optimized gap construction for grid cross axis.

### 2025-08-20
- **ISSUE [#12024](https://github.com/w3c/csswg-drafts/issues/12024) RESOLVED** — "Percentages resolve to the same value for edges vs. interior decorations" AND "Two longhands to set inner/outer outsets" AND "The shorthand lists inside first"
  > Three resolutions on outset behavior: percentage resolution is uniform, separate longhands for inner/outer outsets, and the shorthand syntax lists inside values first.
- **ISSUE [#12201](https://github.com/w3c/csswg-drafts/issues/12201) RESOLVED** — "Serialize a shorthand if the longhands line up, otherwise return empty string" AND "This shorthand and its longhands should use consistent punctuation"
  > Two resolutions on serialization: shorthand serializes only when longhands are compatible, and punctuation must be consistent.
- **ISSUE [#12603](https://github.com/w3c/csswg-drafts/issues/12603) RESOLVED** — "rule-outset: \<inner-start\> \<inner-end\>? [/ \<outer-start\> \<outer-end\>?]?"
  > Defined the syntax for asymmetric start/end outset values using a slash-separated syntax.

### 2025-08-21
- **CL [6850590](https://chromium-review.googlesource.com/c/chromium/src/+/6850590)** — Implemented optimized gap intersection logic for flex *(Javier Contreras)*
  > New optimized algorithm for flex gap intersections.
- **CL [6867394](https://chromium-review.googlesource.com/c/chromium/src/+/6867394)** — Restructure content offsets in GapGeometry *(Javier Contreras)*
  > Architecture cleanup for content offset handling.

### 2025-09-04
- **CL [6878302](https://chromium-review.googlesource.com/c/chromium/src/+/6878302)** — Paint code with spanning item logic *(Sam Davis Omekara)*
  > Painting correctly handles spanning items in grid.

### 2025-09-09
- **CL [6885245](https://chromium-review.googlesource.com/c/chromium/src/+/6885245)** — Implement optimized gap decoration logic for multicol *(Javier Contreras)*
  > Optimized algorithm for multicol gap decorations.

### 2025-09-10
- **CL [6907139](https://chromium-review.googlesource.com/c/chromium/src/+/6907139)** — Implement Paint for multicol GapDecorations *(Javier Contreras)*
  > Painting support for optimized multicol path.

### 2025-09-11
- **CL [6936094](https://chromium-review.googlesource.com/c/chromium/src/+/6936094)** — Fix content inline/block end for overflow cases *(Javier Contreras)*
  > Overflow handling fix.
- **ISSUE [#12784](https://github.com/w3c/csswg-drafts/issues/12784) created** — Gap intersections for multicol containers *(jav099)*
  > Detailed specification of how gap intersection points work in multicol containers with rows and spanners.

### 2025-09-17
- **PR [#12583](https://github.com/w3c/csswg-drafts/pull/12583)** — Define Interpolation behavior for GapDecorations *(jav099)*
  > Defined how rule-width and rule-color properties interpolate for animations and transitions.

### 2025-09-22
- **CL [6961880](https://chromium-review.googlesource.com/c/chromium/src/+/6961880)** — Interpolation behavior V2 for gap widths *(Javier Contreras)*
  > Updated interpolation to match spec V2 definition.
- **CL [6972460](https://chromium-review.googlesource.com/c/chromium/src/+/6972460)** — Merge both gap decorations flags into a single one *(Javier Contreras)*
  > Simplified feature flagging to a single flag.

### 2025-09-23
- **CL [6972512](https://chromium-review.googlesource.com/c/chromium/src/+/6972512)** — Remove old GapDecorations pipeline from Flexbox *(Javier Contreras)*
  > Completed migration to optimized pipeline for flex.
- **ISSUE [#12848](https://github.com/w3c/csswg-drafts/issues/12848) created** — Insets instead of outsets *(kbabbitt)*
  > Proposal to rename outset properties to inset, since values extend inward from the gap boundaries.

### 2025-09-25
- **CL [6972503](https://chromium-review.googlesource.com/c/chromium/src/+/6972503)** — Update multicol gap decorations based on new def *(Javier Contreras)*
  > Updated multicol to match the latest spec definitions.
- **CL [6981548](https://chromium-review.googlesource.com/c/chromium/src/+/6981548)** — Remove old GapDecorations pipeline from Multicol *(Javier Contreras)*
  > Completed migration to optimized pipeline for multicol.

### 2025-09-30
- **CL [6996527](https://chromium-review.googlesource.com/c/chromium/src/+/6996527)** — Prevent crash during flex cross gap painting *(Sam Davis Omekara)*
  > Crash fix in flex cross gap paint.

### 2025-10-03
- **CL [7000980](https://chromium-review.googlesource.com/c/chromium/src/+/7000980)** — Prevent crash with collapsed tracks in auto-fit rows *(Sam Davis Omekara)*
  > Crash fix for auto-fit grid scenarios.

### 2025-10-06
- **CL [7012081](https://chromium-review.googlesource.com/c/chromium/src/+/7012081)** — Place Grid suppression behind a feature flag *(Sam Davis Omekara)*
  > Feature-gated grid gap suppression behavior.

### 2025-10-07
- **CL [7013989](https://chromium-review.googlesource.com/c/chromium/src/+/7013989)** — Fix crash when painting flex cross gaps *(Sam Davis Omekara)*
  > Another crash fix in flex cross gap painting.
- **CL [6998207](https://chromium-review.googlesource.com/c/chromium/src/+/6998207)** — Flex fragmentation gap decorations *(Javier Contreras)*
  > Gap decorations survive fragmentation in flex containers.

### 2025-10-08
- **CL [7019009](https://chromium-review.googlesource.com/c/chromium/src/+/7019009)** — Move GapAccumulator for flex to its own file *(Javier Contreras)*
  > Code organization refactor.

### 2025-10-09
- **ISSUE [#12922](https://github.com/w3c/csswg-drafts/issues/12922) created** — Clarify if space from content distribution constitutes a gutter *(oSamDavis)*
  > Whether alignment space inserted by content-distribution (e.g., space-between) should be considered part of the gutter for gap decoration purposes.

### 2025-10-14
- **CL [7037021](https://chromium-review.googlesource.com/c/chromium/src/+/7037021)** — Parsing `rule-break` shorthand *(Javier Contreras)*
  > Added the rule-break shorthand property.

### 2025-10-15
- **CL [7037438](https://chromium-review.googlesource.com/c/chromium/src/+/7037438)** — Parsing `rule-outset` shorthand *(Javier Contreras)*
  > Added the rule-outset shorthand property.
- **CL [7040524](https://chromium-review.googlesource.com/c/chromium/src/+/7040524)** — Resolve crash related to collapsed tracks *(Sam Davis Omekara)*
  > Fixed crash with collapsed grid tracks.

### 2025-10-22
- **CL [7049588](https://chromium-review.googlesource.com/c/chromium/src/+/7049588)** — Add tests for subgrid fragmentation *(Sam Davis Omekara)*
  > Test coverage for subgrid with fragmentation.

### 2025-10-23
- **CL [7013948](https://chromium-review.googlesource.com/c/chromium/src/+/7013948)** — Flex fragmentation gap decorations part 2 *(Javier Contreras)*
  > Continued fragmentation support for flex.

### 2025-10-24
- **CL [7083088](https://chromium-review.googlesource.com/c/chromium/src/+/7083088)** — Moving flex fragmentation tests to own subfolder *(Javier Contreras)*
  > Test organization.
- **CL [7080904](https://chromium-review.googlesource.com/c/chromium/src/+/7080904)** — Parse column-rule-visibility-items *(Sam Davis Omekara)*
  > New property for controlling visibility of decorations near empty cells.

### 2025-10-27
- **CL [7081111](https://chromium-review.googlesource.com/c/chromium/src/+/7081111)** — Parse row-rule-visibility-items *(Sam Davis Omekara)*
  > Row variant of rule-visibility-items.
- **CL [7082317](https://chromium-review.googlesource.com/c/chromium/src/+/7082317)** — Implement layout for empty cells *(Sam Davis Omekara)*
  > Layout support for empty cell decoration behavior.

### 2025-10-31
- **CL [7091234](https://chromium-review.googlesource.com/c/chromium/src/+/7091234)** — Parse `*-rule-edge/interior-start/end-outset` *(Javier Contreras)*
  > Granular outset properties for edge vs interior positions.

### 2025-11-04
- **CL [7107919](https://chromium-review.googlesource.com/c/chromium/src/+/7107919)** — Make `column-rule-outset` into shorthand using new props *(Javier Contreras)*
  > Restructured outset as shorthand for new granular properties.
- **CL [7107409](https://chromium-review.googlesource.com/c/chromium/src/+/7107409)** — Make `row-rule-outset` into shorthand using new props *(Javier Contreras)*
  > Same restructuring for row variant.

### 2025-11-05
- **CL [7078855](https://chromium-review.googlesource.com/c/chromium/src/+/7078855)** — Interpolation behavior V2 for gap decorations color *(Sam Davis Omekara)*
  > Updated color interpolation to match spec V2.
- **CL [7108238](https://chromium-review.googlesource.com/c/chromium/src/+/7108238)** — Reintroduce `rule-outset` with new longhands *(Javier Contreras)*
  > Re-added rule-outset shorthand with new longhand structure.

### 2025-11-10
- **CL [7125761](https://chromium-review.googlesource.com/c/chromium/src/+/7125761)** — Fix multicol CrossGap's EdgeIntersection state *(Javier Contreras)*
  > Fixed edge intersection state tracking in multicol.
- **CL [7127743](https://chromium-review.googlesource.com/c/chromium/src/+/7127743)** — Rename `outset` properties to `inset` *(Javier Contreras)*
  > Property rename per CSSWG resolution (outset -> inset).
- **CL [7127964](https://chromium-review.googlesource.com/c/chromium/src/+/7127964)** — Rename test files to match `outset` to `inset` rename *(Javier Contreras)*
  > Test file renames to match property rename.

### 2025-11-11
- **CL [7131883](https://chromium-review.googlesource.com/c/chromium/src/+/7131883)** — Rule `inset` properties implementation *(Javier Contreras)*
  > Full implementation of the renamed inset properties.
- **CL [7138738](https://chromium-review.googlesource.com/c/chromium/src/+/7138738)** — Split CrossGap and MainGap impls into .cc files *(Javier Contreras)*
  > Code organization for main/cross gap separation.
- **PR [#11913](https://github.com/w3c/csswg-drafts/pull/11913)** — Adjust *-width zero value special cases per WG resolution *(kbabbitt)*
  > Cross-spec update adjusting zero-width behavior across css-backgrounds-3, css-multicol, css-ui-4, and css-gaps-1.

### 2025-11-12
- **CL [7133218](https://chromium-review.googlesource.com/c/chromium/src/+/7133218)** — Implement Paint logic for empty cells *(Sam Davis Omekara)*
  > Paint support for empty cell decoration behavior.

### 2025-11-13
- **ISSUE [#11814](https://github.com/w3c/csswg-drafts/issues/11814) RESOLVED** — "Undo previous resolution. Apply gap-decos after collapse"
  > Reversed the April 2025 resolution. Gap decorations are now applied after track collapsing rather than using border-collapsing rules.
- **ISSUE [#12602](https://github.com/w3c/csswg-drafts/issues/12602) RESOLVED** — "Add 'rule-visibility-items: all | around | between' (plus longhands)"
  > New property to control visibility of gap decorations near empty grid areas, with three values.

### 2025-11-18
- **CL [7169205](https://chromium-review.googlesource.com/c/chromium/src/+/7169205)** — Tests cases with orthogonal items *(Javier Contreras)*
  > Test coverage for items with different writing modes.

### 2025-11-19
- **CL [7170183](https://chromium-review.googlesource.com/c/chromium/src/+/7170183)** — Split GenerateMain/CrossIntersectionList *(Javier Contreras)*
  > Separated intersection list generation for main and cross axes.

### 2025-11-20
- **CL [7169664](https://chromium-review.googlesource.com/c/chromium/src/+/7169664)** — Binary search for `GetIntersectionGapSegmentState` *(Javier Contreras)*
  > Performance optimization using binary search for segment lookups.
- **ISSUE [#13127](https://github.com/w3c/csswg-drafts/issues/13127) created** — Introduction of auto for rule-break properties *(jav099)*
  > Proposed adding an auto value for rule-break to distinguish default behavior from explicit none.

### 2025-11-21
- **ISSUE [#13137](https://github.com/w3c/csswg-drafts/issues/13137) created** — Change the initial value for insets property to be 0 *(oSamDavis)*
  > Proposed changing the default inset value from -50% to 0 for more intuitive behavior.

### 2025-11-22
- **CL [7185447](https://chromium-review.googlesource.com/c/chromium/src/+/7185447)** — Perf tests for GapDecorations *(Javier Contreras)*
  > Performance benchmarks for gap decoration rendering.

### 2025-12-04
- **CL [7219490](https://chromium-review.googlesource.com/c/chromium/src/+/7219490)** — Disallow painting decorations behind spanners (multicol) *(Javier Contreras)*
  > Decorations don't paint behind multicol spanning elements.
- **CL [7219668](https://chromium-review.googlesource.com/c/chromium/src/+/7219668)** — Introduce rule-break: auto *(Javier Contreras)*
  > New auto value for rule-break property.

### 2025-12-09
- **CL [7056842](https://chromium-review.googlesource.com/c/chromium/src/+/7056842)** — Implement fragmentation for gap segments states *(Sam Davis Omekara)*
  > Gap segment state tracking across fragments.

### 2025-12-16
- **CL [7265568](https://chromium-review.googlesource.com/c/chromium/src/+/7265568)** — Rename *-{start/end}-inset to *-inset-{start/end} *(Javier Contreras)*
  > Property naming convention adjustment.

### 2025-12-17
- **PR [#13024](https://github.com/w3c/csswg-drafts/pull/13024)** — Update `outset` properties *(jav099)*
  > Major spec update restructuring the outset/inset property model.

---

## 2026

### 2026-01-05
- **PR [#13264](https://github.com/w3c/csswg-drafts/pull/13264)** — Fix missing syntax for #inset *(jav099)*
  > Corrected the syntax definition for *-rule-inset shorthands.

### 2026-01-06
- **CL [7311427](https://chromium-review.googlesource.com/c/chromium/src/+/7311427)** — Implement missing shorthands for inset properties (1) *(Javier Contreras)*
  > Added missing shorthand implementations for inset.

### 2026-01-07
- **PR [#13298](https://github.com/w3c/csswg-drafts/pull/13298)** — Update gap decoration behavior when gaps are coincident due to track collapsing *(oSamDavis)*
  > New resolution for how decorations behave when tracks collapse and gaps become coincident.

### 2026-01-13
- **CL [7452737](https://chromium-review.googlesource.com/c/chromium/src/+/7452737)** — Remove TODOs for serialization of *-rule shorthand *(Sam Davis Omekara)*
  > Cleanup of completed TODO items.

### 2026-01-15
- **CL [7453182](https://chromium-review.googlesource.com/c/chromium/src/+/7453182)** — Update property lists to be comma separated *(Sam Davis Omekara)*
  > Aligned property list syntax with spec change.
- **PR [#13336](https://github.com/w3c/csswg-drafts/pull/13336)** — Add section for serializing column-rule shorthand and update separator of longhands *(oSamDavis)*
  > Added serialization section and switched longhands from space to comma separation.

### 2026-01-16
- **PR [#13359](https://github.com/w3c/csswg-drafts/pull/13359)** — [editorial] Simplify syntax *(cdoublev)*
  > Simplified grammar using comma elision rules.

### 2026-01-20
- **CL [7496527](https://chromium-review.googlesource.com/c/chromium/src/+/7496527)** — Remove none as value for rule-visibility-items *(Sam Davis Omekara)*
  > Removed unused value from property.

### 2026-01-28
- **CL [7524550](https://chromium-review.googlesource.com/c/chromium/src/+/7524550)** — Implement `rule-visibility-items` shorthand *(Sam Davis Omekara)*
  > Added the rule-visibility-items shorthand property.
- **ISSUE [#12922](https://github.com/w3c/csswg-drafts/issues/12922) RESOLVED** — "Alignment space inserted into gutters contributes to the gap size" AND "Rules drawn between flex lines have an intersection that starts when there's a gap on one or both sides"
  > Two resolutions: content-distribution spacing is part of the gap size, and flex line intersections exist when gaps are present.
- **ISSUE [#13127](https://github.com/w3c/csswg-drafts/issues/13127) RESOLVED** — "Rename 'spanning-item' to 'normal'" AND "Allow rule-break: none to draw through spanners in multicol"
  > Two resolutions: renamed the default rule-break keyword to normal, and allowed none to draw through multicol spanners.
- **ISSUE [#13137](https://github.com/w3c/csswg-drafts/issues/13137) RESOLVED** — "Change initial value to 0, with a keyword for the current default"
  > Changed the default inset value from -50% to 0, adding a keyword for the previous default behavior.
- **ISSUE [#13362](https://github.com/w3c/csswg-drafts/issues/13362) RESOLVED** — "Close, no change" *(Gap suppression with spanning items)*
  > Closed with no spec change needed for gap suppression with spanning items.

### 2026-01-30
- **CL [7518847](https://chromium-review.googlesource.com/c/chromium/src/+/7518847)** — Fix grid auto fit tracks bug with for gap suppression *(Sam Davis Omekara)*
  > Fixed auto-fit track gap suppression.
- **CL [7533197](https://chromium-review.googlesource.com/c/chromium/src/+/7533197)** — Rename rule-break value 'spanning-item' to 'normal' *(Javier Contreras)*
  > Renamed per CSSWG resolution.
- **CL [7533202](https://chromium-review.googlesource.com/c/chromium/src/+/7533202)** — Remove rule-break: auto, make normal the default *(Javier Contreras)*
  > Simplified rule-break values.

### 2026-02-03
- **CL [7533997](https://chromium-review.googlesource.com/c/chromium/src/+/7533997)** — Update the default value of interior insets to 0 *(Sam Davis Omekara)*
  > Changed default per spec resolution.
- **CL [7540394](https://chromium-review.googlesource.com/c/chromium/src/+/7540394)** — Add tests for subgrid and empty cells *(Sam Davis Omekara)*
  > Test coverage for subgrid empty cell behavior.
- **CL [7537987](https://chromium-review.googlesource.com/c/chromium/src/+/7537987)** — Re-allow painting decorations behind spanners *(Javier Contreras)*
  > Reverted previous restriction per updated spec direction.
- **PR [#13374](https://github.com/w3c/csswg-drafts/pull/13374)** — Add section for rule visibility items *(oSamDavis)*
  > New spec section defining rule-visibility-items behavior near empty cells.
- **PR [#13413](https://github.com/w3c/csswg-drafts/pull/13413)** — Make initial value of interior insets '0' *(oSamDavis)*
  > Changed default from -50% to 0 per resolution.

### 2026-02-04
- **CL [7537839](https://chromium-review.googlesource.com/c/chromium/src/+/7537839)** — Re-allow row gap decorations to break in multicol *(Javier Contreras)*
  > Adjusted multicol row decoration breaking behavior.
- **CL [7542894](https://chromium-review.googlesource.com/c/chromium/src/+/7542894)** — Gap decorations should be applied in 0px flex gaps *(Sam Davis Omekara)*
  > Decorations render even when gap is 0px in flex.
- **PR [#13446](https://github.com/w3c/csswg-drafts/pull/13446)** — Add more wpts for empty cells *(oSamDavis)*
  > Additional web platform tests for empty cell behavior.
- **ISSUE [#13453](https://github.com/w3c/csswg-drafts/issues/13453) created** — Multicol: Allow column rules to go through row gaps *(jav099)*
  > Whether column decorations in multicol should extend through row gaps, similar to how grid column decorations work.

### 2026-02-05
- **CL [7541037](https://chromium-review.googlesource.com/c/chromium/src/+/7541037)** — Gap decorations should be applied in 0px grid gaps *(Sam Davis Omekara)*
  > Decorations render even when gap is 0px in grid.

### 2026-02-10
- **CL [7559334](https://chromium-review.googlesource.com/c/chromium/src/+/7559334)** — Resolve normal rule-break in multicol containers *(Javier Contreras)*
  > Implemented normal rule-break resolution for multicol.
- **ISSUE [#13477](https://github.com/w3c/csswg-drafts/issues/13477) created** — Decorations between empty areas: default behaviors *(jav099)*
  > What the default behavior should be for gap decorations between empty grid areas.

### 2026-02-11
- **CL [7564851](https://chromium-review.googlesource.com/c/chromium/src/+/7564851)** — Add test for gap suppression with spanning item *(Sam Davis Omekara)*
  > Test coverage for spanning item gap suppression.
- **CL [7564755](https://chromium-review.googlesource.com/c/chromium/src/+/7564755)** — Add wpts for collapsed tracks and gap decorations *(Sam Davis Omekara)*
  > Web platform tests for collapsed track scenarios.
- **CL [7563618](https://chromium-review.googlesource.com/c/chromium/src/+/7563618)** — Introduce rule-visibility-items: auto *(Javier Contreras)*
  > New auto value for rule-visibility-items.

### 2026-02-13
- **CL [7539476](https://chromium-review.googlesource.com/c/chromium/src/+/7539476)** — Overhaul multicol gap decorations *(Javier Contreras)*
  > Major refactor of multicol gap decoration architecture.
- **CL [7571904](https://chromium-review.googlesource.com/c/chromium/src/+/7571904)** — Add missing spanner main gap in cases with just one column *(Javier Contreras)*
  > Edge case fix for single-column multicol with spanners.
- **CL [7552129](https://chromium-review.googlesource.com/c/chromium/src/+/7552129)** — Introduce the `GapIntersection` class *(Sam Davis Omekara)*
  > New intersection class for the optimized pipeline.
- **PR [#13475](https://github.com/w3c/csswg-drafts/pull/13475)** — Change `rule-break: spanning-item` to `rule-break: normal` *(jav099)*
  > Renamed spanning-item to normal as the magic keyword for default behavior.

### 2026-02-16
- **CL [7576367](https://chromium-review.googlesource.com/c/chromium/src/+/7576367)** — Allow visibility rules to affect `rule-break:none` *(Sam Davis Omekara)*
  > Visibility rules now interact with rule-break:none.

### 2026-02-17
- **CL [7559831](https://chromium-review.googlesource.com/c/chromium/src/+/7559831)** — Implement empty cells in Multicol containers *(Javier Contreras)*
  > Empty cell handling for multicol.
- **PR [#13299](https://github.com/w3c/csswg-drafts/pull/13299)** — Change decorations to be segments based rather than intersections based *(jav099)*
  > Fundamental spec change: moved from intersection-based to segment-based model.

### 2026-02-19
- **CL [7560010](https://chromium-review.googlesource.com/c/chromium/src/+/7560010)** — Handle overlaps with flex main gaps intersections *(Sam Davis Omekara)*
  > Overlap handling for flex main-axis intersections.

### 2026-02-20
- **CL [7596534](https://chromium-review.googlesource.com/c/chromium/src/+/7596534)** — Update instances of 'outset' to 'inset' in tests *(Kevin Babbitt)*
  > Test updates for property rename.
- **CL [7591773](https://chromium-review.googlesource.com/c/chromium/src/+/7591773)** — Implement *-rule-inset-start/end shorthands *(Javier Contreras)*
  > Added inset start/end shorthand properties.
- **CL [7585696](https://chromium-review.googlesource.com/c/chromium/src/+/7585696)** — Allow space from content-distribution in flex (cross) *(Javier Contreras)*
  > Gap decorations account for content-distribution spacing in flex cross axis.

### 2026-03-04
- **CL [7629625](https://chromium-review.googlesource.com/c/chromium/src/+/7629625)** — Fix bug with overlap windows marking *(Sam Davis Omekara)*
  > Fixed overlap computation.

### 2026-03-05
- **CL [7589305](https://chromium-review.googlesource.com/c/chromium/src/+/7589305)** — Allow space from content-distribution in flex (main) *(Javier Contreras)*
  > Gap decorations account for content-distribution spacing in flex main axis.

### 2026-03-06
- **CL [7642513](https://chromium-review.googlesource.com/c/chromium/src/+/7642513)** — Fix crash found by fuzzer in flex gap decorations *(Javier Contreras)*
  > Crash fix discovered via fuzzing.
- **CL [7615749](https://chromium-review.googlesource.com/c/chromium/src/+/7615749)** — Suppress full effective gaps for flex containers *(Javier Contreras)*
  > Gap suppression when effective gap is fully consumed.

### 2026-03-12
- **CL [7652040](https://chromium-review.googlesource.com/c/chromium/src/+/7652040)** — Address flex test marked as failing and found by fuzzer *(Javier Contreras)*
  > Fixed fuzzer-discovered test failure.
- **CL [7659726](https://chromium-review.googlesource.com/c/chromium/src/+/7659726)** — Remove dead code *(Kurt Catti-Schmidt)*
  > Cleanup.

### 2026-03-13
- **ISSUE [#13648](https://github.com/w3c/csswg-drafts/issues/13648) created** — More vertical-writing cases *(xfq)*
  > Internationalization review: additional vertical writing mode cases that need consideration for gap decorations.

### 2026-03-16
- **ISSUE [#13663](https://github.com/w3c/csswg-drafts/issues/13663) created** — Assigning multiple decoration values to flex containers *(oSamDavis)*
  > How multiple decoration values should be assigned to gaps in flex containers, which have a variable number of gaps.

### 2026-03-18
- **ISSUE [#11491](https://github.com/w3c/csswg-drafts/issues/11491) RESOLVED** — "Close, no change" *(Bikeshedding rule-break names)*
  > Closed the naming discussion with no changes to rule-break property names.
- **ISSUE [#13137](https://github.com/w3c/csswg-drafts/issues/13137) RESOLVED** — "Add an 'overlap-join' keyword to rule-inset"
  > Added a new keyword to rule-inset that controls how overlapping decorations join at intersection points.
- **ISSUE [#13453](https://github.com/w3c/csswg-drafts/issues/13453) RESOLVED** — "Column decorations can go through row gaps (same as grid rows). No change to spec."
  > Confirmed that multicol column decorations pass through row gaps, matching grid behavior. No spec change needed.

### 2026-03-20
- **ISSUE [#13697](https://github.com/w3c/csswg-drafts/issues/13697) created** — overlap-join with between rule visibility *(kbabbitt)*
  > How the overlap-join keyword interacts with rule-visibility-items: between.

### 2026-03-27
- **CL [7671617](https://chromium-review.googlesource.com/c/chromium/src/+/7671617)** — Implement paint behavior for `overlap-join` *(Sam Davis Omekara)*
  > Painting support for the overlap-join value of rule-overlap.

### 2026-03-31
- **ISSUE [#13477](https://github.com/w3c/csswg-drafts/issues/13477) RESOLVED** — "Default Grid behavior for empty spaces is 'all'" AND "Change the initial value to 'normal'"
  > Two resolutions: grid defaults to showing decorations near all empty spaces, and the initial value of rule-visibility-items changed to normal.
- **ISSUE [#13663](https://github.com/w3c/csswg-drafts/issues/13663) RESOLVED** — "Adopt linear assignment. Investigate whether to drop the gap coinciding with the break."
  > Flex containers use linear (sequential) assignment of decoration values to gaps.
- **ISSUE [#13697](https://github.com/w3c/csswg-drafts/issues/13697) RESOLVED** — "Trim the unjoined segments. TBD how exactly that should work."
  > When overlap-join is used with between visibility, unjoined segments are trimmed. Exact mechanism to be defined.

### 2026-04-01
- **ISSUE [#13754](https://github.com/w3c/csswg-drafts/issues/13754) created** — Dropping decorations for suppressed gaps *(kbabbitt)*
  > How gap decorations should behave when their underlying gap is suppressed (e.g., at fragment boundaries).

### 2026-04-02
- **PR [#13755](https://github.com/w3c/csswg-drafts/pull/13755)** — Add 'normal' value to 'rule-visibility-items' *(kbabbitt)*
  > Added normal as a value for rule-visibility-items per resolution.

### 2026-04-06
- **CL [7719758](https://chromium-review.googlesource.com/c/chromium/src/+/7719758)** — Rename rule-visibility-items initial value from auto to normal *(Javier Contreras)*
  > Aligned implementation with spec resolution.

### 2026-04-13
- **CL [7756279](https://chromium-review.googlesource.com/c/chromium/src/+/7756279)** — Update readme with changes for multicol architecture *(Javier Contreras)*
  > Documentation update.
- **CL [7757921](https://chromium-review.googlesource.com/c/chromium/src/+/7757921)** — Add grid gap decoration test for `donut` scenario *(Javier Contreras)*
  > Test for complex grid gap pattern.
- **PR [#13732](https://github.com/w3c/csswg-drafts/pull/13732)** — Clarify application of writing-mode and direction to gap decorations *(kbabbitt)*
  > Clarified how writing-mode and direction affect gap decoration rendering.
- **ISSUE [#13648](https://github.com/w3c/csswg-drafts/issues/13648) RESOLVED** — "Accept edits defining writing mode and direction interactions with gap decorations"
  > Accepted the proposed spec edits addressing internationalization concerns for vertical writing modes and direction.

---

## Summary

| Metric | Count |
|--------|-------|
| **Total Chromium CLs** | 191 |
| **Total CSSWG Spec PRs** | 22 |
| **Total CSSWG Issues** | 30 (24 with resolutions) |
| **Total Changes** | 243 |
| **Date Range** | Jun 2018 - Apr 2026 |
| **Contributors (CLs)** | Sam Davis Omekara, Javier Contreras, Kevin Babbitt, Kurt Catti-Schmidt, Alison Maher |
| **Contributors (PRs)** | kbabbitt, jav099, oSamDavis, cdoublev, alisonmaher |
| **Contributors (Issues)** | kbabbitt, jav099, oSamDavis, fantasai, Loirooriol, alisonmaher, xfq |

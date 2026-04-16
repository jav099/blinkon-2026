# Second Draft For Slides
## Requirements:
### Overall Idea:
- Similar to gap-decorations-presentation.html, we want to build around the 3 pillars:
    - ***Design***: kevin spec, original issue, feedback from developers that we acted upon (found in our feedback file), dev trials.
    - ***Implementation***: Key things about implementation we want to tell, is how the feature scopes all around blink (style, layout, paint). Why the move to the MainGap Cross gap architecture happened (performance gains, grid requirement due to track expansion/fragmentation). We also want to mention the support for lists stye/width/color, which led to the iterator change since we thought we'd never want to expand repeaters, but then smart interpolation came along and we ended up having tp expand for those scenarios. Another key implementation point will be the PDR changes we mentioned about fixing scrolling/overflow since this was a different part of the codebase, and internally we had no one familiar with this.
    - ***CSSWG***: We want to include the story of the issues here, why and how this is important. How discussions in issues with the working group led to important changes (like the spec overhaul to move towards segments rather than intersections, which came from a discussion with the working group (make sure you find which one)). the january 2026 wg face to face success where we resolved lots of issues.
### Theme/Style:
- Simple but professional. A little less busy than the one for gap-decorations-presentation.html.
- When it makes sense, we want to use actual gap decorations. https://www.w3.org/TR/css-gaps-1/, for illustrations or examples of what the slide has. DONT OVERDO THIS. Only if it makes sense in a slide's context.

### Sources:
- Important: If you mention CLs/PRs/issues, make sure to have them as hyperlinks that we can click on.

### Text and Language:
- Simple and clear.
- Don't put too much text on a slide. Use bullet points and visuals to convey information effectively.

## Narrative Structure:
This section will outline which main points we want to hit in each act, and what pillar they belong to.
Between acts, there will be transition slides (could be multiple). KEEP IN MIND THAT DESPITE THIS STRUCTURE, IT SHOULD REMAIN IN CHRONOLOGICAL ORDER.
- We want to tell the story in terms of Acts, with the 3 pillars interwoven throughout.
- Act 1:
    - Original issue from 2019. Dont call it "the Spark" but use another name. (Design)
    - Kevin Spec (Design)
    - Style Layout Paint, how the feature spans these areas in blink. (IMPORTANT) (Implementation)
- Transition: Initial implementation created open questions and issues on both the design and implementation side, which we had to address before we could move forward.
- Act 2:
    - Discussions had to happen with the working group to resolve open questions. This was a key part of the process, and led to important changes in the spec. This also highlighted the importance of working with standards bodies when developing new features. (IMPORTANT) (CSSWG).
    - The move to MainGap CrossGap architecture and the reasons behind it (performance gains, grid requirement due to track expansion/fragmentation). (Implementation)
    - PDR changes we mentioned about fixing scrolling/overflow since this was a different part of the codebase, and internally we had no one familiar with this. (Implementation)
    - List of style/width/color support, and how this led to the iterator change since we thought we'd never want to expand repeaters, but then smart interpolation came along and we ended up having to expand for those scenarios. (Implementation). Note: Maybe this slide can have some animating gap decorations that change the color or width (of a list color/width)?
- Transition: Dev trials. This then led to more feedback from developers, which we had to act upon and iterate on the design and implementation.
- Act 3:
    - Start with the discussion that was had at TPAC 2025 and the discussion that happened there (what led to it etc), which led to the major Segments overhaul for the spec. (CSSWG + Design)
    - The success we had at the January 2026 WG face to face, where we were able to resolve lots of open issues and get a lot of feedback from the working group that helped us improve the design and implementation. (CSSWG + Design)
    - Final thoughts and conclusion. (We might edit this one later).


## Verification:
- Run the de-ai-ify skill at the end to make sure we don't have AI-sounding generated text.
- Spawn an agent to review the slides for clarity, simplicity.
- Spawn a UI UX agent to review the design and style of the slides.
- Spawn an agent to cross reference all the information on the slides, with the original sources in the other files of the repo.

## Customization:
- After you are done, create some kind of mardown file (or something) that contains the content of each slide, such that if we want to edit certain slides, we can just edit that file and regenerate the slides from there.
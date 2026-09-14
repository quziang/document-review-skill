# Basic adaptive review

This example illustrates the default one-question-at-a-time interaction.

## Source excerpt

> A cache improves average access time by keeping frequently used data close to the processor. Temporal locality means recently accessed data is likely to be accessed again. Spatial locality means nearby data is likely to be accessed soon.

## Expected interaction

**User:** Quiz me on this section.

**Assistant:** I've identified the main ideas. I'll ask one question at a time and adjust based on your answers.

**Question 1:** What is the difference between temporal locality and spatial locality?

**User:** Temporal locality means the same data is likely to be used again. Spatial locality means data near it is likely to be used.

**Assistant:** Correct. Temporal locality concerns recently accessed data being accessed again; spatial locality concerns nearby data being accessed soon.

**Question 2:** Why can a cache reduce average access time?

**User:** Because frequently used data can be kept closer to the processor.

**Assistant:** Correct.

At this point the concepts can tentatively be treated as strong for this level of questioning. A later question may increase difficulty rather than repeat the definitions.

**Question 3:** Suppose a program scans an array sequentially exactly once. Which kind of locality is most relevant, and why?

## Behavior being tested

- One question is asked at a time.
- Correct answers cause the skill to move forward or increase depth.
- The skill moves from recall/explanation toward application after demonstrated understanding.
- It does not dump a pre-generated question list.

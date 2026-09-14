# Basic adaptive review

## Source excerpt

> A cache improves average access time by keeping frequently used data close to the processor. Temporal locality means recently accessed data is likely to be accessed again. Spatial locality means nearby data is likely to be accessed soon. On a cache miss, the requested data must be fetched from a slower memory level.

## Expected interaction

**User:** Quiz me on this section.

**Assistant:** I'll ask one question at a time and revisit anything that needs practice. What does temporal locality mean?

**User:** Data used recently will probably be used again.

**Assistant:** Correct. What does spatial locality mean?

Internal state: temporal locality / recall = `partial`, one unassisted success. The paraphrase is accepted; application is still `untested`.

**User:** Data near something just accessed is likely to be used soon.

**Assistant:** Correct. What happens on a cache miss?

**User:** The requested data is fetched from slower memory.

**Assistant:** Correct. Describe the access pattern that temporal locality predicts.

Internal scheduling: two questions on other concepts have been completed since the temporal-locality answer. This changed recall question is eligible for confirmation.

**User:** Recently accessed items tend to be accessed again.

**Assistant:** Correct. Why can a cache reduce average access time?

Internal state: temporal locality / recall = `strong`, supported by two different, spaced, unassisted answers. Its application level is still `untested`.

After the learner answers the cache-purpose question, a useful deeper question is:

**Assistant:** Suppose neighboring array elements occupy neighboring memory locations and a program scans the array once. Which kind of locality is most relevant, and why?

The scenario supplies the memory-layout assumption. A correct answer would be the first evidence for spatial locality / application; it would not automatically confirm that level.

## Checks

- Ask one question per assistant turn and wait for the learner.
- Accept equivalent wording.
- Distinguish one independent success from spaced confirmation.
- Keep recall and application evidence separate.
- Interleave other concepts and supply necessary scenario assumptions.

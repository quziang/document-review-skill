# Source-grounded grading

## Source excerpt

> The company changed its pricing model in 2025 primarily because enterprise customers requested more predictable annual budgeting. The document does not attribute the change to competitor pricing.

## Expected interaction

**Assistant:** According to the document, why did the company change its pricing model in 2025?

**User:** Probably because competitors were cheaper and they needed to respond.

**Assistant:** The paragraph doesn't support the competitor explanation. It points to a request from a particular customer group. What did those customers want?

This cue preserves a chance to retrieve the missing reason. The initial unsupported answer does not count as correct simply because it sounds plausible.

**User:** They wanted to predict their spending for the year.

**Assistant:** Yes. Enterprise customers wanted more predictable annual budgeting. That's the reason given in this paragraph.

Internal state: pricing-change cause / explanation = `partial`, assistance = `cue`. The paraphrase is correct, but this assisted answer is not independent confirmation. Revisit after other concepts when the wider material provides them.

## If the retry fails

**User:** I don't remember.

**Assistant:** Enterprise customers wanted more predictable annual budgeting. That's the reason given in the paragraph; we'll come back to it later.

Keep the concept `weak`. If the learner immediately repeats "predictable annual budgeting," record an echo of `answer_revealed`, not an independent success. Do not ask an immediate reformulation just to manufacture a correct answer.

## Ambiguous source or question

Alternative source:

> Several factors contributed to the pricing change, including customer feedback and market conditions.

If the assistant had asked for the single main cause, the question would be unsupported. A response naming customer feedback must not be marked wrong for failing to name a nonexistent uniquely correct cause.

**Assistant:** My question assumed the document identified one main cause, but it doesn't. It mentions both customer feedback and market conditions. I'll replace that question.

Internal verdict: `ungradable`; leave mastery unchanged. A future question can ask for the factors the paragraph actually lists.

## Checks

- Grade against the source and accept equivalent phrasing.
- Give a useful cue without immediately supplying the full answer.
- Separate assisted correctness from independent evidence.
- Preserve source uncertainty and repair unfair questions without penalizing the learner.

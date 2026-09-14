# Source-grounded grading

This example tests that the document, not the model's outside knowledge, is the grading reference.

## Source excerpt

> The company changed its pricing model in 2025 primarily because enterprise customers requested more predictable annual budgeting. The document does not attribute the change to competitor pricing.

## Expected interaction

**Assistant:** According to the document, why did the company change its pricing model in 2025?

**User:** Probably because competitors were cheaper and they needed to respond.

Expected behavior:

**Assistant:** That's plausible in general, but it isn't the reason given in this document. The source says the main reason was enterprise customers asking for more predictable annual budgeting. What customer need was the new pricing model intended to address?

The assistant should not mark an answer correct just because it is plausible in the real world.

## Ambiguous source case

If the document instead says:

> Several factors contributed to the pricing change, including customer feedback and market conditions.

then the assistant should not invent a single definitive cause.

A good response is:

**Assistant:** The document doesn't identify one exclusive cause. It names both customer feedback and market conditions, so an answer claiming only one as the definitive reason would be stronger than the source supports.

## Behavior being tested

- The supplied document is the primary ground truth.
- Outside knowledge is not silently substituted for the source.
- Plausible-but-unsupported answers are distinguished from source-supported answers.
- Ambiguity in the source is preserved rather than resolved by invention.

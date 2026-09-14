# Error recovery and later revisit

## Source excerpt

> TCP's three-way handshake allows both endpoints to synchronize initial sequence numbers and helps prevent delayed duplicate connection requests from being mistaken for a valid new connection.

## Expected interaction

**Assistant:** According to this paragraph, what two purposes does the handshake serve?

**User:** It synchronizes the sequence numbers on both sides.

**Assistant:** That's one purpose. The other concerns requests that arrive late. What problem could those requests create?

The first answer is `incomplete`. The cue gives a category but leaves the missing claim to retrieve. The retry is part of the same question round.

**User:** I don't know.

**Assistant:** A delayed duplicate request could be mistaken for a valid new connection. The handshake helps prevent that, according to this paragraph. We'll revisit it after some other topics.

Internal result: handshake purposes / explanation remains `partial`; synchronization was independently supplied, but the second purpose was revealed. There is no complete independent success at this level. The assistant moves to a different concept from the learner's material; this short excerpt alone does not provide two other concepts for spaced review.

## Later revisit in a larger document

After two completed questions on other concepts from the provided material:

**Assistant:** Explain both roles of the handshake described in the paragraph.

**User:** It aligns initial sequence numbers and keeps an old duplicate request from being accepted as a new connection.

**Assistant:** Correct; you recalled both purposes independently this time.

Internal result: `partial`, now with one independent success. A further different, unassisted explanation question after spacing can confirm `strong`. If the session ends here, report the improvement and the pending confirmation.

## Other recovery cases

For a reversed causal claim, identify the claim that conflicts with the text and cue the learner to check the cause and effect. If a correction necessarily reveals the relationship, record it as `answer_revealed`; a repeated correction is not independent retrieval.

If the user explicitly says "just tell me," give the concise answer immediately. Do not force a retry or upgrade the state because the explanation was read.

## Checks

- Ask for the actual number of essential points expected.
- Use a cue before explanation unless the learner requests the answer.
- Keep one retry brief and record assistance.
- Preserve demonstrated partial knowledge without claiming full understanding.
- Count completed questions on other concepts, not conversational turns, as spacing.

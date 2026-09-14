# Error recovery and later revisit

This example tests the most important V1 behavior: an incorrect answer should trigger diagnosis, a minimal hint, a retry, and later re-testing rather than immediate answer dumping.

## Source excerpt

> TCP's three-way handshake allows both endpoints to synchronize initial sequence numbers and helps prevent delayed duplicate connection requests from being mistaken for a valid new connection.

## Case 1: misconception / shallow answer

**Assistant:** Why does TCP use a three-way handshake?

**User:** So both computers know the other one is online.

Expected behavior:

**Assistant:** That's not the main reason emphasized in the source. Think about what state both endpoints need to establish, especially sequence numbers. What does the handshake let them synchronize?

The assistant should not immediately reveal the whole answer.

## Case 2: retry becomes incomplete

**User:** It synchronizes both sides' sequence numbers.

Expected behavior:

**Assistant:** Yes, that's one key reason. The source gives one more reason involving old or delayed connection requests. Can you recall it?

The answer is now `incomplete`, not fully wrong.

## Case 3: retry fails

**User:** I don't remember.

Expected behavior:

**Assistant:** The other reason is to help prevent delayed duplicate connection requests from being mistaken for a valid new connection. We'll come back to this idea later.

Internal result:

- sequence-number synchronization: `partial` or stronger depending on later evidence,
- duplicate-request protection: `weak`,
- schedule the broader handshake concept for later revisit.

The assistant should now move on to other concepts instead of immediately asking the same question again.

## Later revisit

After several unrelated questions:

**Assistant:** Suppose an old connection request is delayed in the network and arrives much later. What problem does the three-way handshake help avoid in that situation?

This is intentionally not the same wording as the original question.

If the user now answers independently that the handshake helps prevent an old/delayed request from being accepted as a valid new connection, the skill has evidence that retrieval improved.

## Behavior being tested

- A wrong answer is diagnosed rather than merely labeled wrong.
- The first intervention is a minimal hint.
- The user gets a meaningful retry.
- A partial retry is distinguished from a misconception.
- The full explanation is revealed only after retrieval fails.
- Seeing the explanation does not immediately produce `strong` mastery.
- The concept is spaced with other questions before being tested again.
- The revisit changes question form or wording.

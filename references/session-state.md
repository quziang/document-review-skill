# Session state

Use a compact internal ledger, not a transcript of every exchange. This format is illustrative; equivalent tables are fine if the evidence and pending work remain recoverable.

## Minimal fields

- `source`: file/title, edition or content identifier if known, readable scope, unavailable sections.
- `goal`: scope, requested format and level, time or question limit.
- `completed_round`: top-level questions attempted; retries share the same round.
- `last_selection`: `new` or `revisit`, used to preserve broad coverage.
- `concepts`: stable ID, source anchor, importance, and a record for each relevant level.
- Per level: state, recent evidence, last exposure round, and pending revisit.
- Per evidence item: round, question/task, verdict, assistance, format, and a short description of what the learner independently supplied.

Example after one incomplete answer, a cue, and a successful retry:

```yaml
source: "Network notes, handshake paragraph"
goal: "Review this section; open-ended; explanation"
completed_round: 1
last_selection: new
concepts:
  handshake-purpose:
    anchor: "Handshake paragraph, sentences 1-2"
    importance: major
    levels:
      explanation:
        state: partial
        evidence:
          - round: 1
            question: "Why does the handshake serve two purposes?"
            verdict: incomplete
            assistance: none
            format: open-ended
            supplied: "Initial sequence-number synchronization only"
          - round: 1
            question: "Retry after a cue about delayed requests"
            verdict: correct
            assistance: cue
            format: open-ended
            supplied: "Added protection against delayed duplicate requests"
        last_exposure_round: 1
        revisit_pending: true
        earliest_candidate_round: 4
```

Round 4 is eligible only if rounds 2 and 3 were completed questions about other concepts. Merely reaching round 4, sending three messages, or explaining two facts is insufficient. A later cue or explanation resets the spacing interval for the affected concept and level.

## Updating evidence

- First independent correct answer: `partial`, with one independent success.
- A different unassisted correct answer at the same level after the required intervening questions: `strong`, provided no intervening failure invalidates the earlier success.
- Correct answer after a cue: `partial`; do not use it as either of the two independent successes.
- An echo of a revealed answer: keep the prior state; no independent success.
- Substantive failure or incomplete answer: reset confirmation evidence for that level, set `weak` or `partial`, and queue another attempt.
- Ambiguous question, insufficient source, or skipped item: no negative evidence; preserve the prior state.
- A new response format may change what the evidence means. Label recognition-only results and reassess with open-ended questions before claiming free recall.

Keep enough recent question history to verify spacing and enough source detail to check grading. Preserve unsupported, unreadable, and untested areas in the final summary. Avoid turning unknown values into zero ability.

## Worked scheduling trace

| Round | Concept | Result | Next decision |
| --- | --- | --- | --- |
| 1 | A, explanation | Incorrect, then cued success: `partial` | Queue A; test a new concept. |
| 2 | B, recall | Independent success: `partial` | A is not eligible; test another concept. |
| 3 | C, recall | Independent success: `partial` | A is eligible for round 4. |
| 4 | A, explanation | Independent success: still `partial` | One independent success so far; choose new D. |
| 5 | D, recall | Independent success: `partial` | An eligible revisit may follow. |
| 6 | B, recall | Different unassisted answer: `strong` | A now has two intervening questions. |
| 7 | A, explanation | Different unassisted answer: `strong` | Two spaced independent successes at this level. |

This trace is a scheduling example, not a prescribed lesson. If time expires at round 5, finish with A pending and identify any untested material.

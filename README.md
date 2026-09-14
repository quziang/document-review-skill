# document-review-skill

An adaptive active-recall skill for reviewing documents.

Instead of turning a document into a static quiz, this skill reads the source, identifies important concepts, asks one question at a time, diagnoses the user's answers, and adapts the next question toward weak areas.

## Core idea

```text
Document
   ↓
Concept model
   ↓
Ask one question
   ↓
User answers
   ↓
Evaluate against source
   ↓
Diagnose gap / misconception
   ↓
Hint or explain
   ↓
Update mastery
   ↓
Choose next question
   ↺
```

The skill is designed around active recall rather than question generation.

## What makes it different from a quiz generator?

A quiz generator can decide all of its questions before the user answers anything.

This skill should not. The user's previous answer changes what happens next.

For example, if the user partially understands a concept, the skill may give a small hint, allow a retry, move on to other material, and later revisit the same concept through an application or comparison question.

## Error recovery

The default behavior for an incorrect answer is:

```text
wrong answer
    ↓
diagnose the error
    ↓
give a minimal hint
    ↓
allow a retry
    ↓
explain concisely if needed
    ↓
mark the concept weak/partial
    ↓
move on
    ↓
revisit later with a different question
```

Seeing the answer does **not** count as mastering it.

## Mastery model

During a session, concepts are tracked approximately as:

- `untested`
- `weak`
- `partial`
- `strong`

The model is intentionally simple. V1 does not pretend to know a precise retention probability or mastery percentage.

## Question styles

The skill can use:

- recall,
- explain-in-your-own-words,
- why / cause-and-effect,
- compare and contrast,
- application,
- cross-section connection questions.

It generally prefers open-ended retrieval over recognition-only multiple choice.

## Scope of V1

V1 focuses on a single adaptive review session.

It does not yet attempt to provide persistent spaced repetition across days, durable per-user mastery state, or cross-document knowledge tracking. Those are natural future extensions, but keeping them out of V1 makes the skill portable and easy to use in different agent environments.

## Repository

- [`SKILL.md`](./SKILL.md) — the skill behavior specification
- [`examples/basic-review.md`](./examples/basic-review.md) — a normal adaptive review flow
- [`examples/error-recovery.md`](./examples/error-recovery.md) — incorrect-answer recovery and later retesting
- [`examples/source-grounding.md`](./examples/source-grounding.md) — grading against the supplied document rather than outside knowledge

## Example

```text
User: Quiz me on this PDF.

Assistant: I've identified the main ideas. I'll ask one question at a time and adjust based on your answers.

Question 1: Why does the mechanism require X before Y?

User: Because X initializes the system.

Assistant: That's part of it. The document gives another reason related to stale state. Can you recall it?

User: I'm not sure.

Assistant: The second reason is that X prevents Y from operating on stale state. We'll come back to this concept later.
```

Later in the same session:

```text
Assistant: Suppose Y were allowed to run before X. What failure could occur?
```

That later question tests whether the concept can now be retrieved independently rather than merely recognized immediately after seeing the answer.

## Status

Early design / V1 skill specification.

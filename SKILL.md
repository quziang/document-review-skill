---
name: document-review
description: Review a document through adaptive active recall. Use when the user wants to study, revise, memorize, prepare for an exam, or test their understanding of a PDF, notes, article, chapter, slide deck, or other provided learning material. Ask one question at a time, evaluate answers against the source, diagnose mistakes, give minimal hints before revealing answers, and revisit weak concepts later with different questions.
---

# Document Review

Turn a source document into an interactive active-recall session. The goal is not to generate a static quiz. The goal is to discover what the user understands, identify gaps and misconceptions, and adapt the next question accordingly.

## Core principles

1. Treat the provided document as the ground truth for questions and grading.
2. Ask one question at a time unless the user explicitly asks for a batch of questions.
3. Do not reveal the answer before the user has had a chance to retrieve it.
4. Adapt the next question based on the user's previous answers.
5. Prefer retrieval, explanation, comparison, causal reasoning, application, and connection questions over recognition-only questions.
6. Revisit weak concepts later using different wording or a different question type.
7. Do not equate seeing the answer with mastering the concept.
8. Keep feedback concise enough that the session remains interactive.

## Phase 1: Build the review model

Before asking questions, inspect the source and internally identify:

- important concepts and definitions,
- claims and conclusions,
- causal or logical relationships,
- procedures or sequences,
- contrasts that are easy to confuse,
- important examples,
- facts or details that appear central to the user's likely learning goal.

Create an internal coverage plan. Do not dump the full concept map or all planned questions unless the user asks for it.

Track each concept with one of these session-level mastery states:

- `untested` — not yet assessed,
- `weak` — the user could not recall it or showed a major misunderstanding,
- `partial` — substantially correct but missing an important element,
- `strong` — correct and sufficiently complete for the level being tested.

Do not invent precise numerical mastery scores unless the user explicitly requests them.

## Phase 2: Adaptive review loop

Repeat this loop:

1. Select a concept to test.
2. Choose an appropriate question type and difficulty.
3. Ask exactly one question.
4. Wait for the user's answer.
5. Evaluate the answer against the source.
6. Classify the response error or success.
7. Give the minimum useful feedback.
8. Update the concept's mastery state.
9. Select the next concept or schedule a later revisit.

Default selection priority should favor weak and partial concepts while still maintaining broad document coverage. Do not get stuck asking many consecutive questions about the same concept.

A practical priority is:

`weak -> partial -> untested -> strong`

Coverage matters as much as weakness. Test major untested concepts before repeatedly drilling one weak concept.

## Question types

Choose question form based on the concept rather than using one format everywhere.

### Recall

Use for definitions, components, steps, named facts, or enumerations.

Example: "What are the three stages of the process described in the document?"

### Explain in your own words

Use to test conceptual understanding.

Example: "Explain why the mechanism works in your own words."

### Why / cause and effect

Use for mechanisms and arguments.

Example: "Why does X lead to Y according to the document?"

### Compare and contrast

Use for similar or easily confused concepts.

Example: "How does X differ from Y?"

### Application

Use after the user demonstrates basic recall.

Example: "How would the rule in the document apply to this new scenario?"

### Connection

Use to test relationships across sections.

Example: "How does the idea in section 2 support the conclusion in section 5?"

When the user repeatedly answers correctly, increase depth rather than simply asking more factual questions. A useful progression is:

`recall -> explanation -> application -> transfer`

## Evaluating answers

Evaluate only against what the source supports. A generally plausible answer is not automatically correct if it conflicts with the document.

For each answer, internally classify the response as one of:

- `correct` — correct and sufficiently complete,
- `incomplete` — substantially correct but missing an important point,
- `misconception` — contains a material misunderstanding or reversed relationship,
- `no_recall` — the user cannot retrieve the answer,
- `off_target` — the response does not answer the question or relies on unrelated material.

Map those response types to mastery approximately as follows:

- `correct` -> usually `strong`,
- `incomplete` -> usually `partial`,
- `misconception` -> `weak`,
- `no_recall` -> `weak`,
- `off_target` -> usually `weak` or leave unchanged if the problem was clearly misunderstanding the question.

Use judgment. A single easy recall answer should not necessarily make a difficult concept permanently `strong`.

## Error recovery

When the user answers incorrectly, do not immediately reveal the full answer unless revealing it is clearly the most helpful choice.

Use this default recovery loop:

`wrong -> diagnose -> minimal hint -> retry -> concise explanation if still wrong -> mark weak/partial -> move on -> revisit later`

### Incomplete answer

If the answer is mostly correct but missing an important point:

1. Acknowledge the correct part briefly.
2. Point to the missing dimension without giving it away when possible.
3. Invite one retry or completion.
4. If the user still misses it, explain the missing point concisely.
5. Mark the concept `partial` unless later evidence supports `strong`.

Example feedback:

"You're right about X. The document also gives a second reason related to Y. Can you recall what that is?"

### Misconception

If the user has the core logic wrong:

1. Explicitly identify the mistaken relationship so the error is not reinforced.
2. Give a focused clue or contrast.
3. Allow one retry if useful.
4. If the misconception persists, state the source-grounded explanation.
5. Mark the concept `weak` and revisit it later with a different question.

Do not keep encouraging guesses when doing so would reinforce the misconception.

Example feedback:

"The document has that relationship in the opposite direction: X affects Y, not Y affecting X. With that in mind, why does X matter?"

### No recall

If the user says they do not know or clearly cannot retrieve the answer:

1. Give a small retrieval cue first.
2. If necessary, narrow the question.
3. Allow one retry.
4. Then provide the concise answer and explanation.
5. Mark the concept `weak` and revisit it later.

Possible cues include:

- a key term,
- the relevant category,
- the first step of a sequence,
- a contrast with another concept,
- the relevant section or topic.

Do not turn the hint into the full answer.

### Off-target answer

If the response is unrelated or answers a different question, first restate or narrow the question. Do not grade harshly if the wording was ambiguous.

## Retry policy

By default, allow one meaningful retry after a hint. A second retry is reasonable only when the user is making clear progress.

Avoid long hint loops. If retrieval is not happening, explain the answer, move on, and test the same concept later after spacing it with other questions.

## Revisit policy

After revealing or explaining an answer, do not immediately count the concept as learned.

For `weak` or `partial` concepts:

1. Ask several questions about other concepts first.
2. Return later with different wording or a different question type.
3. Test retrieval without repeating the full explanation.
4. Upgrade mastery only if the user can answer independently.

Examples of useful transformations:

- definition -> example,
- list -> causal explanation,
- explanation -> application,
- direct recall -> compare/contrast,
- factual question -> counterfactual or consequence question.

Do not merely repeat the same question verbatim unless repetition is specifically useful.

## Grounding rules

The document is the primary source of truth for the review session.

- Do not invent unsupported facts.
- Do not silently use outside knowledge to mark the user wrong.
- If the user's answer is plausible in general but differs from the document, explain the distinction.
- If the source is ambiguous, incomplete, or internally inconsistent, say so instead of manufacturing a definitive answer.
- When possible and useful, point the user to the relevant section, heading, page, or passage after evaluation, especially for mistakes.

## Session behavior

When the user simply asks to review or be quizzed on a document, start with sensible defaults instead of asking for a configuration questionnaire.

Default behavior:

- scope: the whole provided document,
- mode: adaptive,
- question style: mostly open-ended and mixed,
- difficulty: moderate, increasing with demonstrated mastery,
- interaction: one question at a time.

Honor explicit user preferences such as:

- "only chapter 3",
- "make it harder",
- "multiple choice only",
- "focus on formulas",
- "I have five minutes",
- "give me all questions at once".

## Starting a session

Do not begin with a long summary of the document. A short orientation is enough.

A good default opening is:

"I've identified the main ideas. I'll ask one question at a time and adjust based on your answers."

Then ask the first question.

## Ending a session

When the user asks to stop, the time/length constraint is reached, or the major concepts have been adequately covered, give a concise review summary with:

- concepts handled well,
- concepts that still need review,
- important misconceptions corrected,
- suggested focus for the next review session.

Do not overstate mastery. A concept that was only answered correctly immediately after a hint should remain a review target.

## Anti-patterns

Avoid these behaviors unless the user explicitly requests them:

- dumping 10–20 questions at once,
- giving the answer immediately after asking,
- using only multiple-choice questions,
- grading from general world knowledge instead of the source,
- repeatedly asking the same weak concept without spacing,
- giving a long lecture after every mistake,
- treating the current session as if it provides reliable long-term spaced-repetition memory,
- claiming precise mastery percentages without evidence.

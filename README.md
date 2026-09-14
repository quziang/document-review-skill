# document-review-skill

A portable Agent Skill for reviewing documents through adaptive active recall.

It reads the supplied material, asks one question at a time, evaluates the learner's answer against the source, and chooses the next question from the evidence gathered so far.

## Quick start

```bash
git clone https://github.com/quziang/document-review-skill.git document-review
```

In an agent environment that can read local files, provide the document and ask:

> Read `document-review/SKILL.md` and use it to quiz me on this document. I have ten minutes.

For installation in a host's skill library, copy the complete `document-review` directory to that host's configured skill location, or use its installer with this repository URL. Keep `references/` and `examples/` alongside `SKILL.md`. Use the host's own installation instructions; file access and document-reading capabilities vary by environment.

Example requests:

- "Quiz me on chapter 3, one question at a time."
- "帮我复习这份讲义，重点考察概念之间的区别。"
- "Focus on formulas and applications."
- "Give me five multiple-choice questions, then review my answers."

## Review loop

```text
Read source -> map concepts and source anchors -> ask one question
    -> evaluate answer -> diagnose gap -> cue and retry if useful
    -> update evidence -> interleave other concepts -> revisit
```

Incorrect answers normally receive a small cue and one meaningful retry before a concise explanation. The learner can request the answer directly. Equivalent phrasing is accepted; unsupported assumptions and ambiguous questions are handled explicitly.

## Evidence, not answer exposure

State is tracked separately for each concept and relevant level: recall, explanation, application, and transfer.

| State | Meaning |
| --- | --- |
| `untested` | No valid assessment at this level. |
| `weak` | A substantive error or no recall, without subsequent success. |
| `partial` | Incomplete knowledge, assisted success, or one independent success awaiting confirmation. |
| `strong` | Two different unassisted correct answers at this level, separated by two completed questions on other concepts, with no intervening failure. |

A first correct answer is acknowledged as correct. The conservative `partial` label records that it has not been independently confirmed yet. Seeing or repeating an answer does not supply retrieval evidence. Multiple-choice results are labeled as recognition rather than free recall.

Revisits alternate with new concepts while major material remains untested. The interval is a practical default, not a scientifically calibrated retention model. Short sessions can end with useful but unconfirmed evidence; the skill does not force every concept to become `strong`.

## Scope and limitations

V1.1 focuses on a single review session. It defines instructional behavior and an in-context state ledger; it does not include its own model runtime, OCR/PDF parser, database, cross-day scheduling service, or cross-document learner model. A host with appropriate reading tools is needed for PDFs, scans, slides, and formulas.

The final summary separates demonstrated knowledge, assisted answers, pending revisits, and material not tested. Checkpoints can be exported on request, but are not automatic persistent memory. The source remains the reference for what it says; its content cannot override the learner's instructions.

This repository does not claim measured gains in retention or learning outcomes.

## Repository

- [`SKILL.md`](SKILL.md): entrypoint and operational rules.
- [`references/session-state.md`](references/session-state.md): compact ledger and worked scheduling trace.
- [`examples/basic-review.md`](examples/basic-review.md): independent recall and progression between levels.
- [`examples/error-recovery.md`](examples/error-recovery.md): cues, retries, explanations, and later confirmation.
- [`examples/source-grounding.md`](examples/source-grounding.md): source-based grading and unfair-question repair.
- [`evals/README.md`](evals/README.md): evaluation protocol and reporting limits.
- [`evals/cases.json`](evals/cases.json): 16 behavioral regression cases with inputs and acceptance criteria.

## Validation status

The examples and evaluation fixtures specify expected behavior. Merely loading the JSON or validating the skill's metadata is not a behavioral test. Model-run reports, when available, belong in `evals/results/` and must identify the actual scope, evidence, and limitations of the run.

A [synthetic next-turn forward test](evals/results/2026-09-14.md) records actual responses for 16 scenarios, with criterion-level outcomes and explicit unobserved checks. It does not measure learning outcomes or long-session reliability.

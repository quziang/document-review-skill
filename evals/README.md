# Behavioral evaluation

`cases.json` contains 16 regression cases. Each includes source material, context, conversation messages, and acceptance criteria. These cases exercise observable decisions rather than exact wording.

## Run a case

1. Start a fresh session in the agent host being tested. Load `SKILL.md` and the references it requests.
2. Give the assistant only the case's `input` object, preserving the message roles. Treat `session_context` as a factual checkpoint for the test. Do not include the `expected` object or this case's title in the tested assistant's prompt.
3. Capture the assistant's next learner-facing response. Continue the interaction when the case calls for a retry or later revisit.
4. After the response, request a compact state checkpoint for evaluation. The checkpoint is an explicit test request; it should not be shown by default during normal tutoring. Do not request private chain-of-thought.
5. Compare the response and observable checkpoint against every `expected` criterion. Record `pass`, `fail`, or `not_observed` with the actual supporting text. Do not count an unobserved criterion as a pass.

Do not ask one model to invent both sides of a supposedly live transcript and report that as an independent run. A simulated learner is acceptable when clearly identified; preserve its actual messages.

## Multi-turn checks

The scheduling case has a checkpoint for a reproducible next-step test. Also run a live session with several source concepts and deliberately vary learner behavior: incomplete answer, failed retry, correct paraphrase, and a stop request. Check whether pending revisits and assistance survive the intervening turns.

## Report

Record the date, skill commit/content snapshot, host and model when known, case IDs, actual prompts/responses, observed state, and criterion-level outcomes. Separate:

- package checks, such as valid frontmatter and existing relative links;
- behavioral observations from actual model responses;
- learning outcomes, which require a separate learner study and are not measured here.

A partial run should list what was not tested. Small synthetic cases do not establish reliability on long PDFs, equations, scanned documents, different models, or real learners.

Keep API credentials, learner identities, and private source documents out of public reports. The included fixtures use synthetic material.

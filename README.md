# QA Principles

## Keep Everybody Aware of Work in Development

- Developers share continuously what they’re working on, including any expected changes or risks to be managed.
- This is not daily reporting; it should be done at a cadence that is useful for everyone.
- Pull Requests (PRs) should be shared with the entire team. The team should decide on standards for PR documentation (e.g., adding screenshots).
- Disclose AI-assisted code, so reviewers know to look harder at logic boundaries, error paths, and anything the model might have hallucinated.

## Units of Work Are Well Defined

- **Acceptance Criteria (AC) should be explicit**, including how the work will be tested (e.g., edge cases). AI test-generation and code-generation tools take AC as their primary input. Vague AC produces vague tests and vague code, at scale.
- Stories should not be added to the iteration without proper AC.
- If more information is needed, a proper spike that tracks the investigation should be done and documented separately from the story.
- If a story requires more investigation during the iteration (e.g., clarification from stakeholders), it should be removed from the iteration and the investigation tracked in a separate spike.

## Shift Left

- **QA starts at the beginning.** Encourage pair programming, enforce unit test coverage, and best PR practices.
- Apply the test pyramid: as feature work is done, create as many useful unit, integration, and contract tests as needed.
- Any future PR should run against these small tests to check for regressions.
- As more code is AI-generated, human review of that code becomes more valuable, not less.
- AI-assisted code reviewers should be treated as a first-pass static analysis layer, not as a substitute for human review.
- AI-generated tests are reviewed for whether they actually exercise meaningful behavior, not just for whether they pass.

## Shift Right

- **Canary deployments.** Validate new code in production with a subset of users, then upgrade the rest.
- Keep metrics and alerts for services in production to find and correct bugs before users are affected.
- When a bug is found, add at least one new test case that covers it to the automated suite. Bad LLM outputs found in production are added to the eval set.
- If any part of the product uses an LLM, observability is not optional. A model upgrade can degrade quality without changing a single line of application code.

## E2E Automation

- For end-to-end (E2E) testing, automate critical user flows and flows not covered by lower levels of the testing pyramid.
- Launch the entire E2E test suite against a staging environment every night. Ensure the test report is available for every stakeholder (e.g., automatically publish results in a dedicated Slack channel).
- Flaky tests introduce noise; fix the test or application, or skip the test. If there's a skip, ensure there's documentation and stakeholders are informed.
- Self-healing E2E tools can meaningfully reduce flakiness, but it's a maintenance reducer, not a free pass on diagnosis.

## Manual Testing

- **Do not rely on manual testing for final validation.** Closing a story means automated tests have been added to the repo.
- Use manual testing to champion the final users (e.g., ensure the UX makes sense).
- If the product uses LLMs, you need humans deliberately trying to break them with adversarial inputs, edge prompts, jailbreaks, and bias probes.

## If your application uses an LLM

- Maintain an evaluation dataset of inputs and expected behaviors (e.g. factuality, format, refusal behavior, tone). Run this against prompt changes, model version changes and provider upgrades. 
- Prompt versions are code. They can be reviewed, versioned and rolled back.
- Test for hallucination, prompt injection, PII leakage, and bias as first-class concerns.

## Documentation

- Test plan for each feature should be part of its AC. This documentation should be centralized and stakeholders (e.g., Product or Customer Support) should have easy access to it.
- Test suites should also be documented. Make it easy for new members to add or change tests.
- Bug found: document (e.g., steps, environment, screenshots) in a story. If found during new feature testing, link it to original story. Reach out to relevant stakeholders.


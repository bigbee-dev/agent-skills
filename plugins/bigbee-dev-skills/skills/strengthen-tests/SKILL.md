---
name: strengthen-tests
description: Create, review, and improve automated tests that exercise intended production behavior and detect meaningful regressions. Use whenever Codex writes, modifies, reviews, or hardens test code, especially for tests with excessive mocking, weak or interaction-only assertions, duplicated production logic, implementation coupling, or paths that bypass the target code.
---

# Strengthen Tests

Improve the defect-detection power of tests while preserving appropriate speed, isolation, determinism, and scope.

## Core Invariant

A valuable test fails for the right reason when a realistic defect is introduced in the targeted production behavior.

Do not optimize for mock count, assertion count, coverage percentage, or integration depth alone.

## Choose the Operating Mode

Use an authoring quality gate when tests are being added or modified as part of ordinary implementation:

1. Identify the behavior the test must prove.
2. Confirm the real target code executes.
3. Assert an observable outcome.
4. Keep only justified test doubles.
5. Run the narrowest meaningful verification.

Use the full review-and-improvement workflow when the user explicitly asks to audit, strengthen, review, or improve tests.

Remain read-only when the user asks for an audit, diagnosis, or report without changes. Otherwise, improve verified weaknesses within scope.

## Establish Scope

Before judging a test, identify:

- the requested behavior and explicit boundaries
- the target production function, class, module, endpoint, or workflow
- the intended test level: unit, integration, contract, or end-to-end
- the relevant test files, production files, and verification commands
- the repository's existing test conventions
- external systems and nondeterministic dependencies

Trace the actual production path. Do not assess test quality from the test file alone, and do not broaden into unrelated tests or production behavior.

## Define the Behavioral Contract

For each target, establish:

- inputs and relevant preconditions
- observable result or side effect
- meaningful failure and boundary cases
- production path expected to execute
- realistic regression the test should detect

Prefer observable outcomes such as returned values, errors, state transitions, persisted records, emitted domain events, externally visible responses, or permitted and prevented side effects.

## Choose an Appropriate Test Boundary

Use the narrowest test level that can prove the behavior confidently:

- Unit tests execute the real target unit and keep deterministic, inexpensive collaborators real when they are part of that unit.
- Integration tests exercise meaningful collaboration between internal components.
- Contract tests verify serialization, protocol, schema, or compatibility behavior at a boundary.
- End-to-end tests exercise an externally visible flow without replacing its important internal behavior.

Do not turn every unit test into an integration test. Do not label a test as integration coverage when the important integration is mocked out.

## Use Test Doubles Deliberately

Never mock or stub the behavior under test.

Use a mock, stub, fake, or controlled dependency when it represents a deliberate boundary, including:

- an external network service
- a clock, random source, scheduler, or other nondeterministic input
- an unsafe or irreversible side effect
- unavailable or prohibitively slow infrastructure
- a failure that cannot be produced safely or deterministically
- a collaborator intentionally outside the chosen unit boundary

Prefer a fake or in-memory implementation when the dependency's state or behavior matters. Prefer a mock or spy when the interaction itself is the contract or when controlled failure injection is required.

Interaction assertions are appropriate for contracts such as sending exactly one message, avoiding a charge after validation fails, respecting a retry limit, or committing versus rolling back a transaction. Do not use call-count assertions as a substitute for verifying business behavior.

Avoid deep mock chains, mocked value objects, and test setup that reconstructs production implementation details. Do not classify a retained test double as a weakness merely because it exists.

## Review for Weak Tests

Look for verified cases where:

- the claimed target production code never executes
- the behavior under test is replaced by a test double
- assertions only confirm mocked calls or lack meaningful specificity
- the expected result duplicates the production algorithm
- private methods, incidental call order, or internal structure are treated as the contract
- fixtures cannot expose the regression the test claims to prevent
- broad snapshots replace focused behavioral assertions
- happy-path coverage omits a material error, boundary, or state transition
- excessive setup hides the purpose of the test
- nondeterminism, shared state, timing, or ordering makes the test unreliable
- a meaningful production behavior could be removed while the test still passes

Verify every finding against the intended contract and production path. Reject speculative, purely stylistic, or over-broad findings.

Classify accepted findings as:

- `hollow-test`: does not execute the claimed behavior
- `weak-oracle`: executes behavior but cannot distinguish an important defect
- `over-mocked`: replaces behavior that belongs inside the chosen test boundary
- `implementation-coupled`: asserts incidental internals rather than the contract
- `missing-case`: omits a material failure, boundary, or state transition
- `testability-problem`: meaningful testing requires a production design decision
- `legitimate-boundary`: the test double is appropriate and should remain
- `follow-up`: valid concern outside the current scope

## Improve Tests

For each accepted in-scope weakness:

1. Preserve the intended behavioral contract.
2. Make the real target production path execute.
3. Replace implementation assertions with observable outcomes.
4. Remove, narrow, or replace harmful test doubles.
5. Retain legitimate boundary controls and state why.
6. Add material negative or edge cases when justified.
7. Simplify setup when it obscures intent.
8. Keep the test deterministic and reasonably fast.
9. Follow repository conventions unless they perpetuate the weakness being fixed.
10. Run relevant verification and re-review the changed test.

Do not delete valuable coverage merely because its current form is weak. Replace it with a stronger test when practical.

Do not change production behavior merely to make a test easier to write. Report and escalate `testability-problem` findings that require broader design changes.

## Check Regression Sensitivity

For every new or improved test, identify at least one realistic defect it should catch, such as removed validation, an incorrect calculation, missing persistence, an improper partial commit, a duplicate side effect, an authorization bypass, incorrect error mapping, or a mishandled boundary value.

Use an existing mutation-testing tool when it is already configured and proportionate to the task. Do not manually corrupt production files to prove sensitivity unless the user explicitly authorizes that method.

Treat coverage as supporting evidence only. Coverage does not prove that assertions are meaningful.

## Verify and Converge

Run the narrowest relevant test command first. Run broader verification when changes affect shared fixtures, test infrastructure, common helpers, persistence setup, public contracts, or cross-module behavior.

After verification, confirm that:

- the intended production path executes
- assertions observe the intended contract
- test doubles remain only at justified boundaries
- each test can catch its stated regression
- no unnecessary slowness or nondeterminism was introduced

Re-review after changes. Stop when no verified, in-scope test-quality weaknesses remain. If two improvement rounds do not converge, pause and reclassify the remaining findings before editing again.

If verification cannot run, report the exact reason without claiming success.

## Report the Result

For an ordinary authoring quality gate, keep the closeout brief. Include:

- behavior tested
- important production path exercised
- significant test doubles retained and why
- verification commands and results

For an explicit audit or strengthening run, include:

- mode: audit-only or improved
- target behavior and production path
- weaknesses found and improvements made
- test doubles removed, replaced, and retained with reasons
- behavior-to-test evidence:

| Behavior | Production path exercised | Observable assertion | Regression detected |
|---|---|---|---|

- verification commands and results
- remaining gaps, follow-ups, or testability problems
- status: strengthened, already strong, stopped by scope, stopped by design, or verification unavailable

Do not provide a numerical test-quality score unless the user explicitly asks for one.

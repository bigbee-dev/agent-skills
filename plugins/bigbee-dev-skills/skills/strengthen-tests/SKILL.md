---
name: strengthen-tests
description: "Review, create, and improve automated tests that exercise real production behavior and detect meaningful regressions. Use when test quality is the primary objective: auditing existing tests, replacing weak or over-mocked tests, adding missing behavioral coverage, or investigating why tests missed a defect."
---

# Strengthen Tests

Improve the defect-detection power of tests while preserving appropriate speed, isolation, determinism, and scope.

## Core Invariant

A valuable test fails for the right reason when a realistic defect is introduced in the targeted production behavior.

Judge test value by defect-detection power rather than mock count, assertion count, coverage percentage, or integration depth alone.

## Choose the Operating Mode

- `audit-only`: inspect and report when the user asks for an assessment, diagnosis, or review without changes
- `improve`: repair verified weaknesses when the user asks to strengthen or rework existing tests
- `author`: create tests when adding missing behavioral coverage is the primary deliverable

Choose one mode before editing. Preserve the scope and sequencing of any broader task. Keep this workflow focused on test quality while the surrounding task retains ownership of production implementation, root-cause analysis, and product decisions.

In `audit-only` mode, remain read-only. For standalone `improve` and `author` requests, change only the tests and test support code needed for the requested coverage. When this skill supports a broader implementation request, continue production work already authorized by that request without asking again. Ask only for an unresolved decision that materially changes the agreed scope or behavior; the skill itself grants no additional authorization.

## Establish Scope

Before judging a test, identify:

- the requested behavior and explicit boundaries
- the target production function, class, module, endpoint, or workflow
- the intended test level: unit, integration, contract, or end-to-end
- the relevant test files, production files, and verification commands
- the repository's existing test conventions
- external systems and nondeterministic dependencies

Trace the actual production path rather than assessing test quality from the test file alone. Keep the analysis within the requested behavior and related tests.

## Define the Behavioral Contract

For each target, establish:

- inputs and relevant preconditions
- observable result or side effect
- meaningful failure and boundary cases
- production path expected to execute
- realistic regression the test should detect

Prefer observable outcomes such as returned values, errors, state transitions, persisted records, emitted domain events, externally visible responses, or permitted and prevented side effects.

## Choose a Correct Test Boundary

Use the most stable observable boundary that proves the behavior and exercises the relevant production path. Prefer a public interface that captures the real contract. Move inward only when a narrower boundary is itself a meaningful contract and materially improves determinism, speed, or failure precision without coupling the test to implementation details.

- Unit tests execute the real target unit and keep deterministic, inexpensive collaborators real when they are part of that unit.
- Integration tests exercise meaningful collaboration between internal components.
- Contract tests verify serialization, protocol, schema, or compatibility behavior at a boundary.
- End-to-end tests exercise an externally visible flow without replacing its important internal behavior.

Keep the test-level label honest about which collaboration actually executes.

When no boundary can exercise the real behavior meaningfully, classify the issue as a `testability-problem` and identify the missing seam. Prefer that explicit gap over a proxy test that creates false confidence. Resolve the seam when the surrounding request already authorizes the production change and the requirements determine it. Otherwise report the missing decision and continue independent in-scope work.

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

## Author or Improve Tests

In `author` mode, add the smallest coherent set of tests that proves the requested behavior at the correct boundary. Cover material failure, boundary, or state-transition cases when they are part of the requested contract.

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

Preserve valuable coverage by replacing weak forms with stronger tests when practical.

Preserve the behavior being tested during test-quality changes. A broader implementation request may separately authorize changing that behavior. Report `testability-problem` findings that still require a decision outside the existing authorization.

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

Re-review the requested test-quality scope after changes. Stop when no verified, in-scope weakness remains. If two improvement rounds do not converge, pause and reclassify the remaining findings before editing again.

If verification cannot run, report the exact reason without claiming success.

## Report the Result

Include:

- mode: audit-only, improved, or authored
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

---
name: review-fix-refactor-loop
description: Use when Codex should iteratively review, fix, verify, refactor, and re-review code until convergence. Triggers include review-fix loop, review then fix, refactor review, iterative cleanup, review/fix/refactor cycle, final hardening, or requests to keep reviewing and improving until no in-scope issues remain.
---

# Review Fix Refactor Loop

Run a disciplined convergence loop:

1. Correctness first.
2. Refactor only after correctness is clean.
3. After refactoring, return to correctness review.
4. Stop when both correctness and refactor reviews are clean within scope.

## Core Invariant

Do not begin refactoring until the correctness loop is clean.

After any refactor, run correctness review again before continuing.

## Scope Baseline

Before reviewing, establish:

- user request and explicit boundaries
- current git status
- changed files and intended behavior
- relevant tests or verification commands
- owner, module, API, or product boundaries

Do not broaden scope without user approval.

## Finding Classification

Before fixing any finding, classify it:

- `correctness-blocker`: real bug, regression, broken contract, unsafe behavior, missing required test, or issue blocking the current task
- `refactor-candidate`: behavior-preserving improvement that reduces complexity, duplication, brittle boundaries, unclear naming, or poor testability
- `follow-up`: real issue, but adjacent, broader, subjective, or outside the current task boundary
- `stop-and-escalate`: requires product, API, schema, protocol, storage, ownership, or release-process decision

Fix only `correctness-blocker` findings during the correctness loop.

Apply only material, in-scope `refactor-candidate` findings during the refactor loop.

Report `follow-up` and `stop-and-escalate` findings instead of silently expanding scope.

## Correctness Review

Review for:

- bugs and regressions
- broken contracts or mismatched behavior
- missing edge cases
- unsafe state, concurrency, or async behavior
- data loss or migration risk
- auth, permission, privacy, or security regressions
- error handling gaps
- missing focused tests for risky behavior

Verify each finding against the actual code path and adjacent files before fixing.

Reject speculative, unrealistic, or over-broad findings.

## Correctness Loop

Repeat:

1. Review for correctness only.
2. Classify and verify findings.
3. Fix accepted in-scope correctness blockers.
4. Add or adjust focused tests when justified.
5. Run relevant verification.
6. Review correctness again.

Stop when no verified, in-scope correctness blockers remain.

If the same bug class appears in multiple sibling places within scope, inspect and fix the scoped class together.

## Refactor Review

Review for:

- duplicated logic
- unclear ownership boundaries
- excessive coupling
- confusing names or control flow
- brittle abstractions
- repeated test setup
- poor testability
- dead or unnecessary code
- structure that makes future bugs likely

Reject cosmetic churn and style-only cleanup unless it directly improves clarity around the touched behavior.

## Refactor Loop

Start only after the correctness loop is clean.

Repeat:

1. Review for refactor opportunities only.
2. Classify findings.
3. Refactor accepted in-scope candidates.
4. Preserve behavior.
5. Run relevant verification.
6. Review refactor opportunities again.

Stop when no material, in-scope refactor candidates remain.

## Post-Refactor Return

After any refactor, return to the correctness loop.

If correctness fixes after refactoring meaningfully change structure, run refactor review again.

## Non-Convergence Guard

If two consecutive correction cycles do not converge, pause and reclassify all remaining findings before editing again.

Continue only when every remaining finding is still an in-scope correctness blocker.

Default to at most three full correctness/refactor cycles unless the user explicitly asks for exhaustive iteration.

## Verification

Run the narrowest meaningful verification first.

Use broader verification when the change touches shared behavior, cross-module contracts, generated code, migrations, public APIs, auth, payments, data loss risk, or release surfaces.

If verification cannot be run, report why.

## Final Report

Include:

- correctness issues found and fixed
- refactors performed
- verification run
- accepted and rejected findings, briefly why
- follow-ups or stop-and-escalate items
- loop status: clean, stopped by scope, stopped by risk, or verification unavailable

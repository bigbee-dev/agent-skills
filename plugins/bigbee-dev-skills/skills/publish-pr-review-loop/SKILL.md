---
name: publish-pr-review-loop
description: Publish a GitHub pull request and run a Codex review/fix/re-review loop until the current head has no verified findings worth fixing. Use when the user asks to create, open, publish, or ship a PR with Codex review follow-up, asks for a PR review loop, or wants Codex feedback addressed repeatedly before handoff.
---

# Publish PR Review Loop

Publish the scoped change as a ready-for-review PR by default, then iterate on
Codex feedback until the latest review pass on the current head has no accepted
findings.

## Core Invariants

- Create a new PR as ready for review unless the user explicitly requests a
  draft.
- Never pass a draft flag merely because the work was agent-generated.
- Treat the eye reaction as acknowledgment, not review completion.
- Verify every finding against the current code before changing anything.
- Expect automatic Codex review only when a PR is first opened.
- After every push that changes the head of an existing PR, immediately request
  review by commenting `@codex review it`.
- Do not merge the PR unless the user separately asks for a merge.

## Establish Scope

Before publishing:

- inspect the user request, git status, branch, remotes, and branch diff
- preserve unrelated user changes
- identify the intended base branch and relevant verification commands
- confirm the branch is publishable and is not the protected default branch
- check whether an open PR already exists for the branch

Reuse an existing open PR instead of creating a duplicate. If the existing PR is
closed, merged, or targets the wrong base, stop and report the conflict unless
the correct action is unambiguous.

## Prepare the Change

1. Review the scoped diff for accidental files, secrets, generated noise, and
   obvious incomplete work.
2. Run the narrowest meaningful verification.
3. Stage only intended files.
4. Create a concise conventional commit when uncommitted changes remain.
5. Push the branch and set its upstream when needed.

Do not claim the PR is ready when required verification failed. Fix in-scope
failures or report the blocker.

## Create or Update the PR

For a new PR:

1. Derive a clear title and body from the actual diff.
2. Include purpose, important behavior changes, and verification.
3. Create the PR in the open, ready-for-review state.
4. Include a draft option only when the user explicitly asked for a draft.

For an existing open PR, update stale title or body content only when needed to
describe the current change accurately. Do not overwrite useful user-authored
context.

Record:

- PR URL and number
- current head SHA
- PR creation or review-request time
- existing Codex reactions, comments, reviews, and inline threads

Use these values as the baseline for distinguishing new feedback from stale
feedback.

## Wait for the Initial Codex Review

After creating the PR, allow the repository's automatic Codex integration to
start first. This automatic behavior applies only to opening the PR, not to
later pushes.

1. Monitor the PR for a verified Codex bot or app reaction, comment, review, or
   inline thread.
2. Poll no more aggressively than every 30 seconds; prefer the available
   recurring wait or monitoring mechanism.
3. If Codex has neither acknowledged nor reviewed the PR after about five
   minutes, post this comment:

   `@codex review it`

4. Continue monitoring after acknowledgment. A `👀` reaction means Codex has
   noticed the request; wait for the actual review output.

The five-minute threshold triggers the initial explicit request; it is not a
review-completion timeout.

Do not identify Codex only by a display name when bot or app identity is
available. Do not spam duplicate review requests while a request is already
acknowledged or pending.

## Define a Review Pass

Tie every pass to:

- the head SHA being reviewed
- the time the review was requested or automatically started
- Codex feedback posted after that baseline

Inspect all Codex review surfaces:

- top-level PR comments
- submitted review bodies
- inline review comments and threads
- reactions on the PR or latest review-request comment

Consider older unresolved feedback only when it still applies to the current
head. Do not treat stale comments as new findings.

## Classify Findings

Classify every actionable Codex item before editing:

- `accept`: verified, in scope, and materially improves correctness, safety,
  maintainability, or required test coverage
- `reject`: inaccurate, already resolved, speculative, purely subjective,
  disproportionate, or outside the requested scope
- `escalate`: requires a product, architecture, security, schema, release, or
  ownership decision that should not be assumed

For accepted findings:

1. Reproduce or trace the issue in the current code.
2. Fix the root cause with the narrowest coherent change.
3. Add or update focused tests when justified.
4. Run relevant verification.
5. Re-read the finding against the resulting diff.

For rejected findings, retain a concise rationale. Reply on the PR when the
rationale would help reviewers understand why no change was made.

For escalated findings, pause the loop and ask the user for the missing decision.
Do not describe an escalated pass as clean.

## Push and Request Re-review

When at least one finding is accepted:

1. Group the fixes into a coherent commit.
2. Push the commit.
3. Record the new head SHA.
4. Immediately post a new PR comment:

   `@codex review it`

5. Resolve accepted inline threads only after the fix is pushed and verified,
   when the platform and repository conventions support resolution.
6. Monitor the next pass against the new head SHA.

Repeat classification, fixes, verification, commit, push, and re-review for each
new pass.

Codex does not automatically re-review a PR when new commits are pushed. Do not
wait for an automatic reaction after a push. Every successful push that changes
the head of an already-open PR requires exactly one new `@codex review it`
comment, including pushes made for reasons other than review feedback.

## Convergence and Stop Conditions

Finish successfully only when the latest Codex pass for the current head has:

- no new verified findings classified as `accept`
- no unresolved `escalate` finding
- no accepted finding left uncommitted or unpushed

Codex does not need to use an explicit "no findings" phrase. A completed pass
with no actionable accepted items is clean.

If the latest pass contains only rejected findings, stop and report those
findings with brief rationales.

If two consecutive passes repeat the same already-fixed or rejected point,
re-check the current head and review baseline before making more changes. Avoid
churn and do not implement a finding solely to silence the reviewer.

If Codex remains unavailable after acknowledgment and reasonable monitoring,
leave at most one pending review request for the current head and report the
review as pending rather than claiming convergence.

## Final Report

Include:

- PR link and ready/draft state
- commits pushed during the loop
- accepted findings and fixes
- rejected findings and rationales
- verification commands and results
- final head SHA
- loop status: clean, pending Codex review, stopped for decision, or blocked

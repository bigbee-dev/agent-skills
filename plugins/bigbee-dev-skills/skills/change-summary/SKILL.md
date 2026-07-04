---
name: change-summary
description: Use when summarizing local code, config, docs, or UI changes for review, especially after agent-generated changes, manual tweaks, branch diffs, PR diffs, or "what changed?" requests. Produces a grouped table-based review packet focused on files touched, public surfaces, behavior changes, UI-visible impact, risks, and verification instead of line-by-line diff explanation.
---

# Change Summary

Inspect the actual diff, working tree, commit range, PR diff, or provided patch before summarizing. Do not summarize from memory.

## Output Format

Use this structure by default:

1. **Change Summary**
2. **High-Level**
3. **File Summary**
4. **Contracts / Behavior**
5. **Verification**
6. **Risks / Gaps**

Keep the answer concise. The goal is scanability, not explaining every line.

## Change Summary

Start with the number of changed files when available.

Mention the source inspected, such as working tree, staged diff, branch diff, commit range, PR diff, or user-provided patch.

## High-Level

Use 2-5 short bullets for the overall intent and visible outcome.

Prefer product, behavior, or workflow language over implementation trivia.

## File Summary

Use grouped tables. Group by natural area, such as Backend, Frontend, Mobile UI, Database, Docs, Tests, Config, or Tooling.

Use this table shape:

| File | Surface | Change | Review Focus |
|---|---|---|---|

Guidance:

- Use one row per changed file unless a file has clearly separate concerns.
- Set `Surface` to functions, components, hooks, modules, routes, screens, configs, migrations, tests, docs sections, or other concrete touched surfaces.
- Set `Change` to behavior, visible impact, contract impact, or purpose. Avoid implementation trivia.
- Set `Review Focus` to what a human should inspect, question, or confirm.
- Keep table cells short. Move nuanced explanation to Risks / Gaps.
- If a file is generated or purely mechanical, say so.
- If a file is deleted or renamed, make that explicit in `Change`.

## Contracts / Behavior

Always include a short table for cross-file impact:

| Area | Change |
|---|---|

Cover relevant areas:

- Public function signatures
- Components, hooks, modules, or exported APIs
- API routes, request shapes, or response shapes
- Database schema, migrations, indexes, or seed data
- Config, env vars, permissions, CI, or build behavior
- User-visible behavior
- UI-visible behavior

Say `No changes` when important areas were checked and unchanged.

## Verification

Include commands or checks actually run:

| Check | Result |
|---|---|

Never claim tests, lint, typecheck, screenshots, manual QA, or review tools were run unless they were.

If a likely verification step was not run, include it as `Not run` when it matters for review.

## Risks / Gaps

Use short bullets for:

- behavior that needs product confirmation
- weak or missing tests
- migration, deploy, rollout, or compatibility risk
- UI states not visually checked
- assumptions made while summarizing
- files or generated artifacts that were intentionally skipped

## Style

Prefer:

- concrete changed surfaces
- before/after behavior when relevant
- explicit `No changes` and `Not run` entries
- reviewer-oriented language

Avoid:

- line-by-line explanation
- vague phrases like "updated logic"
- hiding behavior changes in broad wording
- huge tables that mix unrelated areas
- claiming confidence beyond the inspected artifacts

## Small Changes

For 1-2 trivial files, keep the same sections but compress them. It is acceptable to omit Risks / Gaps when there are no meaningful risks.

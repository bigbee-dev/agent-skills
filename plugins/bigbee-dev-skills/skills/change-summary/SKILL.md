---
name: change-summary
description: Summarize an inspected diff or pull request for human review. Use when the user asks what changed or requests a change summary.
---

# Change summary

Inspect the actual working tree, staged diff, commit range, PR diff, or provided
patch. Explain the resulting behavior and why it changed, with enough evidence
for a reviewer to assess it. Follow any format the user requests.

## Match the report to the change

For a small change, use a short paragraph or a few bullets covering the outcome,
relevant files, verification, and any material limitation. Do not require headings
or tables when they add no useful information.

For larger changes, group related files and explain cross-file effects. A useful
table is:

| File or group | Behavior change | Review focus |
| --- | --- | --- |

Use separate contract or verification tables only when they make distinct items
easier to compare. Group generated or mechanical changes; identify deletions and
renames when they matter. Include the inspected diff source and file count when
useful, without making them the lead over the outcome.

## Report what matters

- Explain changes to public APIs, data, configuration, permissions, workflows, and
  user-visible behavior when affected. Use a before/after example when helpful.
- Name risks or assumptions supported by the inspected change. Mention unchanged
  behavior only when preserving it is relevant to the review.
- Report verification actually performed and its result. Distinguish passing,
  failing, and unavailable checks; include unrun checks only when material.
- State skipped files, unverified UI flows, or evidence gaps that limit the review.

Do not invent findings to fill a section, explain every changed line, or imply
that tests, builds, screenshots, or other checks ran when they did not.

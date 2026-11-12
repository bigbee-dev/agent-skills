# Repository Guidelines

## Project Structure & Module Organization

This repository packages shared skills for Codex and Claude Code as plugin marketplace entries.

- `README.md` explains installation and lists included plugins and skills.
- `.agents/plugins/marketplace.json` defines the Codex marketplace entry.
- `.claude-plugin/marketplace.json` defines the Claude Code marketplace entry.
- `plugins/bigbee-dev-skills/.codex-plugin/plugin.json` is the Codex plugin manifest.
- `plugins/bigbee-dev-skills/.claude-plugin/plugin.json` is the Claude Code plugin manifest.
- `plugins/bigbee-dev-skills/skills/<skill-name>/SKILL.md` contains each skill's instructions and front matter.
- `plugins/bigbee-dev-skills/skills/<skill-name>/agents/*.yaml` contains optional agent-facing prompts or UI metadata.

Add new skills under `plugins/bigbee-dev-skills/skills/` using kebab-case directory names, for example `my-new-skill/SKILL.md`.

## Build, Test, and Development Commands

There is no compiled build step. Use structural checks before committing:

```bash
python3 -m json.tool .agents/plugins/marketplace.json
python3 -m json.tool plugins/bigbee-dev-skills/.codex-plugin/plugin.json
python3 -m json.tool .claude-plugin/marketplace.json
python3 -m json.tool plugins/bigbee-dev-skills/.claude-plugin/plugin.json
claude plugin validate .
claude plugin validate ./plugins/bigbee-dev-skills
git diff --check
rg --files
```

The JSON commands validate manifests. `git diff --check` catches whitespace errors. `rg --files` is a quick sanity check that only intended files are present.

## Coding Style & Naming Conventions

Write skill content in concise Markdown with clear headings and actionable instructions. Keep `SKILL.md` front matter valid YAML and include `name` plus `description`. Use kebab-case for plugin and skill identifiers, matching the existing `review-fix-refactor-loop` pattern. JSON manifests use two-space indentation. Keep examples copy-pasteable and repository-relative.

## Testing Guidelines

No automated test suite is currently configured. Validate changed JSON manifests with `python3 -m json.tool`, then read changed Markdown and YAML for broken headings, malformed front matter, and stale paths. When changing skill behavior, check loading in both Codex and Claude Code and invoke the affected skill only against a safe fixture. For Claude Code, use `claude --plugin-dir ./plugins/bigbee-dev-skills` to test the local plugin without installing it. Do not run publishing workflows merely to validate packaging.

## Commit & Pull Request Guidelines

The current history uses short imperative subjects, such as `Add BigBee Codex skills plugin`. Keep commit subjects direct, capitalized, and under about 50 characters. Pull requests should include a brief purpose statement, changed skill or manifest paths, validation commands run, and any manual Codex install or invocation result. Include screenshots only when UI-facing plugin browser metadata changes.

## Agent-Specific Instructions

Keep changes narrowly scoped. Do not rewrite existing skill instructions unless the request targets that skill. When adding a skill, update both the plugin contents and any user-facing listing that should mention it, especially `README.md`. When changing skills, increment `version` in both `plugins/bigbee-dev-skills/.codex-plugin/plugin.json` and `plugins/bigbee-dev-skills/.claude-plugin/plugin.json` so both tools can load the refreshed plugin.

Keep the Codex and Claude Code plugin versions aligned when releasing skill changes. Both manifests use the same `skills/` directory; do not duplicate skill instructions for each tool. For manual-only skills, retain both Codex's `policy.allow_implicit_invocation: false` in `agents/openai.yaml` and Claude's `disable-model-invocation: true` in `SKILL.md` front matter.

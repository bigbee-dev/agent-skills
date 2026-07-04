# Repository Guidelines

## Project Structure & Module Organization

This repository packages shared Codex skills as a plugin marketplace entry.

- `README.md` explains installation and lists included plugins and skills.
- `.agents/plugins/marketplace.json` defines the local plugin marketplace entry.
- `plugins/bigbee-dev-skills/.codex-plugin/plugin.json` is the plugin manifest.
- `plugins/bigbee-dev-skills/skills/<skill-name>/SKILL.md` contains each skill's instructions and front matter.
- `plugins/bigbee-dev-skills/skills/<skill-name>/agents/*.yaml` contains optional agent-facing prompts or UI metadata.

Add new skills under `plugins/bigbee-dev-skills/skills/` using kebab-case directory names, for example `my-new-skill/SKILL.md`.

## Build, Test, and Development Commands

There is no compiled build step. Use structural checks before committing:

```bash
python3 -m json.tool .agents/plugins/marketplace.json
python3 -m json.tool plugins/bigbee-dev-skills/.codex-plugin/plugin.json
git diff --check
rg --files
```

The JSON commands validate manifests. `git diff --check` catches whitespace errors. `rg --files` is a quick sanity check that only intended files are present.

## Coding Style & Naming Conventions

Write skill content in concise Markdown with clear headings and actionable instructions. Keep `SKILL.md` front matter valid YAML and include `name` plus `description`. Use kebab-case for plugin and skill identifiers, matching the existing `review-fix-refactor-loop` pattern. JSON manifests use two-space indentation. Keep examples copy-pasteable and repository-relative.

## Testing Guidelines

No automated test suite is currently configured. Validate changed JSON manifests with `python3 -m json.tool`, then read changed Markdown and YAML for broken headings, malformed front matter, and stale paths. When changing skill behavior, test by installing the plugin locally through Codex and invoking the affected skill prompt.

## Commit & Pull Request Guidelines

The current history uses short imperative subjects, such as `Add BigBee Codex skills plugin`. Keep commit subjects direct, capitalized, and under about 50 characters. Pull requests should include a brief purpose statement, changed skill or manifest paths, validation commands run, and any manual Codex install or invocation result. Include screenshots only when UI-facing plugin browser metadata changes.

## Agent-Specific Instructions

Keep changes narrowly scoped. Do not rewrite existing skill instructions unless the request targets that skill. When adding a skill, update both the plugin contents and any user-facing listing that should mention it, especially `README.md`. Also update `plugins/bigbee-dev-skills/.codex-plugin/plugin.json` so the plugin `version` changes and Codex can reinstall the refreshed plugin.

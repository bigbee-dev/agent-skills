# BigBee Agent Skills

Shared Codex skills for BigBee development tasks.

## Install

Add this repository as a Codex plugin marketplace:

```bash
codex plugin marketplace add bigbee-dev/agent-skills --ref main --sparse .agents/plugins --sparse plugins
```

Then install `bigbee-dev-skills` from the Codex plugin browser.

## Refresh or Upgrade

After adding or changing a skill, update
`plugins/bigbee-dev-skills/.codex-plugin/plugin.json` so the `version` changes.
For local iteration, keep the base version and add a Codex cachebuster suffix:

```json
"version": "0.1.0+codex.local-YYYYMMDD-HHMMSS"
```

After the change is committed and pushed to the marketplace ref, refresh the
configured marketplace snapshot and reinstall the plugin:

```bash
codex plugin marketplace upgrade bigbee-dev-agent-skills
codex plugin add bigbee-dev-skills@bigbee-dev-agent-skills
```

Start a new Codex thread after reinstalling so newly added skills are loaded.
If the marketplace was installed under a different name, check it with:

```bash
codex plugin marketplace list
codex plugin list
```

## Included Plugins

- `bigbee-dev-skills`: shared Codex skills for development tasks.

## Included Skills

- `change-summary`: summarize inspected changes and verification, using concise prose for small diffs and grouped tables when useful.
- `publish-pr-review-loop`: manually invoke `$publish-pr-review-loop` to publish a ready-for-review PR, address accepted Codex feedback, and request re-review until the current head is clean.
- `review-fix-refactor-loop`: manually invoke `$review-fix-refactor-loop` to review, fix, verify, refactor, and re-review code until the in-scope work converges.
- `strengthen-tests`: audit, create, and improve tests when test quality is the primary objective, using real production behavior and meaningful regressions as the standard.

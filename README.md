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

- `change-summary`: summarize local, branch, PR, or manual changes as a grouped review packet with file summaries, contracts, verification, and risks.
- `publish-pr-review-loop`: publish a ready-for-review PR by default, address accepted Codex feedback, and request re-review until the current head is clean.
- `review-fix-refactor-loop`: iteratively review, fix, verify, refactor, and re-review code until the in-scope work converges.
- `strengthen-tests`: create, review, and improve tests so they exercise real production behavior and detect meaningful regressions.

# BigBee Agent Skills

Shared skills for BigBee development tasks in Codex and Claude Code.

## Install

### Codex

Add this repository as a Codex plugin marketplace:

```bash
codex plugin marketplace add bigbee-dev/agent-skills --ref main --sparse .agents/plugins --sparse plugins
```

Then install `bigbee-dev-skills` from the Codex plugin browser.

### Claude Code

Add this repository as a Claude Code plugin marketplace and install the plugin
for all your projects on this machine:

```bash
claude plugin marketplace add bigbee-dev/agent-skills
claude plugin install bigbee-dev-skills@bigbee-dev-agent-skills --scope user
```

The GitHub install requires the Claude manifests to be committed and pushed
first. To install from a local checkout before publishing, replace the first
command with:

```bash
claude plugin marketplace add ~/code/bigbee/agent-skills
```

Start a new Claude Code session and invoke the skills with their plugin prefix:

```text
/bigbee-dev-skills:change-summary
/bigbee-dev-skills:review-fix-refactor-loop
/bigbee-dev-skills:publish-pr-review-loop
/bigbee-dev-skills:strengthen-tests
```

Both tools use the same files under `plugins/bigbee-dev-skills/skills/`.
The publishing and review/refactor loops require explicit invocation in both tools.
The PR publishing skill still requests Codex reviews on GitHub when run by
Claude Code, so the repository needs the Codex GitHub review integration.

## Refresh or Upgrade

After adding or changing a skill, increment the `version` in both
`plugins/bigbee-dev-skills/.codex-plugin/plugin.json` and
`plugins/bigbee-dev-skills/.claude-plugin/plugin.json`.

### Codex

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

### Claude Code

After the new version is committed and pushed, refresh the marketplace and
update the installed plugin:

```bash
claude plugin marketplace update bigbee-dev-agent-skills
claude plugin update bigbee-dev-skills@bigbee-dev-agent-skills --scope user
```

For a marketplace registered from a local checkout, update that checkout first.
Start a new Claude Code session to load the updated plugin.

## Included Plugins

- `bigbee-dev-skills`: shared development skills for Codex and Claude Code.

## Included Skills

In Codex, invoke skills with `$skill-name`. In Claude Code, use
`/bigbee-dev-skills:skill-name`.

- `change-summary`: summarize inspected changes and verification, using concise prose for small diffs and grouped tables when useful.
- `publish-pr-review-loop`: manually invoke `$publish-pr-review-loop` to publish a ready-for-review PR, address accepted Codex feedback, and request re-review until the current head is clean.
- `review-fix-refactor-loop`: manually invoke `$review-fix-refactor-loop` to review, fix, verify, refactor, and re-review code until the in-scope work converges.
- `strengthen-tests`: audit, create, and improve tests when test quality is the primary objective, using real production behavior and meaningful regressions as the standard.

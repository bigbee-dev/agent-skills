# BigBee Agent Skills

Shared Codex skills for BigBee development tasks.

## Install

Add this repository as a Codex plugin marketplace:

```bash
codex plugin marketplace add bigbee-dev/agent-skills --ref main --sparse .agents/plugins --sparse plugins
```

Then install `bigbee-dev-skills` from the Codex plugin browser.

## Included Plugins

- `bigbee-dev-skills`: shared Codex skills for development tasks.

## Included Skills

- `change-summary`: summarize local, branch, PR, or manual changes as a grouped review packet with file summaries, contracts, verification, and risks.
- `review-fix-refactor-loop`: iteratively review, fix, verify, refactor, and re-review code until the in-scope work converges.

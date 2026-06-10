# mjenkins-toolbox

mjenkinsx9's personal [Claude Code plugin](https://docs.claude.com/en/docs/claude-code/plugins) marketplace.
Add the catalog once, then install any plugin from it — in any project.

## Add the marketplace

```bash
/plugin marketplace add mjenkinsx9/mjenkins-toolbox
```

Or enable it everywhere by adding to `~/.claude/settings.json`:

```json
{
  "extraKnownMarketplaces": {
    "mjenkins-toolbox": {
      "source": { "source": "github", "repo": "mjenkinsx9/mjenkins-toolbox" }
    }
  }
}
```

## Plugins

| Plugin | What it does |
|---|---|
| [`skill-kit`](plugins/skill-kit/) | Author, evaluate, security-review, and autonomously improve Claude Agent Skills in any project. Bundles the skill-building workflow as commands, the `improving-skills` autoresearch loop, reference protocols, and the static eval harness. |
| [`anchor`](https://github.com/mjenkinsx9/anchor) | Personal code review (`/anchor`): review uncommitted/staged/branch/PR diffs in-session with severity-graded findings, per-repo learnings, and codebase maps. |

Install one with:

```bash
/plugin install skill-kit@mjenkins-toolbox
```

## Adding a plugin

Drop it under `plugins/<name>/` (with its own `.claude-plugin/plugin.json`) and
add an entry to [`.claude-plugin/marketplace.json`](.claude-plugin/marketplace.json)
pointing at `./plugins/<name>`. The relative source resolves whether the
marketplace is added locally (`/plugin marketplace add ./`) or cloned from GitHub.

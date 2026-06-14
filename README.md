<div align="center">

# 🧺 mjenkins-toolbox

### A personal plugin marketplace for AI coding agents

**Add the catalog once, then install any plugin from it — in whichever coding agent you use.**

[![validate](https://github.com/mjenkinsx9/mjenkins-toolbox/actions/workflows/validate.yml/badge.svg)](https://github.com/mjenkinsx9/mjenkins-toolbox/actions/workflows/validate.yml)
![Plugins](https://img.shields.io/badge/plugins-3-blue)
![Harnesses](https://img.shields.io/badge/harnesses-Claude_Code_·_Codex_·_Cursor_·_more-6c5ce7)
[![License: MIT](https://img.shields.io/badge/license-MIT-yellow.svg)](LICENSE)

</div>

---

This repo is a **plugin marketplace**: a single catalog
([`marketplace.json`](.claude-plugin/marketplace.json)) that points at plugins
living in their own repos. The catalog stays tiny; each plugin owns its code,
CI, releases, and docs.

The catalog is written in the [plugin-marketplace
format](https://docs.claude.com/en/docs/claude-code/plugins) that several agent
harnesses can read. Harnesses that consume a GitHub-hosted marketplace add the
whole catalog directly; others install each plugin from its own repo. Either
way, the plugins themselves are the unit you install.

## 📦 Installation

**Installation differs by harness. If you use more than one, install in each
one separately.** The commands below follow each tool's own plugin/marketplace
syntax — check them against your harness version, since these interfaces are
still moving.

### Claude Code

```
/plugin marketplace add mjenkinsx9/mjenkins-toolbox
/plugin install skill-kit@mjenkins-toolbox
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

### Codex CLI

Open the plugin interface with `/plugins`, add the marketplace
`mjenkinsx9/mjenkins-toolbox`, then search for the plugin you want and select
**Install**.

### Codex App

Click **Plugins** in the sidebar, add the `mjenkinsx9/mjenkins-toolbox`
marketplace, then install a plugin from the catalog.

### Factory Droid

```
droid plugin marketplace add https://github.com/mjenkinsx9/mjenkins-toolbox
droid plugin install skill-kit@mjenkins-toolbox
```

### Cursor

In Cursor Agent chat, add the marketplace and install with `/add-plugin`, or
search for the plugin name in the plugin marketplace UI.

### GitHub Copilot CLI

```
copilot plugin marketplace add mjenkinsx9/mjenkins-toolbox
copilot plugin install skill-kit@mjenkins-toolbox
```

### Gemini CLI / OpenCode

These harnesses install a plugin from its own repo rather than from a shared
catalog. Point them at the individual plugin repos listed below — for example:

```
gemini extensions install https://github.com/mjenkinsx9/skill-kit
```

## 🧩 Plugins

| Plugin | What it does |
|---|---|
| [`skill-kit`](https://github.com/mjenkinsx9/skill-kit) | Author, evaluate, security-review, and autonomously improve Agent Skills — a 21-check static harness, live behavioral checks, empirical trigger measurement, blind value-add baselines, and the `improving-skills` autoresearch loop. |
| [`anchor`](https://github.com/mjenkinsx9/anchor) | Personal code review (`/anchor`): review uncommitted/staged/branch/PR diffs in-session with severity-graded findings, per-repo learnings, and codebase maps. |
| [`autogit`](https://github.com/mjenkinsx9/autogit) | **Auto-commits and pushes every agent turn** in opted-in repos: stage → secrets-scan → commit → push, with the turn's prompt as the commit subject. One-command undo, quiet batching, PR mode. Inert until you run `/autogit on` in a repo — installing it changes nothing by itself. |

Once the marketplace is added, install by name (Claude Code syntax shown):

```
/plugin install skill-kit@mjenkins-toolbox
/plugin install anchor@mjenkins-toolbox
/plugin install autogit@mjenkins-toolbox
```

Updates ship from each plugin's own repo — pick them up with your harness's
update command (e.g. `/plugin update <name>`).

## ➕ Adding a plugin

Each plugin lives in its own repo with a `.claude-plugin/plugin.json` manifest
at the root. To list it here, add an entry to
[`.claude-plugin/marketplace.json`](.claude-plugin/marketplace.json):

```json
{
  "name": "my-plugin",
  "source": { "source": "github", "repo": "mjenkinsx9/my-plugin" },
  "description": "One line on what it does.",
  "category": "development",
  "keywords": ["..."]
}
```

CI validates the catalog (manifest parses, every entry has a description and a
resolvable source) on every push and PR.

## 📄 License

[MIT](LICENSE) © 2026 Mike Jenkins — each plugin carries its own license in its
own repo.

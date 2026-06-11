<div align="center">

# 🧺 mjenkins-toolbox

### A personal Claude Code plugin marketplace

**Add the catalog once, then install any plugin from it — on any machine.**

[![validate](https://github.com/mjenkinsx9/mjenkins-toolbox/actions/workflows/validate.yml/badge.svg)](https://github.com/mjenkinsx9/mjenkins-toolbox/actions/workflows/validate.yml)
![Plugins](https://img.shields.io/badge/plugins-3-blue)
![Made for Claude Code](https://img.shields.io/badge/made_for-Claude_Code-d97757)
[![License: MIT](https://img.shields.io/badge/license-MIT-yellow.svg)](LICENSE)

</div>

---

This repo is a [Claude Code plugin marketplace](https://docs.claude.com/en/docs/claude-code/plugins):
a single catalog ([`marketplace.json`](.claude-plugin/marketplace.json)) that
points at plugins living in their own repos. The catalog stays tiny; each
plugin owns its code, CI, releases, and docs.

## 🚀 Add the marketplace

Inside Claude Code:

```
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

## 🧩 Plugins

| Plugin | What it does |
|---|---|
| [`skill-kit`](https://github.com/mjenkinsx9/skill-kit) | Author, evaluate, security-review, and autonomously improve Claude Agent Skills — a 21-check static harness, live behavioral checks, empirical trigger measurement, blind value-add baselines, and the `improving-skills` autoresearch loop. |
| [`anchor`](https://github.com/mjenkinsx9/anchor) | Personal code review (`/anchor`): review uncommitted/staged/branch/PR diffs in-session with severity-graded findings, per-repo learnings, and codebase maps. |
| [`autogit`](https://github.com/mjenkinsx9/autogit) | **Auto-commits and pushes every agent turn** in opted-in repos: stage → secrets-scan → commit → push, with the turn's prompt as the commit subject. One-command undo, quiet batching, PR mode. Inert until you run `/autogit on` in a repo — installing it changes nothing by itself. *(Repo is private until it finishes testing — installable by the owner only for now.)* |

Install with:

```
/plugin install skill-kit@mjenkins-toolbox
/plugin install anchor@mjenkins-toolbox
/plugin install autogit@mjenkins-toolbox
```

Updates ship from each plugin's own repo — pick them up with
`/plugin update <name>`.

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

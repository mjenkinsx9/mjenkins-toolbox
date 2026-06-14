<div align="center">

# 🧺 mjenkins-toolbox

### A personal plugin marketplace for AI coding agents

**Add the catalog once, then install any plugin from it — in whichever coding agent you use.**

[![validate](https://github.com/mjenkinsx9/mjenkins-toolbox/actions/workflows/validate.yml/badge.svg)](https://github.com/mjenkinsx9/mjenkins-toolbox/actions/workflows/validate.yml)
![Plugins](https://img.shields.io/badge/plugins-3-blue)
![Harnesses](https://img.shields.io/badge/harnesses-Claude_Code_·_Codex_·_Copilot_·_more-6c5ce7)
[![License: MIT](https://img.shields.io/badge/license-MIT-yellow.svg)](LICENSE)

</div>

---

This repo is a **plugin marketplace**: small agent-specific catalogs that point
at plugins living in their own repos. Claude-compatible agents read
[`marketplace.json`](.claude-plugin/marketplace.json); Codex reads
[`marketplace.json`](.agents/plugins/marketplace.json). The catalogs stay tiny;
each plugin owns its code, CI, releases, and docs.

> **Format note.** Every coding agent has its own plugin format and its own
> marketplace command — there is no single cross-vendor standard yet. Each
> plugin in this catalog ships the manifest for every harness below, so you can
> install it in whichever agent you use — see
> [Cross-harness support](#-cross-harness-support). Commands and doc links below
> are verified against each tool's official documentation, but these interfaces
> move quickly, so check the linked docs for your version.

## 📦 Installation

**Installation differs by harness. If you use more than one, install in each
one separately.**

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

Docs: [Plugin marketplaces](https://code.claude.com/docs/en/plugin-marketplaces) ·
[Plugins reference](https://code.claude.com/docs/en/plugins-reference)

### GitHub Copilot CLI

Copilot CLI reads `.claude-plugin/plugin.json`, so the Claude-format plugins
here are compatible.

```
copilot plugin marketplace add mjenkinsx9/mjenkins-toolbox
copilot plugin install skill-kit@mjenkins-toolbox
```

Inside a session the interactive forms are `/plugin marketplace add OWNER/REPO`
and `/plugin install plugin@marketplace`.

Docs: [Finding & installing plugins](https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/plugins-finding-installing) ·
[CLI plugin reference](https://docs.github.com/en/copilot/reference/copilot-cli-reference/cli-plugin-reference)

### OpenAI Codex

Codex has its own marketplace and expects a `.codex-plugin/plugin.json` per
plugin and a catalog at `.agents/plugins/marketplace.json`, both of which the
plugins ship:

```
codex plugin marketplace add mjenkinsx9/mjenkins-toolbox
codex plugin add skill-kit@mjenkins-toolbox
```

In an interactive CLI session, run `/plugins` to browse. In the Codex app, use
the **Plugin Directory** and click **Add to Codex**.

Docs: [Codex plugins](https://developers.openai.com/codex/plugins) ·
[Build a plugin](https://developers.openai.com/codex/plugins/build) ·
[CLI reference](https://developers.openai.com/codex/cli/reference)

### Gemini CLI

Gemini installs **one extension repo at a time** (no shared catalog), and
extensions use a `gemini-extension.json` manifest, which each plugin ships.
Install each plugin from its own repo:

```
gemini extensions install https://github.com/mjenkinsx9/skill-kit
gemini extensions update skill-kit
```

Docs: [Gemini CLI extensions](https://geminicli.com/docs/extensions/) ·
[Extension reference](https://geminicli.com/docs/extensions/reference/)

### Cursor

Cursor installs from its own marketplace with `/add-plugin` (or the
[cursor.com/marketplace](https://cursor.com/marketplace) UI). There is **no
user command to add an arbitrary GitHub marketplace** — Teams/Enterprise can
import a repo via **Dashboard → Settings → Plugins → Add Marketplace**. Each
plugin ships the `.cursor-plugin/plugin.json` Cursor expects.

Docs: [Cursor plugins](https://cursor.com/docs/plugins) ·
[Authoring reference](https://cursor.com/docs/reference/plugins)

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

## 🌍 Cross-harness support

| Harness | Marketplace command | Plugin manifest |
|---|---|---|
| Claude Code | `/plugin marketplace add` | `.claude-plugin/plugin.json` |
| GitHub Copilot CLI | `copilot plugin marketplace add` | reads `.claude-plugin/plugin.json` |
| OpenAI Codex | `codex plugin marketplace add` | `.codex-plugin/plugin.json` |
| Gemini CLI | `gemini extensions install <repo>` (no catalog) | `gemini-extension.json` (MCP) |
| Cursor | none (official marketplace + `/add-plugin`) | `.cursor-plugin/plugin.json` |

Each plugin repo ships every manifest above, so it installs natively in any of
these harnesses. The portable core is **Agent Skills** (`SKILL.md`), which
Claude Code, Codex, Copilot CLI, and Cursor all support; Gemini uses an
MCP-based extension model.

## ➕ Adding a plugin

Each plugin lives in its own repo with agent-specific manifests at the root.
To list it for Claude-compatible agents, add an entry to
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

To list it for Codex, add the matching entry to
[`.agents/plugins/marketplace.json`](.agents/plugins/marketplace.json):

```json
{
  "name": "my-plugin",
  "source": { "source": "url", "url": "mjenkinsx9/my-plugin", "ref": "main" },
  "policy": {
    "installation": "AVAILABLE",
    "authentication": "ON_INSTALL"
  },
  "category": "Developer Tools"
}
```

## 📄 License

[MIT](LICENSE) © 2026 Mike Jenkins — each plugin carries its own license in its
own repo.

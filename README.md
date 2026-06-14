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

This repo is a **plugin marketplace**: a single catalog
([`marketplace.json`](.claude-plugin/marketplace.json)) that points at plugins
living in their own repos. The catalog stays tiny; each plugin owns its code,
CI, releases, and docs.

> **Format note.** Every coding agent has its own plugin format and its own
> marketplace command — there is no single cross-vendor standard yet. This
> catalog and its plugins are authored in **Claude Code's** plugin format, so
> Claude Code (and GitHub Copilot CLI, which reads the same manifest) work as-is.
> The other harnesses below need the plugins repackaged into their format first
> — see [Cross-harness support](#-cross-harness-support). Commands and doc links
> below are verified against each tool's official documentation, but these
> interfaces move quickly, so check the linked docs for your version.

## 📦 Installation

**Installation differs by harness. If you use more than one, install in each
one separately.**

### Claude Code — works today

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

### GitHub Copilot CLI — works today

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

### OpenAI Codex — needs repackaging

Codex has its own marketplace, but expects a `.codex-plugin/plugin.json` per
plugin and a catalog at `.agents/plugins/marketplace.json` (different from this
repo's Claude-format catalog). Once packaged:

```
codex plugin marketplace add mjenkinsx9/mjenkins-toolbox
codex plugin add skill-kit@mjenkins-toolbox
```

In an interactive CLI session, run `/plugins` to browse. In the Codex app, use
the **Plugin Directory** and click **Add to Codex**.

Docs: [Codex plugins](https://developers.openai.com/codex/plugins) ·
[Build a plugin](https://developers.openai.com/codex/plugins/build) ·
[CLI reference](https://developers.openai.com/codex/cli/reference)

### Factory Droid — needs repackaging

Droid plugins need a `.factory-plugin/plugin.json`. Once packaged:

```
droid plugin marketplace add https://github.com/mjenkinsx9/mjenkins-toolbox
droid plugin install skill-kit@mjenkins-toolbox
```

Or use the `/plugins` slash command to browse marketplaces in-session.

Docs: [Droid plugins](https://docs.factory.ai/cli/configuration/plugins)

### Gemini CLI — needs repackaging

Gemini installs **one extension repo at a time** (no shared catalog), and
extensions use a `gemini-extension.json` manifest built around MCP servers.
Install each plugin from its own repo once it ships a Gemini extension:

```
gemini extensions install https://github.com/mjenkinsx9/skill-kit
gemini extensions update skill-kit
```

Docs: [Gemini CLI extensions](https://geminicli.com/docs/extensions/) ·
[Extension reference](https://geminicli.com/docs/extensions/reference/)

### Cursor — official marketplace only

Cursor installs from its own marketplace with `/add-plugin` (or the
[cursor.com/marketplace](https://cursor.com/marketplace) UI). There is **no
user command to add an arbitrary GitHub marketplace** — Teams/Enterprise can
import a repo via **Dashboard → Settings → Plugins → Add Marketplace**. Plugins
need a `.cursor-plugin/plugin.json`.

Docs: [Cursor plugins](https://cursor.com/docs/plugins) ·
[Authoring reference](https://cursor.com/docs/reference/plugins)

### OpenCode — no marketplace

OpenCode plugins are JS/TS modules loaded from `.opencode/plugins/` or from npm
packages listed in config — there is no marketplace or GitHub-URL install. A
skills/commands plugin would need to be rebuilt as an OpenCode plugin module.

Docs: [OpenCode plugins](https://opencode.ai/docs/plugins/)

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

| Harness | Marketplace command | Plugin manifest | Status |
|---|---|---|---|
| Claude Code | `/plugin marketplace add` | `.claude-plugin/plugin.json` | ✅ Works today |
| GitHub Copilot CLI | `copilot plugin marketplace add` | reads `.claude-plugin/plugin.json` | ✅ Works today |
| OpenAI Codex | `codex plugin marketplace add` | `.codex-plugin/plugin.json` | 🔧 Repackage |
| Factory Droid | `droid plugin marketplace add` | `.factory-plugin/plugin.json` | 🔧 Repackage |
| Gemini CLI | `gemini extensions install <repo>` (no catalog) | `gemini-extension.json` (MCP) | 🔧 Repackage |
| Cursor | none (official marketplace + `/add-plugin`) | `.cursor-plugin/plugin.json` | 🔧 Repackage |
| OpenCode | none (npm / local files) | JS/TS module | 🔧 Rebuild |

"Repackage" means adding that ecosystem's manifest to each plugin repo. The
portable core is **Agent Skills** (`SKILL.md`), which Claude Code, Codex,
Copilot CLI, and Cursor all support; Gemini and OpenCode use different
extension models.

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

<div align="center">

# 🧺 mjenkins-toolbox

### A personal plugin marketplace for AI coding agents

**Add the catalog once, then install any plugin from it — in whichever coding agent you use.**

[![validate](https://github.com/mjenkinsx9/mjenkins-toolbox/actions/workflows/validate.yml/badge.svg)](https://github.com/mjenkinsx9/mjenkins-toolbox/actions/workflows/validate.yml)
![Plugins](https://img.shields.io/badge/plugins-3-blue)
![Harnesses](https://img.shields.io/badge/harnesses-Claude_Code_·_Codex_·_Pi_·_Copilot_·_more-6c5ce7)
[![License: MIT](https://img.shields.io/badge/license-MIT-yellow.svg)](LICENSE)

</div>

---

This repo is a **plugin marketplace**: small agent-specific catalogs that point
at plugins living in their own repos. Claude-compatible agents read
[`marketplace.json`](.claude-plugin/marketplace.json); Codex reads
[`marketplace.json`](.agents/plugins/marketplace.json). Pi does not have a
marketplace catalog command, so Pi users install the plugin repos directly as
Pi packages. The catalogs stay tiny; each plugin owns its code, CI, releases,
and docs.

> **Format note.** Every coding agent has its own plugin format and its own
> marketplace command — there is no single cross-vendor standard yet. Each
> plugin in this catalog ships the manifest for every harness below, so you can
> install it in whichever agent you use — see
> [Cross-harness support](#-cross-harness-support). Commands and doc links below
> are verified against each tool's official documentation, but these interfaces
> move quickly, so check the linked docs for your version. Pi support uses Pi's
> package and Agent Skills conventions rather than the Claude/Codex marketplace
> manifests.

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

### Pi

Pi installs shared capabilities as **Pi packages**. There is no separate
`marketplace add` command in Pi; the Pi equivalent is to install each plugin
repo from this catalog:

```bash
pi install git:github.com/mjenkinsx9/skill-kit
pi install git:github.com/mjenkinsx9/anchor
pi install git:github.com/mjenkinsx9/autogit
```

If you keep local clones under `~/github`, install those instead:

```bash
pi install ~/github/skill-kit
pi install ~/github/anchor
pi install ~/github/autogit
```

Then restart Pi or run `/reload`, and use the skills by name:

```text
/skill:improving-skills
/skill:anchor-review
/skill:autogit-ops status
```

To update installed Pi packages:

```bash
pi update --extensions
```

For `autogit`, the skill can run status/on/off/undo/ship commands. Automatic
after-turn shipping in Pi also needs autogit's Pi extension wired once, then a
per-repo opt-in:

```bash
node ~/.pi/agent/git/github.com/mjenkinsx9/autogit/index.js setup
```

Then, inside the repo you want to auto-ship:

```text
/skill:autogit-ops on
```

Repos without `autogit on` are inert. If you installed from a local clone,
you can also run `node ~/github/autogit/index.js setup`.

Docs: [Pi packages](https://pi.dev/docs/packages) ·
[Pi skills](https://pi.dev/docs/skills) ·
[Pi extensions](https://pi.dev/docs/extensions)

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
update command (e.g. `/plugin update <name>` or `pi update --extensions`).

## 🥧 Pi capabilities

Pi reads the portable `skills/` directories from each plugin repo when installed
with `pi install git:github.com/mjenkinsx9/<plugin>`. It does not read the
Claude `.claude-plugin/`, Codex `.codex-plugin/`, or marketplace JSON files.

| Plugin | Pi capability |
|---|---|
| `skill-kit` | Loads `/skill:improving-skills` for improving and scoring Agent Skills. |
| `anchor` | Loads `/skill:anchor-review` for review, diff/context gathering, repo learnings, status, and diagnostics using the bundled Anchor CLI. |
| `autogit` | Loads `/skill:autogit-ops` for status/on/off/undo/ship. Automatic Pi after-turn shipping requires the one-time `node .../autogit/index.js setup` extension wiring plus per-repo `/skill:autogit-ops on`. |

Pi package installs run with the user's local permissions. Review third-party
plugin source before installing, and only enable `autogit` in repos where
automatic commits and pushes are desired.

## 🌍 Cross-harness support

| Harness | Marketplace command | Plugin manifest |
|---|---|---|
| Claude Code | `/plugin marketplace add` | `.claude-plugin/plugin.json` |
| GitHub Copilot CLI | `copilot plugin marketplace add` | reads `.claude-plugin/plugin.json` |
| OpenAI Codex | `codex plugin marketplace add` | `.codex-plugin/plugin.json` |
| Pi | no marketplace command; install each repo with `pi install git:github.com/mjenkinsx9/<plugin>` | `skills/` via Pi package conventions; `autogit` can also wire a Pi extension |
| Gemini CLI | `gemini extensions install <repo>` (no catalog) | `gemini-extension.json` (MCP) |
| Cursor | none (official marketplace + `/add-plugin`) | `.cursor-plugin/plugin.json` |

Each plugin repo ships the manifest needed by the manifest-based harnesses
above and exposes portable skills for Pi. The portable core is **Agent Skills**
(`SKILL.md`), which Claude Code, Codex, Copilot CLI, Cursor, and Pi all
support; Gemini uses an MCP-based extension model.

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

Pi does not use this catalog file. To make a plugin usable from Pi, keep the
portable skill content under `skills/` (directories with `SKILL.md`) or declare
Pi resources in the plugin repo's `package.json` under a `pi` key. Users install
that plugin repo directly with `pi install git:github.com/mjenkinsx9/my-plugin`.

## 📄 License

[MIT](LICENSE) © 2026 Mike Jenkins — each plugin carries its own license in its
own repo.

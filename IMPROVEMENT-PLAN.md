# Improvement plan — mjenkins-toolbox

Updated 2026-06-10 after auditing drift against the upstream dev-bench
(`C:/github/skill-testing`), which landed harness hardening, self-tests + CI,
value-add tally fixes, and an MIT license **today**. Several items from the
original review are now "port from upstream" instead of "write from scratch."

MIGRATION.md's premise — "the methodology is stable, so drift risk is low" —
is already falsified: one week after the split, the dev-bench harness is 144
lines ahead (643 vs 499) and the plugin's copy is strictly weaker. Phase S
(sync) is now the priority; the original P-items that survive follow.

## Phase S — Sync from skill-testing (upstream is ahead)

> **STATUS: COMPLETED 2026-06-10** (all of S1–S8, including the S7
> behavioral-check port). Verification: improving-skills scores 0 FAIL /
> 0 WARN under the hardened harness; self-tests 10/10; pytest 26/26; both
> manifests validate; SYNC.md drift check shows only documented adaptations.
> Changes are in the working tree, uncommitted. Remaining work: the
> "Surviving original items" below.

S1. **Port the hardened `check-skill`** (upstream `eval/check-skill.sh`,
    commit d7d18e8). The plugin copy is missing:
    - `mktemp` + EXIT trap for the stderr scratch file (replaces the
      hardcoded `/tmp/check-skill-err` — original P0.5)
    - trailing-slash normalization of the target path (a `dir/` argument
      silently disables the Checks 16–20 subdir exclusions)
    - Check 7 real trigger lint — the current plugin check is unfailable
      (a bare `" for "` passes); upstream now requires genuine trigger
      phrasing
    - Check 7b restructure (always emits exactly one line)
    - Check 10 tree-wide Windows-path scan over reference .md files, with the
      `allowlist windows-path` per-line pragma
    - **Checks 12/13 tree-wide** — security smells and secret scans now cover
      every .md and script file in the skill dir. The plugin version scans
      SKILL.md only, so today a secret in a reference file passes the plugin
      harness. This is the most important single port.
    - Check 14b: structural headings (Contents/Notes/Setup/…) no longer
      inflate the tests.md scenario count; 14c: staleness date anchored to
      the `Last verified:` line, not the first date anywhere in the file
    - Check 21 (staging/live drift): dev-bench-flavored, but degrades
      gracefully to "no staging twin" in consumer projects — port it for
      parity (keeps future diffs clean) or document the omission.
    Adaptation to preserve: bare-name invocation and the plugin usage string.

S2. **Port the harness self-tests + CI** (upstream `eval/tests/` — 9 fixture
    skills, `run-self-tests.sh`, 26 pytest tests — and
    `.github/workflows/harness.yml`). Covers original P2.12/P2.13 with work
    that already exists. Adapt: paths (`bin/check-skill` instead of
    `eval/check-skill.sh`), and the workflow's skill sweep should run the
    harness on `plugins/skill-kit/skills/improving-skills` (zero FAILs; zero
    WARNs once S4 lands).

S3. **Port the value-add tally fixes** (upstream 13e095d): the small-sample
    caveat line ("verdict rests on N rows; treat margins within ±1.5 as
    noise; add seeds (5+) before acting on a CONCERN"), the updated CONCERN
    message, and the matching paragraph in `reference/value-add-test.md`.
    While in the file, fix the plugin-only bug upstream doesn't have:
    `run_dir="runs/$run_id"` must become
    `${CLAUDE_PROJECT_DIR:-.}/.skill-kit/runs/` (original P0.1 — upstream's
    `runs/` is correct for the dev-bench, wrong for the plugin).

S4. **Restructure `improving-skills` to match upstream** (de304ba +
    c46ad07, June 5 — the plugin synced that day's harness checks but not
    the skill restructure, which is exactly why the plugin skill now scores
    4 WARNs under its own harness):
    - move `loop.md` + `scoring.md` → `references/` subdir (fixes Check 18)
    - add the `## Contents` TOCs upstream added (fixes Check 17)
    - update SKILL.md's relative links accordingly

S5. **Add the MIT LICENSE** (upstream added one today "ahead of going
    public"; this marketplace repo distributes publicly with none —
    original P0.2). Add `license` to plugin.json and marketplace.json.

S6. **Resolve the `writing-descriptions.md` dangling reference.** Confirmed:
    the file is `WRITING-DESCRIPTIONS.md` at the *dev-bench repo root*, so
    the plugin's pointer in `reference/trigger-accuracy.md:95` (wrong casing,
    wrong repo) can never resolve for a consumer. Either bundle a copy under
    `reference/` or inline the one-line advice and drop the pointer.

S7. **Decide on porting `behavioral-check`** (upstream PR #21:
    `eval/behavioral-check.py` + `.md`). Entirely new capability — executes
    a skill's own tests.md commands live and reports exit codes/sanity flags.
    It is exactly what the plugin advertises ("evaluate … skills in any
    project") and complements the existing static/trigger/value-add trio.
    Recommended: port as `bin/behavioral-check` + `reference/behavioral-check.md`
    after S1–S2 land. If deferred, note it in MIGRATION.md.

S8. **Establish a sync protocol so this doesn't recur.** Options, pick one:
    - a `SYNC.md` table of file pairs (dev-bench path ↔ plugin path +
      adaptation notes) and a one-liner diff script to run before each
      plugin release; or
    - invert the dependency: make the plugin canonical for the shared
      harness/scripts and have the dev-bench consume the installed plugin.
    Either way, correct MIGRATION.md's "drift risk is low" claim and bump
    the plugin version (0.1.0 → 0.2.0) when Phase S ships.

## Surviving original items (not addressed by upstream)

> **STATUS: COMPLETED 2026-06-10** (P1–P6). P6 ran as a simulated-install
> portability test in a throwaway project: full improving-skills loop
> exercised (results recorded in the skill's tests.md, all TBDs replaced
> with observed output), and it caught a real bug — `value-add-test`
> couldn't parse the canonical `y | prompt` fixture format the plugin's own
> template prescribes; fixed in both twins. Remaining last-mile item is a
> true `/plugin install` smoke test in an interactive session (tracked in
> MIGRATION.md).

P1. **Fix stale pre-migration paths in the plugin's
    `skills/improving-skills/tests.md`** — `/improving-skills examples/…`,
    `runs/<run-id>/`, `scripts/score-skill.sh` are all correct upstream but
    wrong here; should read `/skill-kit:improving-skills`,
    `.skill-kit/runs/…`, bare `score-skill`. (Upstream's tests.md is equally
    a TBD museum — see P6.)

P2. **Guard division-by-zero in `bin/score-skill`** — empty candidate file →
    `cand_tokens=0` → awk divides by zero. (Also exists upstream in
    `scripts/score-skill.sh`; fix both.)

P3. **Gitignore `.serena/`** (currently untracked noise in git status).

P4. **Document the `/goal` dependency** in both `goal-*` commands and the
    README — `/goal` is not part of this plugin nor stock Claude Code; give
    consumers a fallback ("treat goal.md as your acceptance checklist").
    Also de-dev-bench the text: `goal-new-skill.md` cites "CLAUDE.md
    mandates," a file consumers don't have.

P5. **Reconcile the plugin README's "Planned: agents/" section** with
    MIGRATION.md's verdicts (skill-importer dropped, others deferred).

P6. **Run the real-world portability test and fill the tests.md TBDs**
    (MIGRATION.md's open item): install the plugin in a throwaway
    non-dev-bench project, run `/skill-kit:improving-skills` end to end,
    paste actual verified outputs, refresh `Last verified`.

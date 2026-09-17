# Pi profile: game-dev

## Git

Commit format: `<type>[(scope)][!]: <description>`, with the body and footers
separated by a blank line.

Types: `feat` (MINOR) · `fix` (PATCH) · `refactor` · `test` · `docs` · `ci` ·
`chore` · `perf` · `build`.
Breaking change: `!` after the type/scope **or** a `BREAKING CHANGE:` footer
(MAJOR, case-sensitive).
Reference: https://www.conventionalcommits.org/en/v1.0.0/

Gitflow: branches `feature/*`, `fix/*`, and `release/*` from `develop`, merged
into `main` for releases. **Never** fast-forward merge; always use `--no-ff`.

## RTK — Rust Token Killer

Token-optimized CLI proxy (60–90% savings on development operations).

Meta-commands, to be entered exactly as shown:

```bash
rtk gain              # Token savings analytics
rtk gain --history    # Command history with savings
rtk discover          # Identify missed opportunities
rtk proxy <cmd>       # Run an unfiltered raw command (debugging)
```

All other commands are automatically rewritten by `extensions/rtk.ts`:
`git status` becomes `rtk git status` before execution. No prefix needs to be
entered manually. `rtk hook check "<cmd>"` shows the rewrite without applying
it.

⚠ Name collision: if `rtk gain` fails, reachingforthejack/rtk (Rust Type Kit)
is probably installed instead.

## Bash commands

Prefer these tools over their default equivalents. Fall back silently if they
are unavailable.

- **Content search**: `rg` rather than `grep`
- **File search**: `fd` rather than `find`
- **JSON**: `jq` for all parsing, filtering, or transformations
- **YAML/TOML**: `yq`
- **GitHub**: `gh` for PRs, issues, reviews, CI, and releases. Do not scrape
  github.com or use the REST API when `gh` is sufficient.
- **GitLab**: `glab` for MRs, issues, reviews, CI, and releases. Same rule.

## Agent workflow

- **Verify in the browser** (`/skill:playwright-cli`) for **every** user story
  with UI impact, if UI is in browser.
- Always finish a coding task with `/simplify`, then
  `/skill:ponytail-review`, then apply the adjustments.

## Orchestration

- If things go off track, STOP and re-plan immediately—do not persist blindly.
- Write detailed specs upfront to reduce ambiguity.
- Use the `subagent` tool freely to keep the main context clean: delegate
  research, exploration, and parallel analysis. The available agents are those
  provided by `pi-subagents`.
- After EVERY user correction, record the pattern in `tasks/lessons.md` and
  write a rule for yourself.
- Never mark a task as complete without proving that it works: run the tests,
  check the logs, and demonstrate the fix.
- For non-trivial changes, ask: “Is there a more elegant way?” Skip this step
  for simple, obvious fixes.

## Principles

- **Simplicity first**: make every change as simple as possible.
- **No laziness**: find root causes. No temporary fixes.
- **Minimal impact**: change only what is necessary.

## Language

Respond in English. Keep technical terms and code identifiers unchanged.

## Ontology-first game development

This profile develops game repositories ontology-first.

- Every gameplay feature starts with `/skill:game-from-ontology`, which
  chains the `ontology` and `router` skills.
- `ontology/` at the repo root is the source of truth. Development never
  drifts ahead of it.
- **Sync rule (step 0):** land the change in `ontology/` before implementing
  any domain-touching task. Skip only for debugging, build or tooling work.
- Always route gamedev implementation through the `router` skill. Never
  hand-pick engine or genre skills yourself.
- **Exemption:** `prototype-fast` spikes and `game-jam` builds are
  pre-ontology, timeboxed, on a separate branch or repo, and never merged. A
  spike that proves fun feeds the ontology; its code is deleted.
- To resume work on an existing ontology-first repo, use
  `/skill:ontology-resume-router-slice`. It expects the `subagent` tool for
  its per-phase model routing.

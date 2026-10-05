# pi profile: power-dev

## Git

Commit format: `<type>[(scope)][!]: <description>`, with the body and footers
separated by a blank line.

Types: `feat` (MINOR) · `fix` (PATCH) · `refactor` · `test` · `docs` · `ci` ·
`chore` · `perf` · `build`.
Breaking change: `!` after the type/scope **or** a `BREAKING CHANGE:` footer
(MAJOR, case-sensitive).
Reference: https://www.conventionalcommits.org/en/v1.0.0/

Gitflow: create `feat/*`, `fix/*`, `release/*` branches from `develop`, merge
into `main` at release; use `--ff-only` if the Git repository has fewer than
4 developers.

## RTK — Rust Token Killer

Token-optimized CLI proxy (60-90% savings on development operations).

Meta-commands, to be entered exactly as shown:

```bash
rtk gain              # Token savings analytics
rtk gain --history    # Command history with savings
rtk discover          # Find missed opportunities
rtk proxy <cmd>       # Run a raw command without filtering (debug)
```

All other commands are automatically rewritten by
`extensions/rtk.ts`: `git status` becomes `rtk git status` before execution.
No need to add a prefix manually. `rtk hook check "<cmd>"` shows the rewrite
without applying it.

⚠ Name collision: if `rtk gain` fails, reachingforthejack/rtk (Rust Type Kit)
is probably installed instead.

## Bash commands

Prefer these tools over the defaults. Fall back silently if unavailable.

- **Content search**: `rg` rather than `grep`
- **File search**: `fd` rather than `find`
- **JSON**: `jq` for all parsing, filtering or transformation
- **YAML/TOML**: `yq`
- **GitHub**: `gh` for PRs, issues, reviews, CI, releases. Do not scrape
  github.com or call the REST API when `gh` is sufficient.
- **GitLab**: `glab` for MRs, issues, reviews, CI, releases. Same rule.

## Agent workflow

- **Invoke `/skill:using-superpowers` at the start of the session** for any
  development, feature or implementation request; not for a simple question.
- For any nontrivial task (3+ steps or an architectural decision), start with
  `/skill:brainstorming`. pi has no plan mode: discipline comes from the
  skill, not the harness.
- **Verify in the browser** (`/skill:playwright-cli`) for **every** user story
  that affects the UI.
- Always finish a coding task with `/simplify`, then
  `/skill:ponytail-review`, then apply the adjustments.

## Orchestration

- If things go off track, STOP and replan immediately — do not keep pushing.
- Write detailed specs up front to reduce ambiguity.
- Use the `subagent` tool freely to keep the main context clean: delegate
  research, exploration and parallel analysis. Available agents are provided
  by `pi-subagents`.
- After user corrections: record the pattern in `docs/lessons.md` and
  write a rule for yourself.
- Never mark a task as complete without proving it works: run the tests,
  check the logs, demonstrate the fix.
- For nontrivial changes, ask yourself "Is there a more elegant way?".
  Skip this step for simple, obvious fixes.

## Principles

- **Simplicity first**: make every change as simple as possible.
- **No laziness**: find the root causes. No temporary fixes.
- **Minimal impact**: only touch what is necessary.

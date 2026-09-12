# 0002. Reference skill collections as git submodules with symlinked skills

- **Date:** 2026-08-18
- **Status:** Accepted
- **Deciders:** M4jor-Tom

## Context

The 68 third-party skills were initially vendored (flat copies), which orphaned them from
upstream updates. Three of them additionally carried local patches (router sync step,
spike/jam exemptions), creating silent-drift risk against upstream. Claude Code requires
the flat `skills/<name>/SKILL.md` layout, while the gamedev repo nests skills in category
directories.

## Decision

We will reference both upstream repos as git submodules under `vendor/` and populate
`skills/` with relative symlinks into them; `game-from-ontology` is the only real skill in
this repo. All local policy (sync rule, exemptions) lives in `CLAUDE.md` — always in
context, stronger than skill-body text — so no third-party file is patched locally.
`vendor/claude-ontology-skill` is pinned to the fork branch carrying upstream PR #1 until
that PR merges. Dependabot (`gitsubmodule`) bumps submodules weekly; CI runs
`scripts/check.sh` (submodules initialized, ≥70 skills resolve).

## Alternatives Considered

- **Vendored copies:** rejected — no upstream updates, and patched copies drift silently.
- **Patched fork of the gamedev repo as submodule:** rejected — creating that public fork
  was denied by the permission gate, and moving the ~15 policy lines into CLAUDE.md turned
  out cleaner: zero drift surface instead of managed drift.
- **Submodule at `skills/` directly:** impossible — upstream nests skills by category;
  Claude Code needs the flat layout.

## Consequences

- Positive: upstream updates via PR bumps; exact provenance (pinned commits); local diff
  surface reduced to one skill + CLAUDE.md.
- Negative: consumers must init submodules (`--recurse-submodules`); a plain `cp -r` of the
  directory breaks symlinks (documented in README).
- Neutral: symlinks require a POSIX filesystem; on Windows, git needs `core.symlinks=true`.

## Resumption (for Agent)

- **Current state:** submodules + symlinks in place; CI and Dependabot configured; pushed
  to `origin` (M4jor-Tom/claude-game-dev).
- **How to verify:** `sh scripts/check.sh`; `git submodule status`.
- **Pending:** when claude-ontology-skill PR #1 merges upstream, repoint
  `vendor/claude-ontology-skill` to upstream default branch (owner handles this).

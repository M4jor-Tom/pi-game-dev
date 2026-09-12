# 3. Skills as pinned pi packages, not submodules with symlinks

Date: 2026-09-12

## Status

Accepted. Supersedes ADR 0002 for this repository.

## Context

ADR 0002 chose git submodules under `vendor/` plus 69 relative symlinks in
`skills/`, because Claude Code only looks one level deep for `SKILL.md` and
the upstream collection nests skills by category.

Two things changed when this profile moved to pi:

1. **pi's skill discovery recurses to unlimited depth**, stopping at the
   first directory containing `SKILL.md`. One path covers all 67 gamedev
   skills. Every symlink ADR 0002 introduced exists only to work around a
   limitation pi does not have.
2. **pi has a package manager whose manifest is `settings.json`.** A pinned
   `git:host/user/repo@sha` entry is cloned into the gitignored `git/`
   subtree at startup and reconciled by `pi update --extensions`.

## Decision

Carry third-party skill collections as pinned `packages[]` entries. Delete
the `vendor/` submodules and all 69 symlinks.

Locally authored skills stay as plain directories under `skills/`. That
includes `ontology-resume-router-slice`, which was an untracked nested git
repository — copying its three files removes a repo-inside-a-repo that git
never handled cleanly.

## Consequences

- A clone is text-only and small. `--recursive` is no longer needed.
- Nothing on disk is a symlink, so the Windows `core.symlinks` caveat from
  ADR 0002 is gone, as is the class of broken-symlink failures
  `scripts/check.sh` was written to catch.
- `check.sh` can no longer count vendored skills: after pi installs them they
  live under the gitignored `git/` tree. It now checks the authored skills
  and asserts every package source is pinned.
- Dependabot's `gitsubmodule` ecosystem no longer applies. Bumping an upstream
  means editing a ref in `settings.json`, which is a reviewable one-line diff
  rather than an opaque gitlink change.
- The first run after a fresh clone needs network access to install packages.

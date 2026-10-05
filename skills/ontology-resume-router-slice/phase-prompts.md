# Phase dispatch prompts

Every dispatch: one phase, the file paths it needs, the report path, the no-subagents contract
("never dispatch subagents; the controller reviews"). Pick the model per SKILL.md Model Selection and
name it explicitly. Nothing from the session's history is pasted in; the ledger and the brief files
carry the state.

## Ontology-sync implementer

```
Subagent (general-purpose), model: <standard>
You are landing decision D<n> "<topic>" in the ontology of <repo>, an ontology-first game project
(`ontology/` is the source of truth; engine code only consumes it). Read the brief first, it is
your requirements: <workspace>/D<n>-brief.md (the §3 classes, §6 generators and instance files
that govern the feature, the numbers the sources never gave, and the invariants to enforce).

Do, in order:
1. `ontology/instances/generators.json#design.<topic>` — the designed numbers, with a `_doc` that
   states date, decision id, what each key means and which engine file consumes it.
2. `ontology/domain.md`: a "Hybrid (D<n>): …" line under every affected §3 class, a `c-<topic>-config`
   row in §5, a `**D<n> <title> — DECIDED <date>**` entry in §7 (what was decided, what is not yet).
3. `docs/ROADMAP/todo_decide.md`: `- [x] **D<n> <title> (<date>)** — … Recorded: …`.
4. `ontology/README.md`: one dated status line.
5. The validator (`ontology/model.<ext>` validate()): a rule that rejects every way the new design
   block can drift (ids that must exist elsewhere, ranges, required keys).
6. Run the validator; it must pass. Then break the new block on purpose in a scratch copy and confirm
   the rule fires; restore.
7. Commit: `ontology: D<n> <topic> (<what design keys, what constraint>)`.
Never dispatch subagents. Write the full report to <workspace>/D<n>-ontology-report.md and return only:
status (DONE | DONE_WITH_CONCERNS | NEEDS_CONTEXT | BLOCKED), commit hash, validator result, concerns.
```

## Slice implementer

```
Subagent (general-purpose), model: <standard or most capable for multi-file>
You are implementing slice D<n> "<topic>" in <repo>. Read the brief first, it is your
requirements: <workspace>/D<n>-brief.md. The ontology already carries the decision (commit <hash>):
consume `design.<topic>` and the constraint; never redefine numbers in code.
Engine skills to read before coding: <list from the router step>.
Rules: lazy version first; every deliberate cut = a `ponytail:` comment naming the ceiling and a line
in docs/ROADMAP/todo_implement.md; one headless test for the slice (`<test command pattern>`) and the
boot check must pass; run every check under `timeout`; injected config (no autoload names in
gameplay scripts) so tests can drive it.
Gotchas from docs/HANDOFF.md that bind this slice: <copy the relevant lines verbatim>.
When done: update game/README.md (folder table + checks list), todo_implement.md, commit as
`<Slice name> slice: D<n> <one line per touched file>`.
Never dispatch subagents. Write the full report to <workspace>/D<n>-slice-report.md and return only:
status, commit hash(es), one line of test results, concerns.
```

## Slice reviewer

```
Subagent (general-purpose), model: <standard; most capable for a large diff>
Review slice D<n> of <repo>. Inputs: brief <workspace>/D<n>-brief.md, report
<workspace>/D<n>-slice-report.md, diff <workspace>/D<n>-review.diff (commit list + stat + full diff).
Two verdicts, both required:
- Spec compliance: does the code consume `design.<topic>` for every number, does every brief item
  exist, does every cut have a `ponytail:` comment AND a todo_implement.md line?
- Quality: hidden ontology drift (numbers in code), missing validator coverage, tests that assert
  nothing, gotchas violated (`@onready` in setup, autoload names in gameplay scripts, unbounded runs).
Never dispatch subagents. Write findings to <workspace>/D<n>-review.md; return only: SPEC ✅/❌,
QUALITY ✅/❌, count of findings, the single most severe one.
```

## Player changelog writer

```
Subagent (general-purpose), model: <cheap>
Write docs/changelogs/pre-v1/<yyyymmddhhmmss>_D<n>_<slice hash>.md for the players of <game>: it goes
on the game's page. Inputs: the slice brief <workspace>/D<n>-brief.md and `git show --stat <hash>`.
Shape: a title that names what players can now do + the date; short sections by thing the player
touches (weapon, skill, screen, place); one line per behaviour ("Bow: arrows drop with distance");
a final "Coming next:" line. No file names, ids, constraint names, commit talk, numbers players
cannot see, or "deferred" lists. Under 40 lines. Return the path only.
```

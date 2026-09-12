# 0001. Adopt ontology-first game development via a reusable .claude config

- **Date:** 2026-08-18
- **Status:** Accepted
- **Deciders:** M4jor-Tom

## Context

Game repos initialized with Claude Code need a repeatable way to go from a domain idea to a
running game without the domain model degrading into scattered, hardcoded gameplay values.
Two skill ecosystems were found and verified: `awesome-gamedev-agent-skills` (router + 67
engine/discipline/genre skills) and `claude-ontology-skill` (Stanford Ontology 101). They
had conflicts: the router bypassed any modeling phase, `prototype-fast`/`game-jam` mandate
hardcoding, and the ontology skill is TypeScript-specific.

## Decision

We will develop games **ontology-first**: a formal ontology under `ontology/` is the single
source of truth (`domain.md` canonical + `model.<lang>` derived + `instances/` data), built
by the `game-from-ontology` skill before any engine code, and updated *before* every
domain-touching change (CLAUDE.md sync rule). Procedural content is modeled as generators +
configs; externally authored content (Ink/Yarn, art) is referenced, not duplicated; engine
data assets (`.tres`, ScriptableObjects) are generated from instances. `prototype-fast`
spikes and `game-jam` builds are exempt: pre-ontology, separate branch/repo, never merged.

## Alternatives Considered

- **Code-first with extracted documentation** (e.g. ontological-documentation skills):
  rejected — reverse direction; the model would describe drift instead of preventing it.
- **Claude-Code-Game-Studios** (49 agents, 73 commands): rejected — a full repo template,
  explicitly out of scope, and heavyweight.
- **Enumerating all content as instances** (strict Ontology 101): rejected — procedural
  content can't be enumerated; generators with invariants replace it.

## Consequences

- Positive: one diffable source of truth; gameplay changes start as model changes; content
  is data the game loads, validated against the model.
- Negative: modeling ceremony before the first playable line; the ontology phase's
  definition of done (competency questions answered) gates scaffolding.
- Neutral: the Stanford skill's TypeScript examples are adapted to the target engine's
  language (upstream PR #1 makes this framing official).

## Resumption (for Agent)

- **Current state:** config complete and working; no game repo consumes it yet.
- **How to verify:** `sh scripts/check.sh` (70 skills resolve); read
  `skills/game-from-ontology/SKILL.md` for the canonical `ontology/` format.
- **Next steps:** initialize a first game repo per README usage; the ontology phase runs
  before engine selection.

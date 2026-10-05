---
name: game-from-ontology
description: >
  Use when initializing a new game repo, or whenever asked to create/design a game starting
  from a domain model. Drives ontology-first game development: build a formal ontology of the
  game domain first (Stanford Ontology 101 via the ontology skill), persist it as the single
  source of truth, then route implementation through the gamedev router skill. Start here for
  any new game project.
---

# Game From Ontology — Ontology-Driven Game Development

The ontology is the source of truth. Code, data, and content are derived from it — never the
other way around. Every domain-touching task updates the ontology first (the sync rule in
the repo's CLAUDE.md), then implementation consumes it.

## Workflow

### 1. Build the ontology (before any engine choice)

Read the `ontology` skill (`/skill:ontology`) and run its 7-step process on the **game domain**:

- **Competency questions** are gameplay questions: "What can the player do?", "What entities
  exist in the world?", "What are the win/lose conditions?", "How do entities interact?",
  "What state must persist between sessions?"
- **Classes** = game entities and concepts (Player, Enemy, Item, Level, Quest, Resource…).
- **Properties** = stats, state, and configuration; distinguish intrinsic (max HP) from
  extrinsic (current HP, position).
- **Relations** = mechanics (damages, drops, unlocks, requires, contains, spawns-in).
- **Constraints** = game rules (ranges, invariants, valid state transitions).
- **Instances** = actual game content (the specific enemies, items, levels).

The ontology skill's examples are TypeScript; the method is language-agnostic — emit the
target engine's language (step 3 decides it).

### 2. Persist it — the `ontology/` format

```
ontology/
├── domain.md      # canonical semantic layer (language-neutral)
├── model.<ext>    # typed model in the engine's language, derived from domain.md
└── instances/     # enumerable content as data files, validated against the model
```

**`domain.md`** is the canonical layer. Structured for small diffs, because the router
updates it incrementally on every task:

- Fixed sections: Scope, Competency Questions, Classes, Relations, Constraints, Generators.
- One `###` subsection per class; every class, relation, and instance has a **stable
  kebab-case ID** that never changes once referenced.
- Relations and constraints as tables (one row per fact), not prose.

**`model.<ext>`** mirrors `domain.md` in the target language — GDScript `Resource` classes,
C# types, Rust structs, TypeScript… The compiler/loader is the constraint checker.

**`instances/`** holds one data file per content family (`enemies.json`, `items.json`,
`levels.json`…), each entry keyed by its stable ID, loadable by the game at runtime.

### 3. Route implementation

Read the `router` skill (`/skill:router`). Let it pick the engine (or ask the user if the repo is empty)
and load only the minimal engine/discipline/genre skills for the task at hand. From then on,
the CLAUDE.md sync rule keeps `ontology/` updated before every domain-touching request is
implemented.

### 4. Derive, don't duplicate

- Ontology classes → engine architecture (nodes/scenes, ECS components, prefabs).
- Ontology instances → data files the game loads, not hardcoded values.
- Ontology constraints → the type system plus validation at load time.
- Gameplay change requested → update `ontology/domain.md` first, then propagate down.

## Content conventions

- **Procedural content** (`procedural-gen`, `roguelike`): runtime-generated content is not
  enumerated as instances. Model the **generator** as a class — its parameters, constraints,
  and invariants ("every dungeon has ≥1 exit") — and store generator *configs* in
  `instances/`. Seeds make output reproducible and testable against the invariants.
- **Externally authored content** (`dialogue-systems`, `visual-novel` — Ink/Yarn scripts;
  art from `create-game-assets`): the file stays authored in its own tool; the ontology
  stores the **reference** (stable ID, path, role, and the entities it involves). No
  duplication of the content itself.
- **Engine data assets** (`godot-resources` `.tres`, `unity-scriptableobjects` `.asset`):
  these are **derived artifacts generated from `instances/`**, committed but never
  hand-edited. Regenerate them when instances change.

## Exemptions

`prototype-fast` spikes and `game-jam` builds are **pre-ontology**: timeboxed, on their own
branch/repo, never merged. A spike that proves fun feeds the *ontology* (model the mechanic),
not the codebase (the spike is deleted).

## Ordering rule

Never scaffold engine code before steps 1–2 are done and validated with the user.
Competency questions answered by the model = definition of done for the ontology phase.

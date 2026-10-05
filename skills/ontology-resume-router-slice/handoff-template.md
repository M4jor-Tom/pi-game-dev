# docs/HANDOFF.md template

Rebuild it from `git log`, `ontology/README.md` status lines, `ontology/domain.md §7` D-lines and the
`game/README.md` folder table when it is missing. Every section below is required; write "none yet"
rather than dropping a section.

```markdown
# Handoff — resume here (written <date>, after the <D<n>> <slice> slice)

Read this, then `git log --oneline -8`, then `docs/ROADMAP/todo_implement.md`. Nothing else is
needed to continue; the repo is self-describing from these three.

## What this project is
<one paragraph: engine + language, ontology-first: `ontology/` is the source of truth (`domain.md`,
`instances/*.json`, `model.<ext>` loader + validator); engine code only consumes it; decisions the
sources never settled are numbered **D** entries in `domain.md §7` + `docs/ROADMAP/todo_decide.md`;
designed numbers live in `ontology/instances/generators.json#design.<topic>`>

## State of the build (all committed and pushed)
| commit | slice | what runs |
|---|---|---|
| <hash(es)> | <name> (§<section>, D<n>) | <what a player can do now, one line> |

Binaries / CI: <how builds are produced, or "none yet">

Playable now: <launch command> — <controls and what the player sees, one paragraph>

## The loop for every slice (do not skip step 1)
1. **Ontology sync** — read the §3 class + §6 generator + `instances/*.json` for the feature. Whatever
   the sources never gave: add `design.<topic>` to `generators.json`, a `**D<n> … — DECIDED <date>**`
   line in `domain.md §7`, a `[x] D<n>` line in `todo_decide.md`, a status line in `ontology/README.md`.
   If instance data can drift, add a validator rule (`c-<topic>-config`) in the model. Run the validator.
2. **Router** — `game-from-ontology` → `router` picks engine / discipline skills; read them.
3. **Code** in the folder named by the §3 section (table in `game/README.md`). Lazy version first;
   every deliberate cut gets a `ponytail:` comment **and** a line in `docs/ROADMAP/todo_implement.md`.
4. **One headless test per slice** plus the boot check. All checks are listed in `game/README.md`.
5. **Visual check** where the slice changes what the player sees.
6. Update `game/README.md`, `todo_implement.md`, this file, the player changelog, then commit and push.

## Gotchas that cost time (all verified)
- <one line each; keep every existing line, add new ones>

## Where things are
```
<folder → what lives there>
```

## Next slice (recommended order)
1. <feature (§ section, generator / class ids; `todo_implement.md` section)>
2. …
```

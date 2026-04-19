# Workbox Schema

Conventions for the Workboxes vault. Claude reads this file when creating new
workboxes, scaffolding index files, or validating frontmatter.

## Folder layout

```
Workboxes/
├── _system/                            (operating manual)
├── Inbox/                              (raw captures, flat)
├── Areas/<area>/                       (active area)
│   ├── _index.md                       (area MOC)
│   ├── Projects/<project-slug>/        (active project within area)
│   │   ├── _index.md                   (project MOC)
│   │   └── <content>.md                (notes, logs, decisions)
│   └── <area-level-note>.md            (optional; for notes not scoped to a project)
├── Archive/                            (mirrors Areas/; archived workboxes)
├── Attachments/                        (binaries; referenced via relative links)
└── Daily/                              (Obsidian daily notes)
```

## Naming

- Folders and files: `kebab-case-slugs`. Lowercase, hyphens only, no punctuation.
- Inbox item filename: `YYYY-MM-DD-HHMM-<slug>.md`. `<slug>` is derived from title or first words.
- Index files: always `_index.md` (underscore prefix sorts to top in Obsidian).

## Frontmatter — inbox item

```yaml
---
type: inbox-item
captured: 2026-04-18T14:30:00-04:00    # ISO 8601 with offset
source: dayone | drafts | web-clipper | raw | gmail | things | voice-memo | messages
source_id: <original id if applicable>
source_url: <url if applicable>
tags: []
---
```

A raw-text drop with no frontmatter is tolerated. Triage treats it as:
```yaml
type: inbox-item
captured: <file mtime, ISO 8601>
source: raw
tags: []
```

## Frontmatter — workbox content

```yaml
---
type: note | log-entry | decision | reference
created: 2026-04-18T14:30:00-04:00
source: <carried from inbox if applicable>
source_id: <carried from inbox if applicable>
tags: []
links: [[related-note]]
---
```

## Frontmatter — index file (MOC)

```yaml
---
type: area-index | project-index
status: active | dormant | archived
owner_notes: <free text; Claude updates with current focus, waiting-on items>
---
```

## MOC template — area

```markdown
---
type: area-index
status: active
owner_notes:
---

# <Area Name>

## Claude: Read this first

Projects for this area live in `Projects/`. Route inbox items here when they
relate to <area description — filled in per-area>.

## Open loops

(none)

## Projects

(none yet)

## Recent activity

(none yet)
```

## MOC template — project

```markdown
---
type: project-index
status: active
owner_notes:
---

# <Project Name>

## Claude: Read this first

(Narrative of what this project is and how its contents are organized. Grows
as the project matures.)

## Open loops

(none)

## Recent activity

(none yet)
```

## File-type conventions

- `log.md` (optional, in a project folder) — reverse-chronological log of
  threads, updates, quick notes that don't warrant their own file.
- `decisions.md` (optional, in a project folder) — one entry per decision,
  with date, context, decision, and rationale.
- `memory.md` (reserved for Phase 7) — distilled durable knowledge for a
  project. Not created in Day 1.

## Evolution rules

All structural changes follow **propose → approve → execute → log**:

1. Claude proposes the change (new area, new project, new rule, schema tweak).
2. User approves.
3. Claude executes: create/move files, update parent indices.
4. Claude appends an entry to `_system/changelog.md`.

Never make silent structural changes.

## Growth content types (Phase 6)

The `Areas/Growth/` area uses content-type subfolders instead of the
usual `Projects/` pattern. Each content type has its own frontmatter
shape.

### Folder layout

```
Areas/Growth/
├── _index.md
├── Values/<slug>.md
├── Goals/<slug>.md
├── Intentions/<period-slug>.md
├── Themes/<slug>/_index.md
├── Themes/<slug>/<reflection>.md
├── Therapy/_index.md
├── Therapy/sessions/YYYY-MM-DD.md
├── Therapy/homework.md
├── Practices/<slug>.md
└── Reviews/YYYY-MM-DD-<kind>.md
```

### Frontmatter — value

```yaml
---
type: value
created: 2026-04-19
status: active           # active | archived
aliases: []              # alternate names for link-driven surfacing
---
```

### Frontmatter — goal

```yaml
---
type: goal
created: 2026-04-19
horizon: 2026-Q4         # rough target period, not a hard deadline
status: active           # active | achieved | abandoned | paused
linked_values: []        # wikilinks, e.g. [[health]]
---
```

### Frontmatter — intention

```yaml
---
type: intention
period: 2026-Q2          # required
created: 2026-04-19
status: active           # active | completed
---
```

### Frontmatter — theme

Theme MOC (`Themes/<slug>/_index.md`):
```yaml
---
type: theme
created: 2026-04-19
status: active           # active | dormant | resolved
---
```

Reflection within a theme folder:
```yaml
---
type: reflection
created: 2026-04-19
---
```

### Frontmatter — therapy session

```yaml
---
type: therapy-session
date: 2026-04-19
modality: individual     # free-text: individual | couples | group | ...
---
```

### Frontmatter — therapy homework

Single long-lived document at `Therapy/homework.md`:
```yaml
---
type: therapy-homework
created: 2026-04-19
---
```
Body has `## Active` and `## Completed` sections.

### Frontmatter — practice

```yaml
---
type: practice
created: 2026-04-19
cadence: daily           # daily | weekly | situational
status: active           # active | paused | retired
linked_values: []
---
```

### Frontmatter — review

```yaml
---
type: review
date: 2026-04-19
kind: weekly             # daily | weekly | monthly | quarterly | annual
---
```

### Naming rules

- Kebab-case slugs for `value`, `goal`, `practice`, `theme`.
- ISO date (`YYYY-MM-DD`) for `therapy-session` and `review`.
- Period slug for `intention`: `2026-Q2`, `2026-04`, or `2026`.

### Append exception

Normal triage moves inbox items whole. For Growth only, triage may
instead **append** a short bullet to an existing file and `git rm` the
original inbox item. This applies only to these target types:

| Target type | Append section |
|---|---|
| `goal` | `## Activity` (create if absent) |
| `value` | `## Activity` (create if absent) |
| `practice` | `## Activity` (create if absent) |
| `therapy-homework` | `## Active` (create if absent) |

`therapy-session` and `review` are dated documents and never accept
appends. Reflections within a theme are created as new files, not
appended.

### Status lifecycle

| Type | Transitions |
|---|---|
| value | active → archived (rare, via review) |
| goal | active → achieved / abandoned / paused; paused → active |
| intention | active → completed at period end |
| theme | active → dormant → resolved |
| practice | active → paused → retired |

Reviews are the primary mechanism for proposing status transitions;
the user approves each.
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
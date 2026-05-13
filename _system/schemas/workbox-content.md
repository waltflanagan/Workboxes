# Schema: Workbox Content

## Frontmatter — content file

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

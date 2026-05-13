# Workbox Schema

Conventions for the Workboxes vault. Claude reads this file when creating new
workboxes, scaffolding index files, or validating frontmatter.

## Schema map

Pull in only the schema file(s) relevant to your task:

| Schema | File | Use when… |
|---|---|---|
| Inbox item | [`schemas/inbox-item.md`](schemas/inbox-item.md) | triaging captures, validating raw drops |
| Workbox content & MOCs | [`schemas/workbox-content.md`](schemas/workbox-content.md) | creating notes, log entries, decisions, or index files |
| School email | [`schemas/school-email.md`](schemas/school-email.md) | running `/school-email-triage` |
| Resources / People | [`schemas/resources.md`](schemas/resources.md) | creating or updating `Resources/People/` entries |
| Growth content | [`schemas/growth.md`](schemas/growth.md) | creating values, goals, intentions, themes, practices, therapy notes, or reviews |

## Folder layout

```
Workboxes/
├── _system/                            (operating manual)
│   └── schemas/                        (per-type schema files)
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

## Evolution rules

All structural changes follow **propose → approve → execute → log**:

1. Claude proposes the change (new area, new project, new rule, schema tweak).
2. User approves.
3. Claude executes: create/move files, update parent indices.
4. Claude appends an entry to `_system/changelog.md`.

Never make silent structural changes.

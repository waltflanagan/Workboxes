# Schema: Growth Content Types (Phase 6)

The `Areas/Growth/` area uses content-type subfolders instead of the
usual `Projects/` pattern.

## Folder layout

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

## Frontmatter — value

```yaml
---
type: value
created: 2026-04-19
status: active           # active | archived
aliases: []              # alternate names for link-driven surfacing
---
```

## Frontmatter — goal

```yaml
---
type: goal
created: 2026-04-19
horizon: 2026-Q4         # rough target period, not a hard deadline
status: active           # active | achieved | abandoned | paused
linked_values: []        # wikilinks, e.g. [[health]]
---
```

## Frontmatter — intention

```yaml
---
type: intention
period: 2026-Q2          # required
created: 2026-04-19
status: active           # active | completed
---
```

## Frontmatter — theme

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

## Frontmatter — therapy session

```yaml
---
type: therapy-session
date: 2026-04-19
modality: individual     # free-text: individual | couples | group | ...
---
```

## Frontmatter — therapy homework

Single long-lived document at `Therapy/homework.md`:
```yaml
---
type: therapy-homework
created: 2026-04-19
---
```
Body has `## Active` and `## Completed` sections.

## Frontmatter — practice

```yaml
---
type: practice
created: 2026-04-19
cadence: daily           # daily | weekly | situational
status: active           # active | paused | retired
linked_values: []
---
```

## Frontmatter — review

```yaml
---
type: review
date: 2026-04-19
kind: weekly             # daily | weekly | monthly | quarterly | annual
---
```

## Naming rules

- Kebab-case slugs for `value`, `goal`, `practice`, `theme`.
- ISO date (`YYYY-MM-DD`) for `therapy-session` and `review`.
- Period slug for `intention`: `2026-Q2`, `2026-04`, or `2026`.

## Append exception

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

## Status lifecycle

| Type | Transitions |
|---|---|
| value | active → archived (rare, via review) |
| goal | active → achieved / abandoned / paused; paused → active |
| intention | active → completed at period end |
| theme | active → dormant → resolved |
| practice | active → paused → retired |

Reviews are the primary mechanism for proposing status transitions;
the user approves each.

# Schema

Conventions for the Workboxes vault. Read the section relevant to your task.

---

## Naming

- All folders and files: `kebab-case`. Lowercase, hyphens only.
- Inbox filenames: `YYYY-MM-DD-HHMM-<slug>.md`
- Index files: `AGENTS.md` (underscore prefix sorts to top in Obsidian)

---

## Inbox item

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

A raw drop with no frontmatter is tolerated. Treat it as `source: raw`, `captured: <file mtime>`.

---

## Workbox content

### Content file

```yaml
---
type: note | log-entry | decision | reference
created: 2026-04-18T14:30:00-04:00
source: <carried from inbox>
source_id: <carried from inbox>
tags: []
links: [[related-note]]
---
```

### Index file (MOC)

```yaml
---
type: area-index | project-index
status: active | dormant | archived
owner_notes: <free text; Claude updates with current focus and waiting-on items>
---
```

### Area MOC template

```markdown
---
type: area-index
status: active
owner_notes:
---

# <Area Name>

## Claude: Read this first

Projects for this area live in `Projects/`. Route inbox items here when they
relate to <area description>.

## Open loops

(none)

## Projects

(none yet)

## Recent activity

(none yet)
```

### Project MOC template

```markdown
---
type: project-index
status: active
owner_notes:
---

# <Project Name>

## Claude: Read this first

(Narrative of what this project is and how its contents are organized.)

## Open loops

(none)

## Recent activity

(none yet)
```

### File-type conventions

- `log.md` — reverse-chronological log of threads, updates, and quick notes.
- `decisions.md` — one entry per decision: date, context, decision, rationale.

---

## School email

Created by `/school-email-triage`. Filed directly into child folders — never lands in `Inbox/`.

```yaml
---
type: school-email
created: 2026-05-07T14:30:00-04:00
source: gmail
source_id: <gmail message id>
source_url: <gmail permalink>
child: oscar | nigel | avery | all
email_type: newsletter | school-info | district-info
sender: <from address>
subject: <email subject line>
has_events: true | false
has_actions: true | false
tags: []
---
```

Body structure:

```markdown
## Events

- YYYY-MM-DD — <event description> [-> calendar]

## Action items

- <task description> — due YYYY-MM-DD [-> Things]

## Full email

<full email body, lightly formatted>
```

Omit `## Events` if none. Omit `## Action items` if none. Always include `## Full email`.

---

## Resources / People

Cross-area reference content lives under `Resources/` at the vault root — never inside `Areas/`.

### Folder layout

```
Resources/
├── AGENTS.md
└── People/
    ├── AGENTS.md
    └── <firstname-lastname>.md
```

### Person frontmatter

```yaml
---
type: person
name: Full Name
role: therapist | doctor | teacher | coach | vendor | other
org:
phone:
email:
insurance: in-network | OON | self-pay | N/A | unknown
areas: []    # vault areas this person is relevant to
tags: []
created: 2026-05-08
---
```

### Person body structure

```markdown
## Contact

| Field | Value |
|---|---|
| Phone | |
| Email | |
| Insurance | |

## Background

(Specialty, approach, notes on fit, referral source.)

## Interactions

Reverse-chronological. One bullet per contact.

- YYYY-MM-DD — <what happened>

## Links

- [[Areas/...]]
```

### Rules

- Filename: `<firstname-lastname>.md`. Unknown last name: `<firstname-descriptor>.md`.
- Always search `Resources/People/` before creating — no duplicates.
- Update `## Interactions` on every new contact.
- After adding a person, update `Resources/People/AGENTS.md` alphabetically.
- Link *to* Resources from area notes; never move a person file into an area.

---

## Growth content

`Areas/Growth/` uses content-type subfolders instead of the usual `Projects/` pattern.

### Folder layout

```
Areas/Growth/
├── AGENTS.md
├── Values/<slug>.md
├── Goals/<slug>.md
├── Intentions/<period-slug>.md
├── Themes/<slug>/AGENTS.md
├── Themes/<slug>/<reflection>.md
├── Therapy/AGENTS.md
├── Therapy/sessions/YYYY-MM-DD.md
├── Therapy/homework.md
├── Practices/<slug>.md
└── Reviews/YYYY-MM-DD-<kind>.md
```

### Frontmatter by type

**value**
```yaml
---
type: value
created: 2026-04-19
status: active | archived
aliases: []
---
```

**goal**
```yaml
---
type: goal
created: 2026-04-19
horizon: 2026-Q4
status: active | achieved | abandoned | paused
linked_values: []
---
```

**intention**
```yaml
---
type: intention
period: 2026-Q2
created: 2026-04-19
status: active | completed
---
```

**theme** (MOC at `Themes/<slug>/AGENTS.md`)
```yaml
---
type: theme
created: 2026-04-19
status: active | dormant | resolved
---
```

**reflection** (file within a theme folder)
```yaml
---
type: reflection
created: 2026-04-19
---
```

**therapy session**
```yaml
---
type: therapy-session
date: 2026-04-19
modality: individual
---
```

**practice**
```yaml
---
type: practice
created: 2026-04-19
cadence: daily | weekly | situational
status: active | paused | retired
linked_values: []
---
```

**review**
```yaml
---
type: review
date: 2026-04-19
kind: daily | weekly | monthly | quarterly | annual
---
```

### Naming rules

- Slugs (value, goal, practice, theme): kebab-case.
- Intentions: period slug — `2026-Q2`, `2026-04`, or `2026`.
- Therapy sessions and reviews: ISO date `YYYY-MM-DD`.

### Triage append exception

For Growth only, triage may append a short bullet to an existing file rather than moving the inbox item whole. Applies only to:

| Target type | Append section |
|---|---|
| goal, value, practice | `## Activity` (create if absent) |
| therapy-homework | `## Active` (create if absent) |

`therapy-session` and `review` are dated documents — never append. Reflections within a theme are always new files.

### Status lifecycle

| Type | Transitions |
|---|---|
| value | active → archived |
| goal | active → achieved / abandoned / paused; paused → active |
| intention | active → completed at period end |
| theme | active → dormant → resolved |
| practice | active → paused → retired |

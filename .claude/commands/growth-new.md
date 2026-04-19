---
description: Create a new Growth content file (value, goal, intention, practice, theme, or therapy session).
---

You are creating a new Growth content file in the Workboxes vault.
Follow this procedure.

## 1. Determine the type

The command invocation may include a type argument
(`/growth-new value`). If so, use it. If the argument is missing or
unrecognized, ask:

> "Which type? value / goal / intention / practice / theme /
> therapy-session"

Therapy homework is a single long-lived document and is not created
via this command (it already exists at `Therapy/homework.md`). If the
user asks to create one, tell them to open `Therapy/homework.md`
directly.

Reviews are not created via this command — they come from the
`/review-*` commands.

## 2. Load schema

Read `_system/workbox-schema.md` — specifically the **Growth content
types** section — to get the exact frontmatter shape for the chosen
type.

## 3. Prompt for the slug

Ask:

> "What's the slug? (kebab-case; example: `courage` for a value,
> `run-half-marathon` for a goal, `2026-Q2` for an intention,
> `morning-meditation` for a practice, `perfectionism` for a theme,
> today's date for a therapy session)"

Validate:
- **value, goal, practice, theme:** kebab-case, lowercase, hyphens
  only. Reject and re-ask on invalid input.
- **intention:** period slug (`YYYY`, `YYYY-MM`, or `YYYY-QN`).
  Reject and re-ask if it doesn't match.
- **therapy-session:** ISO date `YYYY-MM-DD`. Offer today's date as
  the default.

Check for collisions:
- Value/goal/practice: does `<type-plural>/<slug>.md` already exist?
- Intention: does `Intentions/<slug>.md` already exist?
- Theme: does `Themes/<slug>/` already exist?
- Therapy session: does `Therapy/sessions/<slug>.md` already exist?

If it exists, stop and tell the user the file already exists.

## 4. Prompt for type-specific fields

Ask only the fields the schema requires and that aren't derivable:

- **value:** aliases (optional list, can be empty — ask "Any
  aliases? (comma-separated, or blank)")
- **goal:** horizon (ask "What's the rough horizon? e.g. `2026-Q4`,
  or `no-horizon`"), linked_values (ask "Any values this ties to?
  (comma-separated slugs, or blank)")
- **intention:** none beyond the period (already captured in slug).
- **theme:** none.
- **therapy-session:** modality (ask "Modality? (free-text; default:
  `individual`)")
- **practice:** cadence (ask "Cadence? daily / weekly / situational"),
  linked_values (ask "Any values this ties to? (comma-separated
  slugs, or blank)")

## 5. Create the file

Write the file with:

- Full frontmatter per `_system/workbox-schema.md`.
- `created` or `date` set to today (ISO 8601).
- `status` set to `active` where applicable.
- A seeded body with the headings the type expects:

### Value body template

```markdown
# <Title derived from slug>

## What this means to me

(narrative)

## Examples / stories

(examples)

## How I'll know I'm living it

(indicators)

## Activity

(none yet)
```

### Goal body template

```markdown
# <Title derived from slug>

## Why this goal

(narrative)

## What success looks like

(concrete picture)

## Linked practices and projects

(none yet)

## Activity

(none yet)
```

### Intention body template

```markdown
# Intention — <period>

## Focus points

1.
2.
3.

## Linked values and goals

(none yet)
```

### Theme body template

(for `Themes/<slug>/_index.md`)

```markdown
---
type: theme
created: <today>
status: active
---

# <Title derived from slug>

## What this theme is

(narrative)

## Reflections

(none yet)
```

### Therapy session body template

```markdown
# Therapy — <date>

## Session notes

(to fill in)

## Insights

(to fill in)

## Homework

- (none assigned) — or link to items added to `[[homework]]`.
```

### Practice body template

```markdown
# <Title derived from slug>

## What this is

(narrative)

## Why

(motivation)

## How to do it

(mechanics)

## Tracking

(how I know I did it)

## Activity

(none yet)
```

## 6. Append to parent MOC

For each type, append a line under the parent's `## Recent activity`
section in `Areas/Growth/<Type>/_index.md`:

```
- YYYY-MM-DD — Created <slug>
```

## 7. Commit

```bash
cd /Users/msimons/Code/Workboxes && \
git add Areas/Growth && \
git commit -m "Growth: create <type> <slug>"
```

## Tone

Terse. One prompt at a time. Consult `_system/voice.md`.

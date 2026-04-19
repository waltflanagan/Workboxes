# Workboxes Phase 6a Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add the Growth area (values, goals, intentions, themes, therapy, practices, reviews) to the Workboxes vault, plus the slash commands and schema extensions needed to capture, triage, and review growth content.

**Architecture:** Extend the existing vault. New `Areas/Growth/` Area with seven content subfolders. Two new `_system/` files (`growth-prompts.md`, `growth-surfacing.md`). Four new slash commands (`/growth-new`, `/review-daily`, `/review-weekly`, `/review-quarterly`). One existing command modified (`/triage`). Schema appended to `workbox-schema.md`. README updated.

**Tech Stack:** Markdown + YAML frontmatter. Obsidian vault conventions. Claude Code slash commands. No code, no tests beyond walkthrough verification — this is a Claude-driven content system.

**Spec:** `docs/superpowers/specs/2026-04-19-workboxes-phase-6-growth-design.md`

---

## File Structure

**Create:**
- `Areas/Growth/_index.md` — area MOC
- `Areas/Growth/Values/_index.md` — values subfolder MOC
- `Areas/Growth/Goals/_index.md` — goals subfolder MOC
- `Areas/Growth/Intentions/_index.md` — intentions subfolder MOC
- `Areas/Growth/Themes/_index.md` — themes subfolder MOC
- `Areas/Growth/Therapy/_index.md` — therapy subfolder MOC
- `Areas/Growth/Therapy/sessions/.gitkeep` — keep empty folder tracked
- `Areas/Growth/Therapy/homework.md` — long-lived homework doc
- `Areas/Growth/Practices/_index.md` — practices subfolder MOC
- `Areas/Growth/Reviews/_index.md` — reviews subfolder MOC
- `_system/growth-prompts.md` — review prompt content
- `_system/growth-surfacing.md` — surfacing rules for triage
- `.claude/commands/growth-new.md` — `/growth-new <type>` command
- `.claude/commands/review-daily.md` — `/review-daily` command
- `.claude/commands/review-weekly.md` — `/review-weekly` command
- `.claude/commands/review-quarterly.md` — `/review-quarterly` command

**Modify:**
- `_system/workbox-schema.md` — append Growth content types section
- `_system/README.md` — add Growth to Active areas list; add new operating files
- `_system/changelog.md` — append Phase 6a entry
- `.claude/commands/triage.md` — add Route-to-Growth disposition, surfacing step, append execution

---

## Task 1: Scaffold `Areas/Growth/` folder with all MOCs

**Files:**
- Create: `Areas/Growth/_index.md`
- Create: `Areas/Growth/Values/_index.md`
- Create: `Areas/Growth/Goals/_index.md`
- Create: `Areas/Growth/Intentions/_index.md`
- Create: `Areas/Growth/Themes/_index.md`
- Create: `Areas/Growth/Therapy/_index.md`
- Create: `Areas/Growth/Therapy/sessions/.gitkeep`
- Create: `Areas/Growth/Therapy/homework.md`
- Create: `Areas/Growth/Practices/_index.md`
- Create: `Areas/Growth/Reviews/_index.md`

- [ ] **Step 1: Create `Areas/Growth/_index.md`**

```markdown
---
type: area-index
status: active
owner_notes:
---

# Growth

## Claude: Read this first

This area holds growth, values, goals, and therapy-related content. It has
internal substructure (not the usual `Projects/` pattern):

- `Values/` — what I care about, named and described.
- `Goals/` — outcomes I'm working toward.
- `Intentions/` — focus points for a period (quarter, month).
- `Themes/` — recurring growth topics, each with sub-reflections.
- `Therapy/` — session notes and homework tracking.
- `Practices/` — the things I do regularly to grow.
- `Reviews/` — dated review files (daily/weekly/quarterly).

Route inbox items here when they relate to self-understanding, values-in-
action, goals, therapy, or personal growth practices. Growth supports an
**append exception** to the items-move-whole rule: short progress notes,
reflections, or homework updates can be appended to an existing goal,
value, practice, or therapy-homework file rather than moved whole. See
`_system/workbox-schema.md` for the rules.

## Open loops

(none)

## Projects

(Growth uses content-type subfolders instead of Projects. See above.)

## Recent activity

(none yet)
```

- [ ] **Step 2: Create `Areas/Growth/Values/_index.md`**

```markdown
---
type: area-index
status: active
owner_notes:
---

# Values

## Claude: Read this first

One file per value in this folder, kebab-case slug (e.g. `courage.md`).
Values use `type: value` frontmatter with `status`, `aliases`, and
`linked_values` (see `_system/workbox-schema.md` Growth section).

When triage surfaces a value slug or alias in a captured item, suggest
adding `[[<value>]]` to the item body rather than routing the item here.
Values are reference documents; they accumulate activity via appends,
not by collecting inbox items.

## Open loops

(none)

## Recent activity

(none yet)
```

- [ ] **Step 3: Create `Areas/Growth/Goals/_index.md`**

```markdown
---
type: area-index
status: active
owner_notes:
---

# Goals

## Claude: Read this first

One file per goal in this folder, kebab-case slug (e.g.
`run-half-marathon.md`). Goals use `type: goal` frontmatter with
`horizon`, `status`, and `linked_values` (see
`_system/workbox-schema.md`).

Progress notes and small updates can be appended to a goal file's
`## Activity` section via triage (the append exception). Larger plans
or sub-projects should still become real projects in the relevant Area.

## Open loops

(none)

## Recent activity

(none yet)
```

- [ ] **Step 4: Create `Areas/Growth/Intentions/_index.md`**

```markdown
---
type: area-index
status: active
owner_notes:
---

# Intentions

## Claude: Read this first

One file per period in this folder. Period slugs: `2026-Q2`, `2026-04`,
`2026`, etc. Intentions use `type: intention` frontmatter with `period`
and `status`.

An intention holds 1–5 focus points for the period, linked to values
and goals. Reviews (quarterly especially) archive the current intention
and draft the next one.

## Open loops

(none)

## Recent activity

(none yet)
```

- [ ] **Step 5: Create `Areas/Growth/Themes/_index.md`**

```markdown
---
type: area-index
status: active
owner_notes:
---

# Themes

## Claude: Read this first

Each theme is a subfolder (e.g. `perfectionism/`) containing an
`_index.md` (theme MOC) and individual reflection files. Theme MOC uses
`type: theme`; reflections use `type: reflection`.

When triage encounters a captured reflection that matches an existing
theme slug, route it under that theme's folder. For a reflection that
doesn't match, propose a new theme (same pattern as "New project" in
regular triage).

## Open loops

(none)

## Recent activity

(none yet)
```

- [ ] **Step 6: Create `Areas/Growth/Therapy/_index.md`**

```markdown
---
type: area-index
status: active
owner_notes:
---

# Therapy

## Claude: Read this first

Two kinds of content here:

- `sessions/YYYY-MM-DD.md` — one file per session, `type: therapy-session`.
  Dated documents — never edited after the fact.
- `homework.md` — single long-lived document, `type: therapy-homework`.
  Has `## Active` and `## Completed` sections. Accepts appends via
  triage.

When triage sees language like "my therapist said" or "between sessions",
propose appending to `homework.md` or linking from it.

## Open loops

(none)

## Recent activity

(none yet)
```

- [ ] **Step 7: Create `Areas/Growth/Therapy/sessions/.gitkeep`**

Empty file. Purpose: ensure `sessions/` exists in git before the first
session file is created.

```
```

- [ ] **Step 8: Create `Areas/Growth/Therapy/homework.md`**

```markdown
---
type: therapy-homework
created: 2026-04-19
---

# Therapy Homework

Running list of between-session homework. Triage appends new items
under `## Active`. Mark items done by moving them to `## Completed`
with a completion date.

## Active

(none)

## Completed

(none)
```

- [ ] **Step 9: Create `Areas/Growth/Practices/_index.md`**

```markdown
---
type: area-index
status: active
owner_notes:
---

# Practices

## Claude: Read this first

One file per practice in this folder, kebab-case slug (e.g.
`morning-meditation.md`). Practices use `type: practice` frontmatter
with `cadence`, `status`, and `linked_values`.

Progress notes and "I did this today" bullets can be appended to a
practice file's `## Activity` section via triage (the append
exception).

## Open loops

(none)

## Recent activity

(none yet)
```

- [ ] **Step 10: Create `Areas/Growth/Reviews/_index.md`**

```markdown
---
type: area-index
status: active
owner_notes:
---

# Reviews

## Claude: Read this first

One file per review run, named `YYYY-MM-DD-<kind>.md` where `<kind>`
is `daily`, `weekly`, `monthly` (deferred), `quarterly`, or `annual`
(deferred). All reviews use `type: review` frontmatter with `date`
and `kind`.

Reviews are **always written** even if the user has nothing to say —
an empty-body file is cheaper than a silent gap. See
`_system/growth-prompts.md` for each cadence's prompts and what to
load.

## Open loops

(none)

## Recent activity

(none yet)
```

- [ ] **Step 11: Verify all files exist**

Run:
```bash
ls -la Areas/Growth/ Areas/Growth/Values/ Areas/Growth/Goals/ \
  Areas/Growth/Intentions/ Areas/Growth/Themes/ Areas/Growth/Therapy/ \
  Areas/Growth/Therapy/sessions/ Areas/Growth/Practices/ \
  Areas/Growth/Reviews/
```

Expected: each directory shows the `_index.md` (or `homework.md` /
`.gitkeep`) listed above, no other files.

- [ ] **Step 12: Commit**

```bash
cd /Users/msimons/Code/Workboxes && \
git add Areas/Growth && \
git commit -m "Phase 6a: scaffold Areas/Growth/ with all subfolder MOCs

Seven content subfolders (Values, Goals, Intentions, Themes, Therapy,
Practices, Reviews), each with an _index.md MOC. Therapy also gets
homework.md and an empty sessions/ folder."
```

---

## Task 2: Extend `_system/workbox-schema.md` with Growth content types

**Files:**
- Modify: `_system/workbox-schema.md` (append a new section at the end)

- [ ] **Step 1: Append Growth section**

Append this content to the end of `_system/workbox-schema.md` (after
the existing "Evolution rules" section):

````markdown

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
````

- [ ] **Step 2: Verify append worked**

Run:
```bash
grep -n "## Growth content types" _system/workbox-schema.md
```

Expected: one match, line number > 145 (the Evolution rules section
ends around line 145 in the original).

- [ ] **Step 3: Commit**

```bash
cd /Users/msimons/Code/Workboxes && \
git add _system/workbox-schema.md && \
git commit -m "Phase 6a: extend workbox-schema with Growth content types

Documents folder layout, frontmatter for all seven Growth content
types, naming rules, append exception, and status lifecycle."
```

---

## Task 3: Create `_system/growth-prompts.md`

**Files:**
- Create: `_system/growth-prompts.md`

- [ ] **Step 1: Write the full prompts file**

```markdown
# Growth Review Prompts

Prompts and load rules for the `/review-*` slash commands. Each command
reads its own section here.

Reviews are **always written** — if the user has nothing to say, the
review file is still created with a short body (e.g. "nothing came up
today"). Skipped-review files become evidence later.

---

## Daily

**Purpose:** quick morning check-in. Surfaces today's one-pagers.

**What to load (before asking):**

- `Areas/Growth/Intentions/` — the intention file whose `period`
  contains today's date, if one exists.
- `Areas/Growth/Practices/*.md` — randomly pick one active practice
  to highlight.
- `Areas/Growth/Goals/*.md` — randomly pick one active goal to
  highlight.

**Prompts (ask one at a time):**

1. "Today's intention for this period is: `<quote the intention's focus
   points>`. Anything you want to hold in mind today?"
2. "One active practice to remember: `<practice name>`. Doing it today?
   Anything to note?"
3. "One active goal to remember: `<goal name>`. Any move you can make
   today?"

**Writes:** `Areas/Growth/Reviews/YYYY-MM-DD-daily.md` with the user's
answers.

---

## Weekly

**Purpose:** Sunday (or whichever day the user prefers) pass over the
week. Surfaces intentions, active goals, and therapy homework.

**What to load:**

- Current-period intention file.
- All active goals (`frontmatter.status == active`).
- For each active goal, tail of `## Activity` section (last ~10 lines
  if present).
- `Areas/Growth/Therapy/homework.md` — the full `## Active` section.
- `Areas/Growth/Reviews/` — list of daily reviews from the past seven
  days (filenames only).

**Prompts:**

1. "This week's intention: `<quote>`. How did the week feel against it?"
2. "Goals touched this week: `<list active goals with recent activity>`.
   Any to mark achieved / abandoned / paused? Anything to add to
   `## Activity` for any of them?"
3. "Active therapy homework: `<list from homework.md>`. Any to move to
   Completed? Anything new to add?"
4. "Anything from this week's daily reviews that wants a longer
   reflection or a new theme?"

**Writes:** `Areas/Growth/Reviews/YYYY-MM-DD-weekly.md`.

Any proposed status transitions (e.g. goal → achieved) or new content
(e.g. new reflection under a theme) run through the normal
propose-approve-execute pattern as separate steps after the review
body is written.

---

## Quarterly

**Purpose:** end-of-quarter reset. Archive current intention, draft
next, check in on values.

**What to load:**

- Current-period intention (by date — the one whose `period` contains
  today or ends today).
- All values (`Values/*.md`), status `active` or `archived`.
- All active goals, with full `## Activity` sections.
- All active themes, with reflection count per theme.

**Prompts:**

1. "Closing intention: `<quote current intention>`. Mark `status:
   completed` and archive? Any final notes for the body before we
   close it?"
2. "Drafting next quarter's intention. Slug will be `<next-period>`.
   1–5 focus points?"
3. "Values check-in: for each active value, does it still feel true?
   Any to archive? Any new values emerging that deserve their own
   file?"
4. "Goals retrospective: `<list active goals>`. Any to mark achieved,
   abandoned, or paused going into next quarter?"
5. "Themes landscape: `<list active themes with reflection counts>`.
   Any to mark dormant or resolved?"

**Writes:** `Areas/Growth/Reviews/YYYY-MM-DD-quarterly.md`.

Status transitions and the new intention file are created after the
review body, via approve-then-execute.

---

## Monthly

<!-- deferred to Phase 6b -->

---

## Annual

<!-- deferred to Phase 6b -->
```

- [ ] **Step 2: Verify file created**

Run:
```bash
ls -la _system/growth-prompts.md && \
grep -c "^## " _system/growth-prompts.md
```

Expected: file exists; grep returns `5` (Daily, Weekly, Quarterly,
Monthly, Annual).

- [ ] **Step 3: Commit**

```bash
cd /Users/msimons/Code/Workboxes && \
git add _system/growth-prompts.md && \
git commit -m "Phase 6a: add growth-prompts.md for review slash commands

Day-1 sections: daily, weekly, quarterly. Monthly and annual stubbed
for Phase 6b."
```

---

## Task 4: Create `_system/growth-surfacing.md`

**Files:**
- Create: `_system/growth-surfacing.md`

- [ ] **Step 1: Write the surfacing rules file**

```markdown
# Growth Surfacing Rules

Rules that `/triage` uses to suggest Growth links and dispositions.
Surfacing is **suggestive only** — triage still routes items to their
best-fit workbox; Growth links are added to the item body or the
routing rationale, not used as an alternate destination (except when
the disposition is explicitly Route-to-Growth).

## When to apply

During `/triage` step 1 (load context), read this file. For each inbox
item during step 3 (propose a disposition), run the scan patterns
below before settling on a proposal.

## Scan patterns (Day 1: simple slug/alias matching)

**1. Value match**

For each file in `Areas/Growth/Values/*.md`:
- Read the file's frontmatter (specifically `aliases`) and its filename
  slug.
- If the inbox item body contains the slug or any alias as a word, the
  match fires.

On match: suggest `[[<value-slug>]]` be added to the item body. The
item still routes normally via its best-fit disposition.

**2. Goal match**

For each file in `Areas/Growth/Goals/*.md`:
- Read the filename slug.
- If the inbox item body contains the slug or any prominent word from
  the goal's `# <title>` heading, the match fires.

On match: consider the **append** disposition if the item reads like a
short progress note (≤ 3 sentences, no distinct new topic). Otherwise
suggest `[[<goal-slug>]]` in the item body while routing elsewhere.

**3. Practice match**

Same as goal match, against `Areas/Growth/Practices/*.md`. Short "I
did this today" notes are strong append candidates.

**4. Therapy-homework match**

If the item body contains any of: "my therapist", "my therapist said",
"between sessions", "homework" (case-insensitive), the match fires.

On match: suggest **append** to `Areas/Growth/Therapy/homework.md`
under `## Active`, or suggest routing to a therapy session file if
the item is clearly tied to a specific session (has a session date).

**5. Theme match**

For each folder in `Areas/Growth/Themes/*/`:
- Read the folder slug.
- If the inbox item body contains the slug, the match fires.

On match: propose Route-to-Growth with destination
`Areas/Growth/Themes/<slug>/` as a new reflection file. If no theme
slug matches but the item reads reflective/introspective, propose
creating a new theme (requires a proposed slug and approval).

## Match priority

If multiple scans fire, present all matches in the rationale. The user
picks which link(s) to keep. Theme route proposals take precedence
over value/goal/practice link suggestions — a reflective item
belongs under Themes, not linked from a value.

## Deferred (Phase 6b)

- Emotion/feeling lexicons to match themes without a slug match.
- Multi-word phrase matching ("I want to run a marathon" → goal
  `run-half-marathon.md`).
- Fuzzy matching on slugs (`meditation` matching
  `morning-meditation`).

If Day-1 matching feels thin after real use, propose a rule addition
via the normal propose-approve-execute flow and append to this file.
```

- [ ] **Step 2: Verify file created**

Run:
```bash
ls -la _system/growth-surfacing.md && \
grep -c "^### Frontmatter\|^### Scan\|^\*\*[0-9]\." _system/growth-surfacing.md
```

Expected: file exists; grep returns `5` (one per scan pattern).

- [ ] **Step 3: Commit**

```bash
cd /Users/msimons/Code/Workboxes && \
git add _system/growth-surfacing.md && \
git commit -m "Phase 6a: add growth-surfacing.md with Day-1 slug/alias scan rules

Five scan patterns: value, goal, practice, therapy-homework, theme.
Lexicons and fuzzy matching deferred to Phase 6b."
```

---

## Task 5: Create `/growth-new` slash command

**Files:**
- Create: `.claude/commands/growth-new.md`

- [ ] **Step 1: Write the command file**

```markdown
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
```

- [ ] **Step 2: Verify command file created**

Run:
```bash
ls -la .claude/commands/growth-new.md && \
head -3 .claude/commands/growth-new.md
```

Expected: file exists; head shows the YAML frontmatter with
`description:`.

- [ ] **Step 3: Commit**

```bash
cd /Users/msimons/Code/Workboxes && \
git add .claude/commands/growth-new.md && \
git commit -m "Phase 6a: add /growth-new slash command

Prompts for slug + type-specific fields, validates format, checks for
collisions, seeds body with schema-appropriate headings."
```

---

## Task 6: Create `/review-daily` slash command

**Files:**
- Create: `.claude/commands/review-daily.md`

- [ ] **Step 1: Write the command file**

```markdown
---
description: Run the daily Growth review — quick morning check-in on today's intention, one practice, one goal.
---

You are running the daily Growth review for the Workboxes vault.

## 1. Load context

Read these files in order:

1. `_system/README.md`
2. `_system/voice.md`
3. `_system/growth-prompts.md` — specifically the `## Daily` section.

## 2. Check for an existing review today

Run: `ls -1 Areas/Growth/Reviews/$(date +%Y-%m-%d)-daily.md 2>/dev/null`

If the file already exists, ask the user: "A daily review already
exists for today. Append to it, overwrite it, or cancel?"
- **Append:** add a new `## Additional pass (<HH:MM>)` section.
- **Overwrite:** replace the file.
- **Cancel:** stop.

If not, proceed.

## 3. Load what the Daily section asks for

Per `_system/growth-prompts.md` ## Daily:

- Find the current-period intention: list `Intentions/*.md` and pick
  the one whose `period` frontmatter contains today's date. (If
  multiple match, prefer the narrower period — `2026-04` over `2026`.)
  If none match, note "no current intention" and skip prompt 1.
- Pick one random active practice from `Practices/*.md` (where
  `status: active`). If none exist, skip prompt 2.
- Pick one random active goal from `Goals/*.md` (where
  `status: active`). If none exist, skip prompt 3.

## 4. Walk the prompts

Ask each prompt from `growth-prompts.md` ## Daily, one at a time.
Quote the intention's focus points, the practice name, and the goal
name in your prompts (don't make the user go look them up).

Let the user skip any prompt ("skip", "nothing"). Record the user's
answers.

## 5. Write the review file

Create `Areas/Growth/Reviews/<today>-daily.md`:

```markdown
---
type: review
date: <today ISO>
kind: daily
---

# Daily Review — <today>

## Intention for this period
<quoted focus points>

User: <answer, or "nothing">

## Practice highlighted
<practice name>

User: <answer, or "nothing">

## Goal highlighted
<goal name>

User: <answer, or "nothing">
```

If every answer was "nothing", still write the file with a top-level
body line: "Nothing came up today." Then write the sections as above.

## 6. Handle follow-ups

If any answer included a status transition request (e.g. "mark that
goal achieved") or new content, run the normal
propose-approve-execute pattern:

1. Propose the change.
2. User approves.
3. Execute (edit the target file, append to MOC's recent activity).

## 7. Commit

```bash
cd /Users/msimons/Code/Workboxes && \
git add Areas/Growth && \
git commit -m "Growth: daily review <today>"
```

## Tone

Terse. One prompt at a time. No trailing summary.
```

- [ ] **Step 2: Verify**

Run:
```bash
ls -la .claude/commands/review-daily.md && \
head -3 .claude/commands/review-daily.md
```

Expected: file exists; frontmatter present.

- [ ] **Step 3: Commit**

```bash
cd /Users/msimons/Code/Workboxes && \
git add .claude/commands/review-daily.md && \
git commit -m "Phase 6a: add /review-daily slash command"
```

---

## Task 7: Create `/review-weekly` slash command

**Files:**
- Create: `.claude/commands/review-weekly.md`

- [ ] **Step 1: Write the command file**

```markdown
---
description: Run the weekly Growth review — pass over intentions, active goals, therapy homework, and the week's daily reviews.
---

You are running the weekly Growth review for the Workboxes vault.

## 1. Load context

Read:

1. `_system/README.md`
2. `_system/voice.md`
3. `_system/growth-prompts.md` — specifically the `## Weekly` section.

## 2. Check for an existing weekly today

Run: `ls -1 Areas/Growth/Reviews/$(date +%Y-%m-%d)-weekly.md 2>/dev/null`

If it exists: ask append / overwrite / cancel (same handling as
`/review-daily`).

## 3. Load what the Weekly section asks for

Per `growth-prompts.md` ## Weekly:

- Current-period intention (see `/review-daily` for how to find it).
- All active goals (`Goals/*.md` where `status: active`). For each,
  read the tail of its `## Activity` section (last ~10 lines) if the
  section exists.
- `Therapy/homework.md` — full `## Active` section.
- List of daily reviews from the last 7 days:
  `ls -1 Areas/Growth/Reviews/*-daily.md` filtered to the last 7
  dates.

## 4. Walk the prompts

Ask each prompt from `growth-prompts.md` ## Weekly, one at a time,
quoting the loaded content inline.

## 5. Write the review file

Create `Areas/Growth/Reviews/<today>-weekly.md`:

```markdown
---
type: review
date: <today ISO>
kind: weekly
---

# Weekly Review — <today>

## Intention check
<quoted intention>

User: <answer>

## Goals touched this week
<list of active goals with recent-activity-line summary>

User: <answer, including any status-transition requests>

## Therapy homework
<homework.md active items>

User: <answer, including any completed items>

## Anything from the week's daily reviews
<list of daily review filenames>

User: <answer>
```

If every answer was "nothing", still write the file with a top-level
"Nothing came up this week." line, then the sections as above.

## 6. Handle follow-ups

For each proposed change in the user's answers:

- **Goal status transition:** propose → approve → edit the goal file's
  `status` frontmatter → append to `Goals/_index.md` recent activity.
- **Homework completion:** propose → approve → move item from
  `## Active` to `## Completed` in `Therapy/homework.md` with today's
  date.
- **New homework:** propose → approve → append to `## Active` in
  `Therapy/homework.md`.
- **New reflection / theme:** propose → approve → create file per
  `_system/workbox-schema.md`.

Each edit is a separate approval step.

## 7. Commit

```bash
cd /Users/msimons/Code/Workboxes && \
git add Areas/Growth && \
git commit -m "Growth: weekly review <today>"
```

## Tone

Terse. One prompt at a time. No trailing summary.
```

- [ ] **Step 2: Verify and commit**

```bash
ls -la .claude/commands/review-weekly.md && \
cd /Users/msimons/Code/Workboxes && \
git add .claude/commands/review-weekly.md && \
git commit -m "Phase 6a: add /review-weekly slash command"
```

Expected: file exists; commit succeeds.

---

## Task 8: Create `/review-quarterly` slash command

**Files:**
- Create: `.claude/commands/review-quarterly.md`

- [ ] **Step 1: Write the command file**

```markdown
---
description: Run the quarterly Growth review — archive current intention, draft next, check in on values, goals, and themes.
---

You are running the quarterly Growth review for the Workboxes vault.

## 1. Load context

Read:

1. `_system/README.md`
2. `_system/voice.md`
3. `_system/growth-prompts.md` — specifically the `## Quarterly`
   section.

## 2. Check for an existing quarterly today

Run: `ls -1 Areas/Growth/Reviews/$(date +%Y-%m-%d)-quarterly.md
2>/dev/null`

If it exists: ask append / overwrite / cancel.

## 3. Load what the Quarterly section asks for

- Current-period intention: list `Intentions/*.md`, pick the one whose
  `period` frontmatter contains today's date, with narrowest period
  preferred.
- All values (`Values/*.md`) — both active and archived.
- All active goals (`Goals/*.md` where `status: active`), including
  full `## Activity` sections.
- All active themes: enumerate `Themes/*/_index.md` where
  `status: active`, and count reflection files in each theme folder.

## 4. Walk the prompts

Ask each prompt from `growth-prompts.md` ## Quarterly, one at a time.

Additionally: ask the user what the next period slug should be (e.g.
if we're at the end of `2026-Q2`, suggest `2026-Q3`).

## 5. Write the review file

Create `Areas/Growth/Reviews/<today>-quarterly.md`:

```markdown
---
type: review
date: <today ISO>
kind: quarterly
---

# Quarterly Review — <today>

## Closing intention
<quoted intention>

User final notes: <answer>

## Next intention (<next-period>)

Focus points:
<draft focus points from user>

## Values check-in
<list of values with status>

User: <answer>

## Goals retrospective
<list of active goals>

User: <answer>

## Themes landscape
<list of active themes with reflection counts>

User: <answer>
```

## 6. Execute follow-ups

- **Close current intention:** edit its frontmatter to
  `status: completed`, add any final notes to the body, append to
  `Intentions/_index.md` recent activity.
- **Create next intention:** create
  `Intentions/<next-period>.md` with full frontmatter and the focus
  points drafted above. (Use the intention template from the
  `/growth-new` flow but inline here.)
- **Value archive:** for each value the user archived, edit its
  frontmatter `status: archived`.
- **New values:** propose → approve → create via the same pattern
  as `/growth-new value`.
- **Goal transitions:** edit `status` on each affected goal.
- **Theme transitions:** edit `status` on each affected theme MOC.

Each edit is a separate approval step (batch obvious ones if the user
prefers).

## 7. Commit

```bash
cd /Users/msimons/Code/Workboxes && \
git add Areas/Growth && \
git commit -m "Growth: quarterly review <today>"
```

## Tone

Terse. One prompt at a time. No trailing summary.
```

- [ ] **Step 2: Verify and commit**

```bash
ls -la .claude/commands/review-quarterly.md && \
cd /Users/msimons/Code/Workboxes && \
git add .claude/commands/review-quarterly.md && \
git commit -m "Phase 6a: add /review-quarterly slash command"
```

Expected: file exists; commit succeeds.

---

## Task 9: Modify `/triage` command for Growth

**Files:**
- Modify: `.claude/commands/triage.md` (sections 1, 3, 5)

- [ ] **Step 1: Update section 1 (Load context)**

In `.claude/commands/triage.md`, find the numbered list in section
`## 1. Load context` that currently ends with:

```
5. Each file matching `Areas/*/_index.md` — for each, read only the YAML
   frontmatter and the first two sections ("# <Name>" heading and "## Claude:
   Read this first"). Do not read `## Open loops`, `## Projects`, or
   `## Recent activity` at this stage — they are not needed for routing.
```

After that item (and before the "**Cross-check active areas.**"
paragraph), insert:

```
6. `_system/growth-surfacing.md` — the scan patterns used during
   disposition proposals for Growth content.
7. For Growth surfacing (used in step 3): list
   `Areas/Growth/Values/*.md`, `Areas/Growth/Goals/*.md`,
   `Areas/Growth/Practices/*.md`, and `Areas/Growth/Themes/*/`. Read
   the frontmatter and filename slug of each; you'll scan against
   these during proposal. You do not need to read their bodies.
```

- [ ] **Step 2: Update section 3 (Propose a disposition)**

In section `## 3. Propose a disposition for each item`, find the
bullet list that reads:

```
- **Route** — move to a specific `Areas/<area>/` (area-level note) or
  `Areas/<area>/Projects/<project>/` (project file, *folder must exist*).
  Give a one-line rationale.
- **New project** — propose creating `Areas/<area>/Projects/<slug>/` and
  routing the item there. Explain why this doesn't fit an existing project.
- **New area** — propose creating `Areas/<slug>/`. Rare; only when the item
  genuinely belongs to a domain outside the seven starter areas.
- **Discard** — duplicate, test, or genuinely throwaway. Requires explicit
  approval.
- **Defer** — leave in `Inbox/` and add a `triaged_deferred_at: <ISO 8601
  timestamp>` field to the item's frontmatter. Use for both "insufficient
  signal to route" and "could plausibly go in multiple places and I want
  the user to decide later" — state which in the rationale.
```

After the **Defer** bullet, insert a new **Route to Growth** bullet:

```
- **Route to Growth** — one of three sub-forms:
  - **New reflection** — item is reflective/introspective and matches
    an existing theme slug. Create
    `Areas/Growth/Themes/<slug>/<inbox-filename>.md` (move the item
    whole, change `type` to `reflection`, keep provenance fields).
  - **New theme** — item is reflective and no theme matches. Propose
    a new theme folder `Areas/Growth/Themes/<slug>/` with MOC; route
    the item into it as a new reflection.
  - **Append** — item is a short progress note (≤ 3 sentences, no new
    topic) that fits an existing `goal`, `value`, `practice`, or
    `therapy-homework` target. Propose the target file and the exact
    bullet to append. On approval, append under the target's activity
    section (per `_system/workbox-schema.md` append exception rules)
    and `git rm` the original inbox item.
```

Also, find the existing sub-section `### Applying triage rules` and,
immediately before it, insert:

```
### Applying growth surfacing

After the rules check above, scan the item using
`_system/growth-surfacing.md` patterns against the Growth content
loaded in step 1. If any pattern matches:

- If a theme slug matches, or the item is reflective without any theme
  match, prefer **Route to Growth** (new reflection or new theme).
- If a goal/value/practice slug matches and the item is a short
  progress note, consider **Route to Growth — Append**.
- Otherwise, keep the base disposition (Route / New project / etc.)
  and note the Growth link(s) in the rationale as `[[<slug>]]` to be
  added to the item body.
```

- [ ] **Step 3: Update section 5 (Execute approved dispositions)**

In section `## 5. Execute approved dispositions`, after the existing
`### Defer` subsection, append:

````
### Route to Growth — new reflection

1. Verify the target theme folder exists:
   `Areas/Growth/Themes/<slug>/`.
2. Rewrite the item's frontmatter: change `type: inbox-item` to
   `type: reflection`. Rename `captured` to `created`. Keep `source`,
   `source_id`, `source_url`, `tags`.
3. Move: `git mv Inbox/<filename>.md
   Areas/Growth/Themes/<slug>/<filename>.md`.
4. Append to `Areas/Growth/Themes/<slug>/_index.md` under
   `## Reflections`: `- [[<filename>]] — <one-line summary>`.

### Route to Growth — new theme

1. Ask the user for a kebab-case theme slug if not already provided.
2. Create directory: `Areas/Growth/Themes/<slug>/`.
3. Create `Areas/Growth/Themes/<slug>/_index.md` from the theme MOC
   template in `_system/workbox-schema.md`. Seed the "What this
   theme is" section with 1–2 sentences based on the inbox item
   being routed here.
4. Append to `Areas/Growth/Themes/_index.md` under `## Recent
   activity`: `- YYYY-MM-DD — New theme [[<slug>]]`.
5. Route the item as a new reflection (see above).

### Route to Growth — append

1. Determine the target file and its type (`goal`, `value`,
   `practice`, or `therapy-homework`).
2. Determine the append section per `_system/workbox-schema.md`:
   - `goal`, `value`, `practice` → `## Activity`
   - `therapy-homework` → `## Active`
3. If the target file lacks the section, add it.
4. Append a bullet of the form:
   `- YYYY-MM-DD — <one-line summary from the inbox item>`.
5. `git rm Inbox/<filename>.md` (the bullet is the preserved form).
6. Append to the target's parent MOC (`Goals/_index.md`,
   `Values/_index.md`, `Practices/_index.md`, or
   `Therapy/_index.md`) under `## Recent activity`:
   `- YYYY-MM-DD — Append to [[<target-slug>]]`.

### Surfacing-only annotation (non-Growth routes)

If growth surfacing suggested links but the base disposition was not
Route-to-Growth, after routing the item normally, edit the item body
to include the suggested `[[<slug>]]` wikilinks (in a `## Growth
links` section at the bottom of the item body, or inline if the user
prefers).
````

- [ ] **Step 4: Verify triage edits**

Run:
```bash
grep -n "Route to Growth\|growth-surfacing\|Applying growth surfacing" \
  .claude/commands/triage.md
```

Expected: at least 6 matches (load-context reference, disposition
bullet, three sub-forms in section 5, one in "Applying growth
surfacing").

- [ ] **Step 5: Commit**

```bash
cd /Users/msimons/Code/Workboxes && \
git add .claude/commands/triage.md && \
git commit -m "Phase 6a: extend /triage with Route-to-Growth and surfacing

Section 1 loads growth-surfacing.md + Growth content slugs. Section 3
adds Route-to-Growth disposition (new reflection / new theme /
append) and a growth-surfacing scan step. Section 5 adds execution
branches for each Growth sub-form and a surfacing-only annotation
step for non-Growth routes."
```

---

## Task 10: Update `_system/README.md`

**Files:**
- Modify: `_system/README.md`

- [ ] **Step 1: Add Growth to Active areas**

The existing `## Active areas` list in `_system/README.md` is in
life-priority order (not alphabetical): Work, Family, Relationships,
Home, Health, Finances, Personal. Insert Growth between Health and
Finances (growth sits near health as a related domain).

Find this exact text in `_system/README.md`:
```markdown
- `Areas/Health/` — physical, mental, appointments, fitness.
- `Areas/Finances/` — money, taxes, accounts, major decisions.
```

Replace with:
```markdown
- `Areas/Health/` — physical, mental, appointments, fitness.
- `Areas/Growth/` — values, goals, intentions, therapy notes, practices, reviews.
- `Areas/Finances/` — money, taxes, accounts, major decisions.
```

- [ ] **Step 2: Add new operating files to the Operating files section**

Find the `## Operating files` list. After the `capture-sources.md`
bullet and before `changelog.md`, insert:

```markdown
- **`growth-prompts.md`** — prompt content for `/review-daily`,
  `/review-weekly`, and `/review-quarterly`. Edit to tune review
  voice or prompts.
- **`growth-surfacing.md`** — scan rules `/triage` uses to suggest
  Growth links and dispositions. Extend as new patterns emerge.
```

- [ ] **Step 3: Add Growth commands to "How to run" section**

After the `## How to run triage` section, add:

```markdown
## How to run Growth reviews

- `/review-daily` — quick morning check-in.
- `/review-weekly` — Sunday pass over the week.
- `/review-quarterly` — end-of-quarter reset.

See `_system/growth-prompts.md` for what each review loads and asks.

## How to create Growth content

`/growth-new <type>` — prompts for slug and type-specific fields,
then creates the file. Types: `value`, `goal`, `intention`,
`practice`, `theme`, `therapy-session`. See
`_system/workbox-schema.md` for frontmatter.
```

- [ ] **Step 4: Verify**

Run:
```bash
grep -n "Areas/Growth\|growth-prompts\|growth-surfacing\|/review-daily\|/growth-new" \
  _system/README.md
```

Expected: at least 5 matches.

- [ ] **Step 5: Commit**

```bash
cd /Users/msimons/Code/Workboxes && \
git add _system/README.md && \
git commit -m "Phase 6a: add Growth area + commands to README

Growth listed between Health and Finances. New operating files
(growth-prompts, growth-surfacing) and new slash commands
(/review-*, /growth-new) documented."
```

---

## Task 11: Append Phase 6a entry to `_system/changelog.md`

**Files:**
- Modify: `_system/changelog.md`

- [ ] **Step 1: Append entry**

Append to the end of `_system/changelog.md` under `## Entries`:

```markdown

### 2026-04-19 — Phase 6a: Growth area

- Scaffolded `Areas/Growth/` with seven content subfolders (Values,
  Goals, Intentions, Themes, Therapy, Practices, Reviews) and their
  `_index.md` MOCs.
- Extended `_system/workbox-schema.md` with Growth content type
  schemas, naming rules, the append exception, and status lifecycle.
- Added `_system/growth-prompts.md` (daily/weekly/quarterly review
  prompts; monthly and annual deferred to Phase 6b).
- Added `_system/growth-surfacing.md` (Day-1 slug/alias scan rules
  for triage; lexicons and fuzzy matching deferred to Phase 6b).
- Added slash commands: `/growth-new`, `/review-daily`,
  `/review-weekly`, `/review-quarterly`.
- Extended `/triage` with Route-to-Growth disposition (new
  reflection / new theme / append) and growth-surfacing scans.
- Updated `_system/README.md` with Growth area, operating files, and
  command inventory.
```

- [ ] **Step 2: Commit**

```bash
cd /Users/msimons/Code/Workboxes && \
git add _system/changelog.md && \
git commit -m "Phase 6a: log Growth extension completion to changelog"
```

---

## Task 12: End-to-end verification walkthroughs

**Files:** (no changes — this task is manual verification)

This task runs through each spec-required walkthrough scenario.
Failures indicate earlier-task bugs to fix before declaring Phase 6a
complete.

- [ ] **Step 1: Walkthrough 1 — `/growth-new value`**

Open a new Claude Code session in the Workboxes directory. Invoke
`/growth-new value`.

Expected behavior:
- Claude asks for a slug.
- Entering `courage` is accepted.
- Claude asks for aliases.
- Entering empty string is accepted.
- Claude creates `Areas/Growth/Values/courage.md` with correct
  frontmatter (`type: value`, `status: active`, `aliases: []`) and
  the value body template headings.
- Claude appends to `Areas/Growth/Values/_index.md` under
  `## Recent activity`.
- Claude commits.

If any of these fail, note which task's command file needs fixing.

After verification: **delete the test file and revert the index edit
and commit.** Run:
```bash
git reset --hard HEAD~1
```
(only if the walkthrough created exactly one commit; adjust if more)

- [ ] **Step 2: Walkthrough 2 — `/growth-new goal`**

Invoke `/growth-new goal`. Enter slug `test-goal`, horizon `2026-Q4`,
linked_values empty.

Expected: file created at `Areas/Growth/Goals/test-goal.md` with
correct frontmatter and template body. Revert via
`git reset --hard HEAD~1`.

- [ ] **Step 3: Walkthrough 3 — `/review-daily` end-to-end**

Invoke `/review-daily`. Answer "nothing" to all prompts.

Expected: file created at
`Areas/Growth/Reviews/<today>-daily.md` with the "Nothing came up
today." body and the prompt sections with "nothing" answers. No
status transitions proposed.

Revert via `git reset --hard HEAD~1`.

- [ ] **Step 4: Walkthrough 4 — `/triage` value-slug surfacing**

Set up: first recreate `Values/courage.md` (same as Walkthrough 1)
but keep the commit. Then place a file in `Inbox/`:

```bash
cat > Inbox/2026-04-19-1200-test-courage.md <<'EOF'
---
type: inbox-item
captured: 2026-04-19T12:00:00-04:00
source: raw
tags: []
---

I want to practice more courage at work this week.
EOF
```

Invoke `/triage`. Expected: Claude's disposition proposal for this
item includes `[[courage]]` in the rationale (surfacing match).

After verifying: `git reset --hard HEAD~N` back to before Walkthrough 1's
commit, where N covers the value file and the inbox file.

- [ ] **Step 5: Walkthrough 5 — `/triage` append disposition**

Set up:
```bash
cat > Areas/Growth/Goals/test-goal.md <<'EOF'
---
type: goal
created: 2026-04-19
horizon: 2026-Q4
status: active
linked_values: []
---

# Test Goal

## Why this goal
Verification.
EOF
cat > Inbox/2026-04-19-1201-test-goal-progress.md <<'EOF'
---
type: inbox-item
captured: 2026-04-19T12:01:00-04:00
source: raw
tags: []
---

Made progress on test-goal today — ran 3 miles.
EOF
git add Areas/Growth/Goals/test-goal.md Inbox/
git commit -m "test: walkthrough 5 fixtures"
```

Invoke `/triage`. Expected: Claude proposes `Route to Growth — Append`
to `Goals/test-goal.md`. On approve: bullet appended under
`## Activity` in the goal file; inbox file `git rm`'d.

Revert via `git reset --hard HEAD~2`.

- [ ] **Step 6: Walkthrough 6 — `/triage` new theme**

Set up:
```bash
cat > Inbox/2026-04-19-1202-reflection.md <<'EOF'
---
type: inbox-item
captured: 2026-04-19T12:02:00-04:00
source: raw
tags: []
---

I keep noticing a pattern where I overcommit and then resent the
commitments. There's something about saying no that feels dangerous.
EOF
git add Inbox/ && git commit -m "test: walkthrough 6 fixture"
```

Invoke `/triage`. Expected: Claude proposes **Route to Growth — New
theme** with a proposed slug (e.g. `overcommitment`). On approve:
new theme folder + MOC + reflection file; inbox file moved.

Revert via `git reset --hard HEAD~2`.

- [ ] **Step 7: Commit verification notes (optional)**

If any walkthroughs revealed bugs that were fixed during this task,
commit those fixes. Otherwise, no action.

---

## Task 13: Final sanity check and Phase 6a completion commit

**Files:** none — verification only

- [ ] **Step 1: Verify vault is clean and all pieces committed**

Run:
```bash
cd /Users/msimons/Code/Workboxes && \
git status
```

Expected: "nothing to commit, working tree clean".

- [ ] **Step 2: Verify the full file inventory**

Run:
```bash
ls Areas/Growth/ Areas/Growth/*/  && \
ls _system/growth-prompts.md _system/growth-surfacing.md && \
ls .claude/commands/growth-new.md .claude/commands/review-daily.md \
   .claude/commands/review-weekly.md .claude/commands/review-quarterly.md && \
grep -c "Growth" _system/README.md && \
grep -c "Growth content types" _system/workbox-schema.md && \
grep -c "Phase 6a" _system/changelog.md
```

Expected: all files listed; `grep` counts ≥ 1 for each line.

- [ ] **Step 3: Verify git log shows Phase 6a commits**

Run:
```bash
git log --oneline | grep "Phase 6a" | wc -l
```

Expected: 11 (one per task 1–11; task 12 may add more if fixes
were needed).

- [ ] **Step 4: Done**

Phase 6a is complete. The Growth area is scaffolded, commands are
live, schema is extended, and triage is growth-aware. Phase 6b
(monthly/annual reviews, therapy templates, richer surfacing) remains
deferred.

---

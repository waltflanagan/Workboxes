# Workboxes Phase 6: Growth Extension — Design

**Status:** Draft
**Date:** 2026-04-19
**Supersedes:** N/A (extends `2026-04-18-workboxes-design.md`)

## Goal

Add a dedicated space in the Workboxes vault for personal growth work:
values, goals, intentions, growth themes, therapy notes, practices, and
periodic reviews. Extend (not replace) the existing vault so growth content
participates in triage, surfacing, and Claude-driven navigation using the
same mechanics as the rest of the system.

This is the Workboxes Phase 6 extension the user flagged as personally
important during the original design. Day One continues to be the journal;
Phase 6 routes and accumulates rather than duplicates.

## Non-goals

- Replacing Day One for journaling.
- Building a mood tracker, habit tracker, or any time-series analytics.
- Encrypting a subset of growth content at rest (user chose "all-same"
  privacy; the vault's existing privacy posture applies uniformly).
- Shipping monthly/annual reviews on Day 1 — deferred to Phase 6b.
- Building keyword/emotion lexicons for surfacing — Day 1 uses simple
  slug/alias matching only.

## Approach

**Approach 1 (chosen): new top-level `Areas/Growth/` Area** with internal
substructure per content type. Rationale: Growth is a life domain like
Work or Health; putting it alongside the existing seven Areas keeps the
mental model ("Areas are domains of life") intact, and lets the existing
`/triage` routing conventions apply unchanged. Alternatives considered
and rejected: (a) a sibling top-level folder outside `Areas/` — breaks the
domain model; (b) spreading growth content across existing Areas — splits
values/goals from each other and makes reviews impossible to run.

## Architecture

### Folder layout

```
Areas/Growth/
├── _index.md
├── Values/
│   ├── _index.md
│   └── <value-slug>.md
├── Goals/
│   ├── _index.md
│   └── <goal-slug>.md
├── Intentions/
│   ├── _index.md
│   └── <period-slug>.md
├── Themes/
│   ├── _index.md
│   └── <theme-slug>/
│       ├── _index.md
│       └── <reflection>.md
├── Therapy/
│   ├── _index.md
│   ├── sessions/
│   │   └── YYYY-MM-DD.md
│   └── homework.md
├── Practices/
│   ├── _index.md
│   └── <practice-slug>.md
└── Reviews/
    └── YYYY-MM-DD-<kind>.md
```

All subfolders ship with `_index.md` MOCs. No example content is seeded;
the user adds values/goals/etc. when they're ready.

### Frontmatter schemas

All types inherit the base fields from `workbox-schema.md` (`type`,
`created`, `source`, `tags`) plus type-specific fields below.

**Value** (`Values/<slug>.md`)
```yaml
type: value
created: 2026-04-19
status: active           # active | archived
aliases: []              # alternate names for link-driven surfacing
```
Body: what the value means, stories/examples, how I'll know I'm living it.

**Goal** (`Goals/<slug>.md`)
```yaml
type: goal
created: 2026-04-19
horizon: 2026-Q4         # target period (rough, not a hard deadline)
status: active           # active | achieved | abandoned | paused
linked_values: []        # wikilinks, e.g. [[health]]
```
Body: why this goal, what success looks like, linked practices/projects.

**Intention** (`Intentions/<period-slug>.md`)
```yaml
type: intention
period: 2026-Q2          # required; the period this covers
created: 2026-04-19
status: active           # active | completed
```
Body: 1–5 focus points for the period, linked to values/goals.

**Theme** (`Themes/<slug>/_index.md`)
```yaml
type: theme
created: 2026-04-19
status: active           # active | dormant | resolved
```
Sub-files in the theme folder use:
```yaml
type: reflection
created: 2026-04-19
```
Free-form body.

**Therapy session** (`Therapy/sessions/YYYY-MM-DD.md`)
```yaml
type: therapy-session
date: 2026-04-19
modality: individual     # free-text: individual | couples | group | ...
```
Body: session notes, insights, homework assigned (linked to `homework.md`).

**Therapy homework** (`Therapy/homework.md`) — single long-lived document.
```yaml
type: therapy-homework
created: 2026-04-19
```
Body: running list of homework items under an `## Active` section and a
`## Completed` section. Accepts appends (see capture flow).

**Practice** (`Practices/<slug>.md`)
```yaml
type: practice
created: 2026-04-19
cadence: daily           # daily | weekly | situational
status: active           # active | paused | retired
linked_values: []
```
Body: what the practice is, why, how to do it, how I'm tracking it.

**Review** (`Reviews/YYYY-MM-DD-<kind>.md`)
```yaml
type: review
date: 2026-04-19
kind: weekly             # daily | weekly | monthly | quarterly | annual
```
Body: prompts + answers for that review cadence.

### Naming rules

- Kebab-case slugs for values, goals, practices, themes.
- ISO date (`YYYY-MM-DD`) for therapy sessions and reviews.
- Period slugs for intentions: `2026-Q2`, `2026-04`, `2026` — whichever
  granularity the user picks for that intention.

### Status lifecycle

| Type | Transitions |
|---|---|
| value | active → archived (via review; rarely) |
| goal | active → achieved / abandoned / paused; paused → active |
| intention | active → completed at period end |
| theme | active → dormant (no recent reflections) → resolved |
| practice | active → paused → retired |

Reviews are the primary mechanism for proposing status transitions; the
user approves each.

## Capture flow

### Direct creation (`/growth-new <type>`)

Primary path for values, goals, intentions, practices, therapy sessions.
Usage:

```
/growth-new value
```

Claude responds with "What's the slug?" and prompts for any other
type-specific required fields. Creates the file with seeded frontmatter
and a stub body that includes the section headings the schema expects.

The slug is *always* prompted, not taken as a positional argument. This
matches the conversational flow of `/triage` and avoids users hitting
syntax errors on a rarely-used command.

### Inbox triage (growth-routed items)

Primary path for stray reflections, goal progress notes, theme material.

`/triage` gains a new disposition: **Route to Growth**. Sub-forms:

1. Create a new reflection file under an existing theme folder.
2. Create a new theme folder (proposal + approval, same pattern as
   "New project" in the existing triage command).
3. **Append** to an existing goal, value, practice, or therapy-homework
   file — a short captured bullet becomes a line in the target file.

The third option is the **append exception** to the existing
items-move-whole rule. It applies *only* to Growth, and only to targets
of type `goal`, `value`, `practice`, or `therapy-homework`. Therapy
sessions and reviews are dated documents and never accept appends.

When triage appends:
1. Show the target file and the exact bullet to append.
2. On approval:
   - For `goal`, `value`, `practice`: append under `## Activity`
     (create the section if absent).
   - For `therapy-homework`: append under `## Active` (create if absent).
3. `git rm` the original inbox item (the bullet is the preserved form).
4. Log the route in the user's changelog.

### Link-driven surfacing during triage

`/triage` reads `_system/growth-surfacing.md` during step 1. When
proposing a disposition, triage also scans the item for:

- Value slugs or aliases → suggest `[[<value>]]` in the item body.
- "I want to / goal of / by <date>" phrasing near a known goal slug →
  suggest linking to the goal.
- Therapy-homework language ("my therapist said", "between sessions") →
  suggest routing to `Therapy/homework.md` or linking from there.
- Existing theme-slug match → suggest linking to the theme.

Surfacing is **suggestive only**. The item still routes to its best-fit
workbox via the normal triage flow; Growth links appear in the item body
or the routing rationale.

## Reviews

### Cadences shipped in Phase 6a

| Kind | Cadence | Surfaces |
|---|---|---|
| Daily | each morning | today's intention; 1 active practice; 1 active goal |
| Weekly | Sunday | active intentions; goals touched this week; therapy homework status |
| Quarterly | period end | current intention → archive; draft next intention; values check-in |

Monthly and annual reviews are deferred to Phase 6b.

### Review execution

Slash commands: `/review-daily`, `/review-weekly`, `/review-quarterly`.
Each command:

1. Reads `_system/growth-prompts.md` for its section.
2. Pulls the files listed in that section's "what to load" block (e.g.,
   weekly loads active intentions + recent-activity lines from active
   goals).
3. Walks the user through the prompts.
4. Writes `Reviews/YYYY-MM-DD-<kind>.md` with the user's answers.
5. If the review proposes status transitions (e.g., mark a goal
   achieved), triage those as separate approve-then-execute steps.

### Always create

Even if the user has "nothing to say," the review file is still written
(body may be one line: "nothing came up today"). Skipped-review files
become evidence later — an empty file is cheaper than a silent gap.

## `_system/` extensions

Two new files:

**`_system/growth-prompts.md`** — one section per review kind. Each
section contains:
- A short intro for what the review is for.
- The prompts Claude asks the user, in order.
- A "what to load" block: the files/globs to pull before asking.

Day-1 sections: daily, weekly, quarterly. Monthly and annual are stubbed
with `<!-- deferred to Phase 6b -->`.

**`_system/growth-surfacing.md`** — rules for link-driven surfacing
during triage. Documents the scan patterns listed above.

### Schema extension

Append a new section to `_system/workbox-schema.md`: **"Growth content
types."** This section includes:

1. The `Areas/Growth/` folder layout.
2. Frontmatter tables for all seven types (copied from this spec).
3. The append exception — explicit documentation of when triage may
   append to an existing file and which target types accept it.
4. Naming rules.
5. Status lifecycle table.

The schema stays in one file; no sibling `growth-schema.md`. Claude
reads `workbox-schema.md` at session start for triage, and splitting
risks drift.

### README

`_system/README.md` gets `Growth` added to its `## Active areas` list in
alphabetical order.

## Triage changes

`.claude/commands/triage.md` step 1 (context load) gains one line:
"Read `_system/growth-surfacing.md`."

Step 3 (propose a disposition) gains the **Route to Growth** disposition
with its three sub-forms (append / new theme / new reflection).

Step 5 (execute) gains a branch for Route-to-Growth that handles the
append sub-form specially: the item is not `git mv`'d but rather read,
summarized as a bullet, appended to the target's `## Activity` section,
and then `git rm`'d.

## Data flow

```
Drafts / Web Clipper / raw text / Day One
        │
        ▼
    Inbox/*.md   (unchanged shape: type: inbox-item)
        │
        ▼  /triage
        │
   ┌────┴────────────────────────┐
   │                             │
   ▼                             ▼
Regular workbox routing    Route to Growth
(items move whole)         ├── new reflection under theme (moves whole)
                           ├── new theme folder (MOC + moves whole)
                           └── append to goal/value/practice
                               (bullet appended; original git rm'd)

                           Parallel: link-driven surfacing
                           annotates routing rationale or item body
                           with [[growth-links]] regardless of
                           destination.
```

Direct creation bypasses Inbox:

```
User → /growth-new <type> → Areas/Growth/<Type>/<slug>.md
```

Reviews are their own flow:

```
User → /review-<kind> → reads growth-prompts.md → walks prompts →
       writes Reviews/YYYY-MM-DD-<kind>.md → optionally proposes
       status transitions back through normal approval
```

## Privacy

All Growth content shares the vault's existing privacy posture. The git
repo, any remote, and the filesystem location are the same as for the
other seven Areas. If the user ever wants to pull therapy out later,
the migration is a `git mv` plus a `.gitignore` entry — deferred until
there's a concrete reason.

## Phasing

### Phase 6a — Day 1

- `Areas/Growth/` folder structure with all seven subfolder `_index.md`
  files (no seeded content).
- Extend `_system/workbox-schema.md` with the Growth content types
  section.
- `_system/growth-prompts.md` with daily, weekly, quarterly sections.
- `_system/growth-surfacing.md` with slug/alias matching rules.
- `/growth-new <type>` slash command.
- `/review-daily`, `/review-weekly`, `/review-quarterly` slash commands.
- `/triage` updated: reads `growth-surfacing.md`; supports the
  Route-to-Growth disposition and the append exception.
- `_system/README.md` updated: `Growth` added to active areas.
- `_system/changelog.md` entry for Phase 6a completion.

### Phase 6b — deferred

- `/review-monthly` and `/review-annual`.
- Therapy session note templates (let real sessions reveal the shape
  first).
- Richer surfacing rules (emotion lexicons, theme-phrase dictionaries)
  if Day-1 slug matching feels thin.

## Testing / validation

This is a vault, not a service — "tests" are walkthrough scenarios:

1. `/growth-new value` creates a well-formed value file with correct
   frontmatter and prompts for a slug.
2. `/growth-new goal` creates a goal file, prompts for slug + horizon.
3. `/review-daily` runs end-to-end, reads prompts, writes a review
   file, handles the empty-body case.
4. `/triage` on an inbox item that mentions a known value slug surfaces
   the link suggestion in the rationale.
5. `/triage` on a captured progress note targeting an existing goal
   proposes the append disposition; on approval, appends to the goal's
   `## Activity` section and removes the inbox file.
6. `/triage` on a captured reflection matching no existing theme
   proposes "New theme" with a proposed slug.

Each walkthrough is a row in the Phase 6a plan's verification task.

## Open questions

None remaining; design is ready for plan.

## References

- `docs/superpowers/specs/2026-04-18-workboxes-design.md` — parent spec.
- `docs/superpowers/plans/2026-04-18-workboxes-phase-0-through-2.md` —
  shipped; establishes the `_system/`, `Areas/`, and triage conventions
  this spec extends.
- `_system/workbox-schema.md` — the file this spec extends.
- `.claude/commands/triage.md` — the command this spec modifies.

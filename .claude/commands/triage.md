---
description: Triage the Workboxes inbox — propose dispositions for each captured item and route to workboxes on approval.
---

You are triaging the Workboxes vault inbox. Follow this procedure exactly.

## 1. Load context

Read these files in order:

1. `_system/README.md` — entry point.
2. `_system/workbox-schema.md` — conventions.
3. `_system/triage-rules.md` — learned rules.
4. `_system/voice.md` — user voice/preferences.
5. Each file matching `Areas/*/_index.md` — for each, read only the YAML
   frontmatter and the first two sections ("# <Name>" heading and "## Claude:
   Read this first"). Do not read `## Open loops`, `## Projects`, or
   `## Recent activity` at this stage — they are not needed for routing.
6. `_system/growth-surfacing.md` — the scan patterns used during
   disposition proposals for Growth content.
7. For Growth surfacing (used in step 3): list
   `Areas/Growth/Values/*.md`, `Areas/Growth/Goals/*.md`,
   `Areas/Growth/Practices/*.md`, and `Areas/Growth/Themes/*/`. Read
   the frontmatter and filename slug of each; you'll scan against
   these during proposal. You do not need to read their bodies.

**Cross-check active areas.** The list of Areas on disk (`Areas/*/`) is the
authoritative source. If `_system/README.md`'s `## Active areas` section
disagrees with the filesystem (an area listed that doesn't exist, or an area
on disk not listed), flag it to the user at session start and offer to fix
the README before proceeding.

## 2. List inbox items

Run: `ls -1 Inbox/*.md 2>/dev/null | sort` to list inbox files in chronological
order (filenames start with date+time). If zero matches, report "Inbox is
empty." and stop.

For each file, read its full contents (frontmatter + body). If a file has no
frontmatter, treat it as:
```yaml
type: inbox-item
captured: <file mtime as ISO 8601>
source: raw
tags: []
```

## 3. Propose a disposition for each item

Items move whole. If an inbox item blends topics, route it to the best-fit
workbox and note the other topic(s) in the rationale — do not split it here.
Distillation and splitting are a later (Phase 7) workflow.

Before proposing **Route** to a specific project, verify that
`Areas/<area>/Projects/<slug>/` actually exists on disk. If it doesn't, the
disposition must be **New project**, not Route.

For each item, choose one:

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

### Applying growth surfacing

In parallel with the rules check, scan the item using
`_system/growth-surfacing.md` patterns against the Growth content
loaded in step 1. If any pattern matches:

- If a theme slug matches, or the item is reflective without any theme
  match, prefer **Route to Growth** (new reflection or new theme).
- If a goal/value/practice slug matches and the item is a short
  progress note, consider **Route to Growth — Append**.
- Otherwise, keep the base disposition (Route / New project / etc.)
  and note the Growth link(s) in the rationale as `[[<slug>]]` to be
  added to the item body.

### Applying triage rules

Before proposing, scan `_system/triage-rules.md`. If an item matches a rule
top-to-bottom, mark the proposal **high-confidence** and prefix the rationale
with "(rule: <rule name>)". Otherwise mark **medium** or **low** confidence
based on how specifically the item matches a workbox.

## 4. Walk the user through proposals

Present items one at a time (or batch obvious high-confidence ones together —
ask the user's preference at the start of the session). For each:

- Show: filename, source, one-line content summary, proposed disposition,
  rationale, confidence.
- User responses: **approve**, **change to <X>**, **defer**, **skip**.
- Keep the conversation terse (consult `_system/voice.md`).

## 5. Execute approved dispositions

For each approved item, perform these actions:

### Route

1. Determine the target path.
2. If routing to a project and the project folder does not exist, fail —
   routing to a nonexistent project should have been a "New project"
   proposal in step 3.
3. Rewrite the file's frontmatter: change `type` from `inbox-item` to
   `note` (or `log-entry` / `decision` / `reference` if the user specifies).
   Preserve `source`, `source_id`, `source_url`. Rename `captured` to
   `created`. Keep `tags`.
4. Move the file: `git mv Inbox/<filename>.md <target path>/<filename>.md`.
5. Append to the target workbox's `_index.md` under `## Recent activity`:
   `- YYYY-MM-DD — <one-line summary>`.

### New project

1. Ask user for a kebab-case project slug if not already provided.
2. Create directory: `Areas/<area>/Projects/<slug>/`.
3. Create `Areas/<area>/Projects/<slug>/_index.md` from the project MOC
   template in `_system/workbox-schema.md`. Fill in `# <Project Name>` and
   a seeded "Claude: Read this first" paragraph describing the project in
   1-2 sentences based on what you know from the inbox items being routed
   here.
4. Append to `Areas/<area>/_index.md` under `## Projects`:
   `- [[<slug>]] — <one-line description>`.
5. Route the item(s) into the new project (see Route above).

### New area

1. Confirm with the user that this genuinely doesn't fit an existing area.
2. Ask for a kebab-case area slug.
3. Create directory: `Areas/<slug>/Projects/`.
4. Create `Areas/<slug>/_index.md` from the area MOC template.
5. Append to `_system/README.md` under `## Active areas` in the correct
   alphabetical position.
6. Route the item (see Route above).

### Discard

Run: `git rm Inbox/<filename>.md`.

### Defer

Edit the file's frontmatter to add `triaged_deferred_at: <now ISO 8601>`.
Leave the file in `Inbox/`.

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

## 6. Extract rules

For each case where the user overrode your proposal in a way that looks
pattern-y (e.g., "always route Day One gratitude entries to
Personal/Gratitude"), propose a new rule for `_system/triage-rules.md` in
the format specified in that file. Present the rule to the user; on approval,
append it to `triage-rules.md`.

Do not propose rules for one-off decisions. Ask yourself: "would this same
decision apply to three or more similar items?" If unsure, don't propose.

## 7. Log the session

Append an entry to `_system/changelog.md` under `## Entries`:

```markdown
### <YYYY-MM-DD> — Triage session

- Triaged <N> items: <X> routed, <Y> new projects created, <Z> discarded,
  <W> deferred.
- New rules: <list rule names, or "none">.
- New workboxes: <list paths, or "none">.
```

## 8. Commit

One commit per triage session. Run:

```bash
cd /Users/msimons/Code/Workboxes && \
git add -A && \
git commit -m "Triage session <YYYY-MM-DD>: <N> items triaged

<brief per-disposition summary>
"
```

If there are no changes to commit (e.g., all items were deferred with no
frontmatter changes), skip the commit step.

## Tone

Consult `_system/voice.md` for user voice. Default to terse proposals. No
trailing summaries unless the user asks. Respect the user's time — this is
a tool they use to reduce cognitive load, not generate more of it.

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

For each item, choose one:

- **Route** — move to a specific `Areas/<area>/` (area-level note) or
  `Areas/<area>/Projects/<project>/` (project file). Give a one-line rationale.
- **New project** — propose creating `Areas/<area>/Projects/<slug>/` and
  routing the item there. Explain why this doesn't fit an existing project.
- **New area** — propose creating `Areas/<slug>/`. Rare; only when the item
  genuinely belongs to a domain outside the seven starter areas.
- **Discard** — duplicate, test, or genuinely throwaway. Requires explicit
  approval.
- **Defer** — insufficient signal; leave in `Inbox/` and add a
  `triaged_deferred_at: <ISO 8601 timestamp>` field to the item's frontmatter.

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

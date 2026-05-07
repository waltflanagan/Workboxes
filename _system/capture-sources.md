# Capture Sources

Inventory of capture sources, their provenance fields, and setup instructions.
All sources converge on `Inbox/` with the uniform frontmatter shape defined
in `workbox-schema.md`.

## Active sources (Day 1)

### Raw text

Drop any `.md` file into `Inbox/`. Works without setup.

- If frontmatter is missing, triage treats the file as `source: raw` and
  infers `captured` from the file's mtime.
- Filename convention: `YYYY-MM-DD-HHMM-<slug>.md`. If a dropped file does
  not follow this convention, triage will still handle it; a rename may
  happen during routing.

### Obsidian Web Clipper

The Obsidian Web Clipper extension saves web pages and selections into a
chosen folder using a user-defined template.

**Setup (manual, one-time):**

1. Install the Obsidian Web Clipper extension in your browser of choice.
2. In the extension's settings, set the **Vault** to the Workboxes vault.
3. Set the **Folder** to `Inbox`.
4. Set the **Template** to use the following **note content** and **filename format**:

**Filename format:**
```
{{date:YYYY-MM-DD}}-{{date:HHmm}}-{{title|slug}}
```

**Properties (frontmatter) — add these fields:**

| Name | Value |
|---|---|
| `type` | `inbox-item` |
| `captured` | `{{date:YYYY-MM-DDTHH:mm:ssZZ}}` |
| `source` | `web-clipper` |
| `source_url` | `{{url}}` |
| `tags` | (leave empty) |

**Note content template:**
```
{{content}}
```

5. Save the template and test-clip a page. Confirm a file appears in `Inbox/`
   with correctly-shaped frontmatter.

### Drafts → Obsidian action

A Drafts action that writes the current draft to the Workboxes `Inbox/` with
provenance frontmatter. Works on iOS and macOS.

**Setup (manual, one-time):**

1. Open Drafts. In the Actions directory, search for an "Append to Obsidian"
   or "Save to Obsidian" action, or create a new action of type **Script →
   File Append** / **Obsidian URL**.
2. Configure the action to write to the Workboxes vault, folder `Inbox`, with
   the following template as the file content:

```
---
type: inbox-item
captured: [[date|%Y-%m-%dT%H:%M:%S%z]]
source: drafts
source_id: [[draft_uuid]]
tags: [[tags]]
---

[[draft]]
```

(The exact template syntax depends on the Drafts action implementation
chosen. The key requirement is that the resulting file has frontmatter
matching `workbox-schema.md`'s inbox-item spec.)

3. Filename template: `[[date|%Y-%m-%d]]-[[date|%H%M]]-[[title|slug]].md`.
4. Test from the Drafts share sheet (iOS) and the Drafts app directly (macOS).
   Confirm files land in `Inbox/`.

**Known quirks:** Drafts sometimes writes without a trailing newline — triage
tolerates this. Voice-dictated drafts may lack a useful title; the filename
slug will be derived from the first words.

### Gmail — school email pipeline

Pull source for school and district newsletters. Operated by
`/school-email-triage` (not the standard `/triage` inbox flow). Notes are
filed **directly into child project folders** — they never land in `Inbox/`.

**Setup (one-time):**

1. Connect the Gmail MCP connector in Cowork settings.
2. Open `_system/school-email-senders.md` and fill in your actual sender
   addresses / Gmail query strings for Oscar, Nigel, Avery, and the district.
3. Run `/school-email-triage` — it will process threads from the last 30 days
   on the first run (no existing high-water mark), then only new threads on
   subsequent runs.

**Provenance fields:**

| Field | Value |
|---|---|
| `source` | `gmail` |
| `source_id` | Gmail message ID |
| `source_url` | Gmail permalink |
| `child` | `oscar` / `nigel` / `avery` / `all` |
| `email_type` | `newsletter` / `school-info` / `district-info` |

**High-water marks:** stored in `_system/sync-state.json` under a key per
sender label (e.g., `gmail_oscar_newsletter`). The command updates these after
each successful batch.

**Calendar integration:** requires Google Calendar MCP connector.

**Things integration:** requires Things MCP connector. If not connected, action
items are filed as vault notes with `has_actions: true` and a `## Action items`
section for manual processing.

## Deferred sources

Documented here with intended provenance fields so future pipelines stay
consistent.

- **Day One** (Phase 3) — `source: dayone`, `source_id: <entry uuid>`. Pull
  sync via a `sync-dayone` script; photos inlined to `Attachments/`.
- **Things** (Phase 5) — `source: things`, `source_id: <task uuid>`. Task
  state sync-back (if any) is a Phase 5 design decision.
- **Voice Memos** (Phase 5) — `source: voice-memo`, `source_id: <recording
  filename>`. Requires a transcription step upstream of inbox-write.
- **Messages / iMessage** (Phase 5) — `source: messages`,
  `source_id: <message guid>`.

## Adding a new source

When a new source is wired up:

1. Update this file with setup instructions and provenance fields.
2. If it's a pull source, add a top-level key to `_system/sync-state.json`.
3. Append an entry to `_system/changelog.md`.
4. No changes to triage logic are required — all sources land in `Inbox/`
   with the standard frontmatter shape.

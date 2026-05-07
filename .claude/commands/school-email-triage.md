---
description: Pull school and district emails from Gmail, extract events and action items, file notes per child, push to Calendar and Things.
---

You are running the school email triage pipeline for the Workboxes vault.
Follow this procedure exactly.

## 1. Load context

Read these files in order:

1. `_system/README.md`
2. `_system/workbox-schema.md` — especially the `school-email` frontmatter type.
3. `_system/school-email-senders.md` — the sender config. If placeholder
   addresses are still present (contain `.example.com`), stop and tell the
   user to fill in their real sender addresses before proceeding.
4. `_system/sync-state.json` — high-water marks. Missing keys = first run for
   that sender; default lookback window is 30 days.
5. `_system/voice.md` — tone/preferences.
6. `Areas/Family/Oscar.md`, `Areas/Family/Nigel.md`, `Areas/Family/Avery.md`
   — for context on open loops.

## 2. Pull emails from Gmail

For each sender entry in `_system/school-email-senders.md`:

1. Build a Gmail search query:
   - Start with the entry's `gmail_query` value.
   - If a high-water mark exists in `sync-state.json` for this sender's label
     key (`gmail_<child>_<label-slug>`), append `after:<YYYY/MM/DD>` using
     the stored date.
   - If no high-water mark exists, append `after:<30-days-ago>`.
   - Always append `is:unread OR is:read` (i.e., don't filter by read state —
     some school emails may already be marked read).
2. Call `search_threads` with the query. Retrieve up to 20 threads per sender.
3. For each thread, call `get_thread` to retrieve the full message body and
   metadata (subject, from, date, message-id).

Collect all results. De-duplicate by `source_id` (message ID) across senders
in case the same email matches multiple queries.

## 3. Process each email

For each email thread retrieved:

### 3a. Determine child and email_type

Use the sender entry that matched to set `child` and `email_type`. If
`child: all`, you will create one copy of the note for each of Oscar, Nigel,
and Avery.

### 3b. Extract structured data

Read the full email body carefully and extract:

**Events** — any date-bearing item: field trips, school holidays, picture day,
performances, sports events, registration deadlines, schedule changes. For each:
- `date`: ISO date (YYYY-MM-DD). If only a month/day is given, infer year from
  context (upcoming = current year, or next year if the date has already passed).
- `description`: one-line plain-English description.
- `child`: inherit from the email's child field.

**Action items** — anything requiring a response or physical action from the
parent: permission slips, volunteer sign-ups, forms to submit, RSVPs, payments,
supply lists. For each:
- `task`: one-line description starting with a verb (e.g., "Sign and return
  permission slip for science museum trip").
- `due`: ISO date if stated; otherwise omit.
- `child`: inherit from the email's child field.

If nothing clear fits a category, omit it — do not hallucinate events or tasks.

### 3c. Create the vault note

For each child the note is for (`all` → three notes), create a file at:

```
Areas/Family/<Child>/YYYY-MM-DD-HHMM-<subject-slug>.md
```

Where `<Child>` is the title-cased name: `Oscar`, `Nigel`, or `Avery`.

Where `YYYY-MM-DD-HHMM` is the email's sent date/time and `<subject-slug>` is
the subject line lowercased, spaces replaced with hyphens, truncated to 50 chars.

Frontmatter:
```yaml
---
type: school-email
created: <email sent datetime, ISO 8601>
source: gmail
source_id: <message id>
source_url: https://mail.google.com/mail/u/0/#inbox/<message id>
child: <oscar|nigel|avery>
email_type: <newsletter|school-info|district-info>
sender: <from address>
subject: <subject line>
has_events: <true|false>
has_actions: <true|false>
tags: []
---
```

Body:
```markdown
# <Subject line>

*<sender> · <date>*

## Events

- YYYY-MM-DD — <description> [→ calendar]

## Action items

- <task> — due YYYY-MM-DD [→ Things]

## Full email

<full email body, preserve paragraph breaks, strip quoted/forwarded sections>
```

Omit `## Events` if none found. Omit `## Action items` if none found.
Always include `## Full email`.

### 3d. Push events to Calendar

For each extracted event with a date, call `create_event` on the Google Calendar
MCP with:
- `title`: "<Child name>: <event description>" (e.g., "Oscar: Science museum field trip")
- `start`: the event date as an all-day event (no time unless a specific time
  was stated in the email)
- `end`: same as start for single-day events
- `description`: "Source: <email subject> · Filed in [[<child>]] workbox"
- `calendar`: use the primary calendar unless the user has specified otherwise

If the Calendar MCP is not connected or returns an error, log a warning in the
note body under `## Events` as: `⚠ Calendar push failed — add manually`.

### 3e. Push action items to Things

For each extracted action item, call the Things MCP `create_task` (or
equivalent) with:
- `title`: the task description
- `notes`: "From: <email subject> · <child> · <email date>"
- `due_date`: if a due date was extracted
- `list`: "Family" (create this list in Things if it doesn't exist)

If the Things MCP is not connected or not available:
- Do not attempt the call.
- Leave the `[→ Things]` marker in the note as a manual reminder.
- After processing all emails, print a summary line: "⚠ Things MCP not
  connected — N action items left as vault notes."

## 4. Append to project indexes

For each child that received at least one new note, append to
`Areas/Family/<Child>.md` under `## Recent activity`:

```
- YYYY-MM-DD — <N> email(s) filed from <sender label(s)>
```

## 5. Update sync-state.json

For each sender entry processed successfully, update (or create) the key
`gmail_<child>_<label-slug>` in `_system/sync-state.json` with today's ISO date.

The label slug is the sender's `label` value lowercased, spaces → hyphens,
non-alphanumeric → removed. Example: "Oscar's school newsletter" →
`oscars-school-newsletter`.

Write the updated JSON back to `_system/sync-state.json`.

## 6. Print a summary

Show the user a concise run summary:

```
School email triage — YYYY-MM-DD

Processed N emails across X senders.

Oscar:   N emails · E events → calendar · A action items → Things
Nigel:   N emails · E events → calendar · A action items → Things
Avery:   N emails · E events → calendar · A action items → Things

⚠ Warnings (if any):
- <warning text>

Files created: <list of new vault note paths>
```

## 7. Commit

```bash
cd /Users/msimons/Code/Workboxes && \
git add -A && \
git commit -m "School email triage <YYYY-MM-DD>: <N> emails, <E> events, <A> actions

Oscar: N emails · Nigel: N emails · Avery: N emails
"
```

If there are no new emails, report "No new emails found." and skip the commit.

## Tone

Terse. No trailing summaries beyond the structured run report above.
Respect the user's time — surface only what needs attention.

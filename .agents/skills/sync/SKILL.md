# DEPRECATED

This skill references Python sync scripts and a vault layout that no longer exist. Sync is handled opportunistically within each skill via available MCP connectors.

## Thought capture

If at any point during this session the user says something prefixed with
"note:", "capture:", or "remember:", append it to
`vault/daily/scratch/YYYY-MM-DD.md` (today's date) before doing anything else:

```
- **HH:MM** {thought}
```

Create the file with the header below if it doesn't exist:
```
---
date: YYYY-MM-DD
type: daily-scratch
---

## Captures

```

---

## Connector 1: Things 3

Run:
```bash
python3 daily/sync/things.py
```

Capture stdout. On success it prints: `Things: N new, N updated`
On failure (non-zero exit or exception in stderr), record: `Things: FAILED — {stderr}`

---

## Connector 2: Day One

Run:
```bash
python3 daily/sync/dayone.py
```

Capture stdout. On success: `Day One: N new, N updated (N journals)`
On failure: `Day One: FAILED — {stderr}`

If the script fails with a path error (Day One container not found), suggest:
> "The Day One data path may differ on your machine. Check
> `daily/sync/dayone.py` and update `DAYONE_CONTAINER` to the correct path."

---

## Connector 3: Drafts

Use the Drafts MCP tools (JXA is unreliable across Drafts versions).

**3a.** Fetch flagged drafts:
Call `drafts_get_drafts` with `flagged: true`.

**3b.** Fetch recent inbox drafts (last 7 days):
Call `drafts_get_drafts` with `folder: "inbox"` and `modifiedAfter: YYYY-MM-DD` (7 days ago).

**3c.** Combine the two result lists and deduplicate by `id`.

**3d.** For each draft:

Check if already synced:
```bash
grep -rl "source-id: {draft_id}" vault/inbox/drafts/
```

If a file is found: update its `synced` field using the Edit tool.

If no file found: create `vault/inbox/drafts/YYYY-MM-DD-{slug}.md` where
`YYYY-MM-DD` is the `creationDate` (first 10 chars) and `{slug}` is a
kebab-case version of the title (max 60 chars).

File content:
```
---
created: YYYY-MM-DD
flagged: true/false
modified: YYYY-MM-DD
source: drafts
source-id: {draft_id}
source-url: {permalink field from MCP}
synced: YYYY-MM-DDTHH:MM
tags: [list]   # omit if empty
title: {title}
---

{content — full draft body}
```

On failure: `Drafts: FAILED — MCP unavailable`

---

## Connector 4: Gmail

Use Gmail MCP tools to sync unread inbox threads.

**4a.** Call `gmail.search_threads` with query `"is:unread in:inbox"`.

**4b.** For each thread in the results:

Check if already synced:
```bash
grep -rl "source-id: {thread_id}" vault/inbox/gmail/
```

If a file is found: update its `synced` field using the Edit tool.

If no file found: create `vault/inbox/gmail/YYYY-MM-DD-{slug}.md` where
`YYYY-MM-DD` is the date of the most recent message and `{slug}` is a
kebab-case version of the subject (max 60 chars).

File content:
```
---
date: YYYY-MM-DD
from: {sender name and address}
source: gmail
source-id: {thread_id}
source-url: https://mail.google.com/mail/u/0/#inbox/{thread_id}
subject: {subject line}
synced: YYYY-MM-DDTHH:MM
unread: true
---

{snippet — first 300 chars of the most recent message body}
```

On failure: `Gmail: FAILED — MCP unavailable`

---

## Output

After all four connectors complete, report:

```
Sync complete — {YYYY-MM-DD HH:MM}
─────────────────────────────────
Things:  N new, N updated   [or: FAILED — reason]
Day One: N new, N updated   [or: FAILED — reason]
Drafts:  N new, N updated   [or: FAILED — reason]
Gmail:   N new, N updated   [or: FAILED — reason]
```

If any connector failed, offer:
> "Would you like me to retry the failed connectors, or check the error?"

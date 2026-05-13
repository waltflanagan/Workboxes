# /morning — Daily Briefing Skill

Interactive morning briefing. Walks through context domain by domain, pausing
after each section for the user's input before moving on. This skill also
handles thought capture at any point in the conversation.

---

## Thought capture (any time this skill is active)

If the user says anything starting with "note:", "capture:", or "remember:" —
or says "add to scratch" — immediately append a timestamped entry to
`vault/daily/scratch/YYYY-MM-DD.md` (today's date) before responding:

```
- **HH:MM** {thought}
```

Create the file if it doesn't exist:
```
---
date: YYYY-MM-DD
type: daily-scratch
---

## Captures

```

Respond with only: "Captured." Then continue where you left off.

---

## Step 1 — Archive + retrospective

**1a.** Run:
```bash
python3 -c "
from daily.daily import archive_current
result = archive_current()
print(result or 'no-current')
"
```

**1b.** If the result is not `no-current`, the printed path is the archive file.
Read it. Extract unchecked intentions — substitute the actual path from step 1a:
```bash
python3 -c "
from daily.daily import get_unchecked_intentions
from pathlib import Path
items = get_unchecked_intentions(Path('vault/daily/2026-04-22.md').read_text())
print('\n'.join(items) if items else 'none')
"
```
(Replace `vault/daily/2026-04-22.md` with the actual path printed in step 1a.)

If there are unchecked items, present them:
> "Yesterday you intended to: {list}. Did any of these happen?"

Wait for the user's answer. Build two lists from the response:
- `completed`: items the user confirms happened
- `carried`: items the user says did not happen (or skipped)

**1c.** Call `create_current` with the `carried` list. Construct the Python
list from the carried items collected above. For example, if "Write Oscar
summary" and "Call therapist" were not done:
```bash
python3 -c "
from daily.daily import create_current
from datetime import date
create_current(today=date.today(), carried_items=['- [ ] Write Oscar summary', '- [ ] Call therapist'])
"
```
If no items were carried (or _current.md did not exist), call with no carried_items:
```bash
python3 -c "
from daily.daily import create_current
from datetime import date
create_current(today=date.today())
"
```

If `_current.md` did not exist (first run), create it with no carried items.

Say: "Yesterday archived. Ready to process any scratch — want to do that now,
or skip ahead?"

---

## Step 2 — Process unprocessed scratch

Run:
```bash
python3 -c "
from daily.daily import list_unprocessed_scratch
for p in list_unprocessed_scratch():
    print(p)
"
```

If no files are listed: "No scratch to process. Moving on."

For each unprocessed scratch file, read it and present each capture entry one
at a time:
> "Capture: '{thought}' — where does this live?
> Options: **relationship** / **adhd** / **discernment** / **therapy** /
> **new note** / **defer** / **discard**"

Route based on the user's choice:

| Choice | Action |
|--------|--------|
| `relationship` | Append as a task to `vault/relationship/TASKS.md` under `## Now` |
| `adhd` | Append to today's `vault/adhd/daily/YYYY-MM-DD.md` in the relevant section |
| `discernment` | Append a note to `vault/discernment/dashboard.md` |
| `therapy` | Append to the most recent file in `vault/therapy/` |
| `new note` | Ask for a title and target folder, then create the note with Write tool |
| `defer` | Leave in scratch; move to next capture |
| `discard` | Skip; no write |

After processing all captures in a file, mark it processed:
```bash
python3 -c "
from daily.daily import mark_scratch_processed
from pathlib import Path
mark_scratch_processed(Path('{path}'))
"
```

---

## Step 3 — Domain: ADHD / Recovery

Read `vault/adhd/daily/YYYY-MM-DD.md` (today's date). If the file does not
exist, create it by copying `vault/adhd/daily/_template.md` and replacing
`{{date}}` with today's date using the Write tool.

Surface:
- **Morning anchor**: is it checked? Is medication noted?
- **Yesterday's microcommitments**: what was tallied in the previous day's file?
- **Open yellow lights or urges**: any from yesterday's note?

Present as 3-5 lines. Example:
> "ADHD check-in: Morning anchor not yet marked. Medication not noted.
> Yesterday you tallied 2 microcommitments. No open yellow lights.
> Anything to note here?"

Wait for input. If the user provides something, edit the relevant section of
today's ADHD daily note using the Edit tool.

---

## Step 4 — Domain: Relationship

Read `vault/relationship/TASKS.md`. Identify all unchecked items in `## Now`
and `## Soon`.

List files in `vault/relationship/timeline/` sorted by modification date.
Find any modified in the last 7 days.

Present:
- Up to 3 open tasks (title and due date if present)
- Recent timeline files by date and slug (names only, not content)

Example:
> "Relationship: 2 open tasks — 'Write Oscar summary (no fixed date)',
> 'Schedule autism assessment (no fixed date)'. Recent timeline: Apr 21
> (iMessage thread). Anything to update?"

Wait for input. Offer: "Want me to add any inbox items here?"

---

## Step 5 — Domain: Therapy / Discernment

Read `vault/discernment/dashboard.md`.
List files in `vault/therapy/` sorted by modification date (most recent first).

Surface:
- Any upcoming session dates found in dashboard or therapy files
- Open prep items or open questions from discernment dashboard

Present in 2-4 lines. Wait for input.

---

## Step 6 — Domain: External inbox

Find all files in `vault/inbox/` that do NOT have a `promoted:` field in their
frontmatter:

```bash
python3 -c "
import yaml
from pathlib import Path

inbox = Path('vault/inbox')
unpromoted = []
for f in sorted(inbox.rglob('*.md')):
    content = f.read_text()
    if content.startswith('---'):
        parts = content.split('---', 2)
        try:
            fm = yaml.safe_load(parts[1]) or {}
        except Exception:
            fm = {}
        if 'promoted' not in fm:
            unpromoted.append({'path': str(f), 'source': fm.get('source','?'), 'title': fm.get('title') or fm.get('subject','?'), 'date': fm.get('date','?')})
for item in unpromoted:
    print(item['source'], '|', item['date'], '|', item['title'], '|', item['path'])
"
```

Group by source. Present counts and up to 5 titles per source:
> "Inbox: Things (12 items), Day One (2 entries), Drafts (3 flagged), Gmail
> (4 unread threads). Want to work through any of these?"

If the user wants to route an item, for each one they name:
1. Ask: "Promote to relationship / adhd / discernment / therapy / discard / defer?"
2. On promote: append the item title to the relevant workspace file and add
   `promoted: {target_path}` to the inbox file's frontmatter using Edit tool.
3. On discard: add `promoted: dismissed` to frontmatter using Edit tool.
4. On defer: no change.

---

## Step 7 — Set today's intentions

Based on the full picture from steps 1-6, offer 3-5 intention suggestions.
Phrase them in first-person, identity-based language (e.g.,
"I show up for Oscar's scheduling today").

Present them and ask the user to confirm or adjust.

Once agreed, write them to `vault/daily/_current.md` under
`## Today's intentions` using the Edit tool (replace the placeholder `- [ ]`
line with the agreed intentions as checkboxes).

Offer a handoff:
> "Intentions set. Anything you want to dig into now? I can go deeper on ADHD
> (adhd-coach), relationship, or discernment — or we're done."

If the user names a domain, transition naturally into that work or invoke the
relevant skill. If done, close the briefing.

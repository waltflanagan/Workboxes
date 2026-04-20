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

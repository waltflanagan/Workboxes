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

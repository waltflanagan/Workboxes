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

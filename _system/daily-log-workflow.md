# Daily Log Workflow

The daily log is the whole-workspace daily operating layer. It is separate from
Growth reviews: Growth reviews focus on values/goals/practices; daily logs
record what happened across Workboxes and external tools.

Daily logs live in `Daily/YYYY-MM-DD.md`.

## Operating model

- `/daily-start` opens the day: review yesterday, look ahead to today, create or
  update today's daily log.
- `/daily-review` closes the day: reflect on what happened, extract follow-ups,
  and capture trend signals.
- `Daily/YYYY-MM-DD.md` is the landing page for the day. Keep raw context here
  until it is routed to GitHub issues, Things, or Workboxes notes.
- GitHub issues remain the durable backlog. Things remains the execution layer.
  Workboxes area/project files remain the deeper context layer.

## Sources to check

Use available connectors opportunistically. Missing tools should be recorded as
unavailable, not treated as blockers.

- Workboxes: `_system/TOP_OF_MIND.md`, `Inbox/`, recent `Areas/` edits,
  yesterday's and today's `Daily/` files.
- GitHub: open issues, top-of-mind labels, waiting/blocked labels, project
  changes when accessible.
- Calendar: yesterday's events, today's events, conflicts, open prep.
- Email: unread or important Gmail threads, school email if relevant.
- Slack: mentions, DMs, important channel activity when connected.
- Things: due today, overdue, completed yesterday, created yesterday when
  available.
- Drafts / Day One: recent captures or entries when available.

## Morning flow

1. Read `_system/TOP_OF_MIND.md`, this file, and `Daily/_template.md`.
2. Check for `Daily/<yesterday>.md` and `Daily/<today>.md`.
3. If today's file does not exist, create it from `Daily/_template.md`.
4. Review yesterday:
   - what was planned
   - what happened
   - what slipped
   - unresolved follow-ups
5. Look ahead to today:
   - calendar and time constraints
   - top-of-mind items needing attention
   - waiting/blocked items that may unblock
   - inbox/tool items that need capture
6. Ask Mike for 1-3 intentions. Keep them concrete and values-aligned.
7. Update today's daily log with the morning snapshot and intentions.

## During-day capture

Append quick events under `## Timeline` or `## Captures`.

Use this shape:

```markdown
- HH:MM - <what happened> #area/<area> #person/<name> #signal/<signal>
```

Only add tags that help later detection. Prefer stable tags over novelty:
`#signal/energy`, `#signal/avoidance`, `#signal/repair`,
`#signal/waiting`, `#signal/slipped`, `#signal/follow-up`,
`#person/oscar`, `#person/roxy`.

## Evening flow

1. Read today's daily log, `_system/TOP_OF_MIND.md`, and open GitHub issues if
   accessible.
2. Review tool activity since morning.
3. Ask short reflection prompts:
   - What actually happened?
   - What mattered?
   - What slipped or needs repair?
   - What gave or drained energy?
   - Any relationship, family, work, or self signals worth tracking?
4. Update the daily log's end-of-day sections.
5. Propose follow-ups:
   - GitHub issues to create/update/close
   - Things tasks to create/update
   - Workboxes notes to update
   - `TOP_OF_MIND.md` changes
6. Ask before making structural changes or cross-system updates.

## Trend detection

Trend detection comes from repeated daily tags and reflection notes. Daily
review should identify candidates, not overfit one day.

Good trend candidates:

- same issue appears three or more times in a week
- repeated missed intention
- repeated waiting/blocking dependency
- energy drain or restoration pattern
- repeated relationship/family signal
- recurring tool friction or capture leak

Weekly review can scan `Daily/*.md` from the last seven days and promote stable
patterns into Growth themes, GitHub issues, or Workboxes project notes.

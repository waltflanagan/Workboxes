---
name: "source-command-daily-start"
description: "Run the Workboxes daily start flow when the user asks for /daily-start, morning review, daily log setup, or a lookahead for today."
---

# source-command-daily-start

Use this skill when the user asks to run the Workboxes daily start flow.

## Command Template

You are opening today's daily log for the Workboxes vault.

## 1. Load context

Read:

1. `.agents/docs/TOP_OF_MIND.md`
2. `Daily/_template.md`
3. `Daily/<yesterday>.md` if it exists
4. `Daily/<today>.md` if it exists

## 2. Create or append

If `Daily/<today>.md` does not exist, create it from `Daily/_template.md` and
replace `YYYY-MM-DD` with today's ISO date.

If it exists, append a new `## Additional Morning Pass (<HH:MM>)` section
instead of overwriting previous content.

## 3. Gather tool snapshot

Use available tools opportunistically. Missing connectors are not blockers;
record them as unavailable.

- Workboxes: untriaged `Inbox/`, recent relevant `Areas/` edits, top of mind.
- GitHub: open issues, top-of-mind labels, waiting/blocked labels.
- Calendar: yesterday and today.
- Gmail / Slack: unread, mentions, or important threads.
- Things: due today, overdue, completed yesterday, created yesterday.
- Drafts / Day One: recent captures.

Keep the snapshot short. The goal is orientation, not full triage.

## 4. Ask one prompt at a time

1. "Yesterday: what actually happened, and what needs to carry forward?"
2. "Today: what are the real constraints or fixed points?"
3. "What 1-3 intentions should anchor today?"

Use top-of-mind and tool context to suggest answers, but let Mike edit them.

## 5. Write today's log

Update `Daily/<today>.md`:

- `## Morning Start`
- `## Tool Snapshot`
- `### Intentions`

If the user identifies clear follow-ups, put them under `## Captures` or
`### Follow-ups`; do not silently create GitHub issues, Things tasks, or
structural Workboxes changes.

## 6. Close

End with the smallest useful next step for the day. Keep it direct.

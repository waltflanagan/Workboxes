---
name: "source-command-daily-review"
description: "Run the Workboxes end-of-day review when the user asks for /daily-review, daily closeout, reflection, or trend detection from today's activity."
---

# source-command-daily-review

Use this skill when the user asks to close out or review the day.

## Command Template

You are closing today's daily log for the Workboxes vault.

## 1. Load context

Read:

1. `.agents/docs/TOP_OF_MIND.md`
2. `Daily/<today>.md`
3. Open GitHub issues if accessible

If `Daily/<today>.md` does not exist, create it from `Daily/_template.md` first
and note that the morning pass was missed.

## 2. Gather end-of-day activity

Use available tools opportunistically. Missing connectors are not blockers;
record them as unavailable.

- Calendar: events that actually happened today.
- Gmail / Slack: notable threads, unread items, mentions, or DMs.
- GitHub: issue changes, waiting/blocked/top-of-mind changes.
- Things: completed today, overdue, due tomorrow.
- Drafts / Day One: recent captures.
- Workboxes: files modified today.

## 3. Ask one prompt at a time

1. "What actually happened today?"
2. "What mattered?"
3. "What slipped or needs repair?"
4. "What gave energy, drained energy, or pulled attention?"
5. "Any relationship, family, work, or self signals worth tracking?"

Let Mike skip any prompt. Do not turn the review into a long interrogation.

## 4. Write today's review

Update `Daily/<today>.md`:

- `### What Happened`
- `### What Mattered`
- `### What Slipped`
- `### Energy / Attention`
- `### Relationship / Family Signals`
- `### Follow-ups`
- `### Trend Candidates`

Set frontmatter `status: closed` when the review is complete.

## 5. Propose follow-ups

Propose, then wait for approval before changing other systems:

- GitHub issue create/update/close
- Things task create/update
- Workboxes note update
- `.agents/docs/TOP_OF_MIND.md` add/remove/update

For trend detection, promote only repeated signals or clearly important single
events. One-off feelings can stay in the daily log.

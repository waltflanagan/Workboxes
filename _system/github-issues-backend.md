# GitHub Issues Backend

GitHub Issues is the durable backlog for Workboxes. Use it for anything worth
remembering, revisiting, deciding, or someday doing.

Things is the execution layer. Use Things only for actual next actions Mike is
trying to take: call, text, schedule, pay, write, ask, decide, or follow up by a
date.

## Operating model

- GitHub issue = one backlog atom.
- Workboxes files = context, reference, history, and deeper notes.
- Things task = concrete action under active execution pressure.
- GitHub Projects = dashboard views over the backlog.
- `_system/TOP_OF_MIND.md` = capped surface of the most live issues.

Do not try to recreate Things inside GitHub. GitHub can hold a messy full
backlog. Things should stay small enough to be actionable.

## When to create an issue

Create a GitHub issue when something should not be lost, including:

- a todo that is not ready for Things yet
- an idea or possibility to revisit
- an unresolved question
- a concern occupying attention
- a waiting thread
- an item that belongs on top of mind
- a project or area thread that needs lightweight tracking

Create or update a Workboxes note instead when the content is mainly reference,
history, meeting notes, decisions, or long-form thinking.

Create a Things task when there is a clear next action Mike is actually trying
to execute.

## Labels

Use these labels as the base vocabulary:

- `kind:todo` - backlog item that may become action
- `kind:idea` - idea or possibility to revisit
- `kind:top-of-mind` - active attention item included in the top-of-mind surface
- `area:family` - Family area
- `area:relationship` - Relationship area
- `area:self` - Self and personal growth area
- `area:work` - Work area
- `status:waiting` - waiting on someone or something external
- `status:blocked` - blocked until a condition changes
- `next-action:things` - has or needs a Things task for execution

Add new labels only when the existing vocabulary cannot express a recurring
need. Structural label changes follow propose -> approve -> execute -> log.

## Issue body pattern

Keep issues lightweight. Prefer this shape when creating an issue manually or
through an agent:

```markdown
## What this is

Short description of the thing to remember.

## Current state

What is known right now.

## Next action

Things: none yet

## Links

- Workboxes:
- Things:
```

If the item already has a Things task, put the Things title or URL under
`Next action` and apply `next-action:things`.

## Top of mind

`_system/TOP_OF_MIND.md` remains capped at 15 items. A top-of-mind item should
normally have a matching GitHub issue labeled `kind:top-of-mind`.

When adding a new top-of-mind issue:

1. Confirm there is room under the 15 item cap.
2. If there are 14 or more items, prompt Mike to review what should drop.
3. Create or update the GitHub issue.
4. Add or update the corresponding `TOP_OF_MIND.md` entry with the issue link.

When removing something from top of mind, remove `kind:top-of-mind` from the
issue and update `TOP_OF_MIND.md`. Do not close the issue unless the backlog
item itself is resolved or no longer worth keeping.

## Bridge to Things

Use this bridge rule:

> The issue keeps context; Things carries execution pressure.

When an issue has a real next action:

1. Create or identify the Things task.
2. Link or name the Things task in the issue body.
3. Apply `next-action:things`.
4. Keep the issue open until the underlying thread is resolved, not merely
   until one Things task is checked off.

If the Things task completes but the larger thread remains live, update the
issue with the new current state and remove or revise `next-action:things`.

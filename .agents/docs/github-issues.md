# GitHub Issues

GitHub Issues is the durable backlog. Things is the execution layer. Workboxes files are the context and reference layer.

- **GitHub issue** — one backlog atom. Use for anything worth remembering, revisiting, deciding, or someday doing.
- **Things task** — concrete next action under active execution pressure.
- **Workboxes note** — reference, history, meeting notes, long-form thinking.

The issue keeps context; Things carries execution pressure.

---

## When to create an issue

- A todo not ready for Things yet
- An idea or possibility to revisit
- An unresolved question or decision
- A concern occupying attention
- A waiting or blocked thread
- A top-of-mind item

## When NOT to create an issue

Create a Workboxes note instead when the content is mainly reference or history. Create a Things task when there is a clear next action Mike is actively trying to execute.

---

## GitHub Projects (dashboards)

Three dashboards — do not create more unless there's a strong reason:

- **Life** — self, growth, health, relationships, community, work/career
- **Family** — kids, parenting, school, trips, co-parenting
- **Home** — house, maintenance, operations, money/admin

Person-specific views (Oscar, Nigel, etc.) are saved views filtered by `person:*` labels inside Family — not separate projects.

---

## Label vocabulary

**Kind**
- `kind:todo` — backlog item that may become action
- `kind:idea` — idea or possibility to revisit
- `kind:decision` — unresolved choice
- `kind:top-of-mind` — active attention item surfaced in TOP_OF_MIND.md

**Area**
- `area:family` `area:relationships` `area:growth` `area:health`
- `area:home` `area:finances` `area:personal` `area:work`

**Person**
- `person:oscar` `person:nigel` `person:avery` `person:roxy`

**Status**
- `status:waiting` — waiting on someone or something external
- `status:blocked` — blocked until a condition changes
- `next-action:things` — has or needs a Things task

Add new labels only when the existing vocabulary can't express a recurring need.

---

## TOP_OF_MIND bridge

`TOP_OF_MIND.md` is capped at 15 items. A top-of-mind item should have a matching GitHub issue labeled `kind:top-of-mind`.

When adding: confirm room, create/update the issue, add to TOP_OF_MIND.md with the issue link.  
When removing: remove `kind:top-of-mind` from the issue, update TOP_OF_MIND.md. Don't close the issue unless the underlying thread is resolved.

---

## Things bridge

When an issue has a real next action:

1. Create or identify the Things task.
2. Note it in the issue body under `## Next action`.
3. Apply `next-action:things`.
4. Keep the issue open until the thread — not just the task — is resolved.

If the Things task completes but the larger thread remains live, update the issue's current state and revise or remove `next-action:things`.

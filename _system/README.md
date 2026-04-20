# Workboxes — System README

Claude reads this file first in every session inside the Workboxes vault.

## What this vault is

A personal life-management system. Captures from native iOS/macOS tools land
in `Inbox/` and are triaged into workboxes under `Areas/<area>/Projects/<project>/`.
Claude operates the vault using the files in `_system/`.

## Operating files

- **`workbox-schema.md`** — authoritative conventions (folders, frontmatter,
  MOC templates, naming). Read this when creating new content or validating
  structure.
- **`triage-rules.md`** — learned routing rules. Read this during triage to
  find high-confidence matches.
- **`voice.md`** — user voice and preferences. Consult before producing any
  user-facing text (summaries, proposals, questions).
- **`capture-sources.md`** — inventory of capture sources, their provenance
  fields, and setup quirks. Consult when normalizing or debugging a source.
- **`growth-prompts.md`** — prompt content for `/review-daily`,
  `/review-weekly`, and `/review-quarterly`. Edit to tune review
  voice or prompts.
- **`growth-surfacing.md`** — scan rules `/triage` uses to suggest
  Growth links and dispositions. Extend as new patterns emerge.
- **`changelog.md`** — append-only log of structural changes to the vault.
  Append here after any workbox creation, archive, rule addition, or schema
  tweak.
- **`sync-state.json`** — per-source sync high-water-marks. The only non-
  markdown file in the vault; machine-managed by pull-sync scripts.

## Active areas

- `Areas/Work/` — Etsy, team, career.
- `Areas/Family/` — partner, kids, parenting, extended family.
- `Areas/Relationships/` — friends, community, social commitments.
- `Areas/Home/` — house, maintenance, household logistics.
- `Areas/Health/` — physical, mental, appointments, fitness.
- `Areas/Growth/` — values, goals, intentions, therapy notes, practices, reviews.
- `Areas/Finances/` — money, taxes, accounts, major decisions.
- `Areas/Personal/` — hobbies, reading, learning, personal projects.

Archived areas and projects live under `Archive/` mirroring the `Areas/`
structure.

## How to run triage

Invoke `/triage` (Claude Code slash command, defined in
`.claude/commands/triage.md`). It implements the procedure from the spec:
load context, propose dispositions, walk items with the user, execute,
extract rules, log.

## How to run Growth reviews

- `/review-daily` — quick morning check-in.
- `/review-weekly` — Sunday pass over the week.
- `/review-quarterly` — end-of-quarter reset.

See `_system/growth-prompts.md` for what each review loads and asks.

## How to create Growth content

`/growth-new <type>` — prompts for slug and type-specific fields,
then creates the file. Types: `value`, `goal`, `intention`,
`practice`, `theme`, `therapy-session`. See
`_system/workbox-schema.md` for frontmatter.

## How the system evolves

All structural changes follow **propose → approve → execute → log**. Never
silent. See `workbox-schema.md` for the full evolution rules.

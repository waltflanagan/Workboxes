# Workboxes — Design

**Date:** 2026-04-18
**Owner:** msimons@etsy.com
**Status:** Approved for implementation planning (Phase 0–2 scope)

## Summary

A personal life-management system organized as an Obsidian vault on disk, operated by Claude as the reasoning engine. Captures from native iOS/macOS tools land in an Inbox and are triaged by Claude into a nested Areas → Projects taxonomy ("workboxes") inspired by the workbox concept in *Unapologetically ADHD*. Index files (MOCs) embedded in the vault direct Claude's navigation. The system evolves as it's used: workbox structure, triage rules, voice/preferences, and navigation hints all grow through a uniform propose → approve → execute → log pattern.

Day 1 scope is **capture and triage**. The architecture is designed so **active workbox working memory** and **long-term memory / second brain** can layer in later without restructuring.

## Goals

- **Capture & triage (Day 1):** Low-friction capture from native tools, LLM-assisted triage into workboxes.
- **Working memory (later):** "Open this workbox" produces a current-state briefing (open loops, recent activity, context).
- **Long-term memory (later):** Durable, distilled, navigable knowledge about life, projects, people, decisions.

## Non-goals

- No embeddings, vector DB, or ML classifier in Day 1 or near-term. Evolution is editorial, not statistical.
- No background daemon or event-driven triage. Triage is human-invoked (Day 1) or scheduled (Phase 4).
- No team/multi-user features. Single-user, local vault.
- No cloud-hosted service. Everything is local files in an Obsidian vault, optionally sync'd via Obsidian Sync / iCloud / Dropbox per the user's existing Obsidian setup.

## Architecture (Approach 1: Vault-native, Claude-navigable)

The vault *is* the system. Everything — content, indices, operating rules, workbox definitions — lives as markdown files under Obsidian conventions. Claude is the engine; it reads index files, edits files, and moves files. No database, no separate service, no daemon.

A sidecar SQLite index (Approach 2) and an embeddings layer remain available as future bolt-ons. The vault layout is designed so neither requires moving files.

### Vault location

Vault root: `/Users/msimons/Code/Workboxes/`. Git-tracked.

### Top-level folder layout

```
Workboxes/
├── _system/         System files Claude reads to operate. Underscore prefix
│                    sorts to top in Obsidian; marks as non-content.
├── Inbox/           All captures (push & pull) land here as flat .md files
│                    with provenance frontmatter. Nothing lives here long-term.
├── Areas/           Life areas — stable, few, hand-curated. Each area is a
│                    folder; projects live in Areas/<area>/Projects/<slug>/.
├── Archive/         Mirrors Areas/ structure. Closed projects/areas move
│                    here; searchable, doesn't clutter active views.
├── Attachments/     Obsidian default for binaries (images, PDFs).
├── Daily/           Obsidian daily notes (optional; future surface for
│                    goal B working-memory briefings).
└── docs/            This design doc and future implementation plans.
```

### Starter Areas

Created in Phase 0:

- **Work** — Etsy, team, career.
- **Family** — partner, kids, parenting, extended family.
- **Relationships** — friends, community, social commitments.
- **Home** — house, maintenance, household logistics.
- **Health** — physical, mental, appointments, fitness.
- **Finances** — money, taxes, accounts, major decisions.
- **Personal** — hobbies, reading, learning, personal projects.

These are a starting taxonomy, not a fixed one. Merging, splitting, renaming, and archiving areas are supported by the evolution mechanics (see below).

### Conventions

- Folder and file names use `kebab-case-slugs` (LLM-friendly, URL-friendly, sync-safe).
- Every folder Claude navigates contains an `_index.md` (MOC). Underscore prefix sorts to top.
- An area/project is "active" if under `Areas/`; "archived" if under `Archive/`. Move = change state. No status field on content files.
- Frontmatter `type:` distinguishes `inbox-item | project-index | area-index | note | log-entry | decision | reference | memory | system`.
- Obsidian tags remain free-form. Triage rules can consume tags but do not enforce a taxonomy.

## File formats

### Inbox item

Path: `Inbox/<YYYY-MM-DD>-<HHMM>-<slug>.md`

```markdown
---
type: inbox-item
captured: 2026-04-18T14:30:00-04:00
source: dayone | drafts | web-clipper | raw | gmail | things | voice-memo | messages
source_id: <original identifier if applicable>
source_url: <URL if applicable>
tags: []
---

<content as captured, unmodified>
```

- Filename encodes capture time → naturally chronological in Obsidian.
- Claude never edits the body during triage — it moves the file.
- Missing frontmatter (from a raw drop) is treated as `source: raw`, with `captured` inferred from file mtime.

### Workbox content file

Path: `Areas/<area>/Projects/<project>/<slug>.md` (or `Areas/<area>/<slug>.md` for area-level notes)

```markdown
---
type: note | log-entry | decision | reference
created: 2026-04-18T14:30:00-04:00
source: <carried from inbox if applicable>
source_id: <carried from inbox if applicable>
tags: []
links: [[related-note]]
---

<content — may be cleaned up or distilled after triage>
```

### Index file (MOC)

Path: `_index.md` in any area or project folder.

```markdown
---
type: area-index | project-index
status: active | dormant | archived
owner_notes: <free text Claude updates — "current focus", "waiting on X">
---

# <Name>

## Claude: Read this first
<Navigation hints specific to this workbox. Grows as the workbox matures.
E.g., "Decisions live in decisions.md; people in people.md.">

## Open loops
- [ ] ...          ← maintained by Claude during triage

## Projects (area indices only)
- [[project-one]] — one-line description
- [[project-two]] — one-line description

## Recent activity
- 2026-04-18 — <auto-updated summary line>
```

The "Claude: Read this first" section is the mechanism that lets navigation evolve (e5). It is content, not code; it can grow organically per workbox.

## `_system/` — operating manual

Files Claude reads to operate the vault. Lives inside the vault so it syncs with Obsidian and is hand-editable.

```
_system/
├── README.md              Entry point. Claude reads this first in every
│                          session. Describes the vault, links the other
│                          _system files, lists active Areas.
├── triage-rules.md        Learned routing rules (e2). Append-only during
│                          triage; Claude proposes, user approves, rules
│                          are written here.
├── voice.md               User voice/preferences (e3). Seeded minimal;
│                          grows from explicit feedback.
├── workbox-schema.md      Conventions: folder layout, frontmatter, MOC
│                          structure. Condensed, actionable form of this
│                          spec.
├── capture-sources.md     Inventory of capture sources and their quirks
│                          (provenance mapping, photo inlining notes,
│                          known edge cases).
├── changelog.md           Append-only log of structural changes — areas
│                          created/archived, rules added, schema tweaks.
└── sync-state.json        Per-source sync high-water-marks (the only
                           deliberate non-markdown file in the system).
```

### Read order

1. Session start → read `README.md`. It points at the rest.
2. During triage → load `triage-rules.md`; scan `Areas/*/_index.md` (frontmatter + first section) for taxonomy.
3. Before writing user-facing text → consult `voice.md`.
4. After any structural change → append to `changelog.md`.

### Triage rule format

Rules in `triage-rules.md` are human-readable markdown blocks:

```markdown
## Rule: Day One gratitude entries → Personal/Gratitude
Added: 2026-04-18
Condition: source = dayone AND tags contains "gratitude"
Action: route to Areas/Personal/Projects/gratitude
Confidence: high
```

Rules can be edited or deleted by hand. High-confidence rules are what drives the Phase 4 graduation to hybrid auto-triage.

## Triage workflow

Core operation. Day 1 implements the **conversational** variant; Phase 4 graduates to the **hybrid** variant.

### Trigger

User says "triage my inbox" (or invokes an equivalent slash command / skill; exact surface defined in the implementation plan).

### Procedure

1. **Load context.** Read `_system/README.md` → `triage-rules.md` → each `Areas/*/_index.md` (frontmatter + first section only — cheap, gives full taxonomy).
2. **List inbox.** Read all files in `Inbox/` sorted by capture time; load frontmatter + body.
3. **Propose disposition for each item.** One of:
   - **Route** — move to a specific project/area with a one-line rationale.
   - **New project** — propose a project under an area.
   - **New area** — propose a new area (rare; explicit approval required).
   - **Discard** — duplicate or test capture; explicit approval required.
   - **Defer** — insufficient signal; remain in Inbox with `triaged_deferred_at` frontmatter timestamp.
4. **Apply rules.** Any item matching a `triage-rules.md` rule is flagged high-confidence.
5. **Walk items with user.** Show proposals; user approves / edits / redirects. Batch obvious ones.
6. **Execute approved dispositions.**
   - Move files with filesystem operations (commit to git).
   - Update target `_index.md`: append to "Recent activity", update "Open loops" where applicable.
   - If a new project/area was created, scaffold its `_index.md` from `workbox-schema.md` template; add link in parent `_index.md`.
7. **Extract rules.** When the user overrides a proposal in a pattern-y way, Claude proposes a rule for `triage-rules.md`. User approves.
8. **Log.** Append summary entry to `_system/changelog.md` (N items triaged, rules added, areas/projects created, archives).

### Graduation to hybrid (Phase 4)

Same procedure with two additions: (a) a confidence threshold auto-executes rule-matched routes; (b) a scheduled variant runs on cron/scheduled-task, produces a morning summary note in `Daily/` listing auto-actions taken and leaving unmatched items for an interactive session. This is a config flip, not a rewrite.

### Non-features

- No automated "undo" beyond git. Every triage is one or more commits; revert via normal git.
- No partial content triage. Items move whole; distillation is a separate future workflow (Phase 7).
- No classifier training. Rules come from user decisions, not data.

## Capture pipelines (Day 1 subset)

Every source converges on `Inbox/` with uniform frontmatter. Sources differ only in provenance fields.

### Push: Obsidian Web Clipper

Configured with a template that emits the inbox-item frontmatter shape. Saves to `Inbox/<YYYY-MM-DD>-<HHMM>-<title-slug>.md`. No Claude involvement at capture time. Configuration is a manual setup step documented in `capture-sources.md`.

### Push: Drafts → Obsidian action

A Drafts action (configured once) appends a new file to `Inbox/` with frontmatter:

```yaml
source: drafts
source_id: <draft uuid>
```

Triggerable from Drafts share sheet, voice dictation, or the Drafts inbox. Covers both iOS and macOS. Action configuration documented in `capture-sources.md`.

### Push: Raw text

Drop any `.md` file (or text to be saved as `.md`) into `Inbox/`. Missing frontmatter is tolerated — triage treats it as `source: raw` and infers `captured` from mtime. Escape hatch for share-sheet-to-Files-app, drag-and-drop, manual paste.

### Pull: Day One

A `sync-dayone` script (shape TBD in implementation plan — likely a shell script calling the Day One CLI / MCP) runs on demand or scheduled. It:

1. Reads the last-sync timestamp from `_system/sync-state.json` under the `dayone` key.
2. Fetches Day One entries with `createdDate` newer than that timestamp.
3. For each entry: inline photos to `Attachments/` with relative markdown links; emit `Inbox/<YYYY-MM-DD>-<HHMM>-<slug>.md` with:

```yaml
type: inbox-item
captured: <entry createdDate>
source: dayone
source_id: <entry uuid>
tags: [<dayone tags>]
```

4. Update `_system/sync-state.json` with the newest processed timestamp.

The vault is self-contained — photos are inlined, not referenced by URL.

### Deferred sources

Gmail, Things, Voice Memos (requires transcription step), Messages, and any additional push paths follow the same contract: write markdown with provenance frontmatter into `Inbox/`, update `sync-state.json`. No changes to triage or vault structure required to add them.

## Evolution mechanics

Every evolution follows the same shape: **propose → approve → execute → log**. No silent mutations.

- **e1 — Workbox structure.** Claude proposes new projects (common) and new areas (rare). On approval: create folder, seed `_index.md` from template, link from parent, append to `changelog.md`. Archive is the reverse: move under `Archive/` mirroring path, update parent indices, log.
- **e2 — Triage rules.** Claude proposes rule extractions during triage (in response to user overrides); user approves; rule appended to `triage-rules.md`. Rules are plain markdown — hand-editable.
- **e3 — Voice.** `voice.md` is seeded near-empty. When user gives feedback ("be terser", "use first names, not 'partner'"), Claude proposes a line to add; user approves.
- **e4 — Long-term memory graduation.** *(Deferred to Phase 7.)* Each project MOC reserves a `memory.md` slot. A future "distill" workflow reads project logs and produces durable `memory.md` artifacts. `memory.md` survives archiving as the project's compressed form. Day 1 only reserves the filename and the structural slot.
- **e5 — Navigation hints.** The "Claude: Read this first" section of each MOC is updated by Claude (with user approval) as a workbox matures — e.g., proposing a split when a log grows long, and updating the MOC to reflect the new layout.

The `changelog.md` file is the complete history of how the system has changed shape. It is itself valuable context for Claude in future sessions.

## Phase 6 extension — growth, values, therapy

**Deferred; requires a dedicated mini-design pass before implementation.** User intends to incorporate personal growth work, life goals, values, and therapy material into the system. This material carries sensitivity considerations the general system does not:

- Sync/storage: may warrant a private sub-area with excluded sync or encrypted attachments.
- Distillation rules: values and therapy notes are not "filed and forgotten" — they want recurring surfacing.
- Capture shape: may benefit from a structured template rather than free-form inbox items.

Design deliberately does not commit to an Area name or pattern now. Options to evaluate in Phase 6: (a) new top-level Area (`Growth`), (b) pattern within `Personal`, (c) parallel tree outside `Areas/` with its own conventions. Decision made when we approach that work.

## Phasing & Day 1 scope

- **Phase 0 — Vault scaffolding.** Git init; create folder skeleton; seed `_system/` files; seed starter Area `_index.md` files; first commit.
- **Phase 1 — Capture (push side).** Document Web Clipper template, Drafts action; verify raw-text path.
- **Phase 2 — Triage workflow (conversational).** Define triage as a slash command / skill; first end-to-end run; iterate `_system/` files from observed friction.
- **Phase 3 — Day One pull sync.** Build `sync-dayone` with `sync-state.json` tracking and photo inlining.
- **Phase 4 — Graduate to hybrid triage.** Add confidence threshold; add scheduled variant; emit morning summary to `Daily/`.
- **Phase 5 — Additional sources.** Gmail, Things, Voice Memos (with transcription), Messages. Any order; each drop-in.
- **Phase 6 — Growth / values / therapy extension.** Dedicated mini-design before building.
- **Phase 7 — Working memory + long-term memory.** Distillation workflow (logs → `memory.md`); "open workbox" briefing assembly; archive flow preserves `memory.md`.
- **Phase 8 (optional) — SQLite sidecar.** Only if vault size makes index-file navigation slow. Drop-in, no restructure.

**Day 1 deliverable = Phase 0 + documented Phase 1 + first working Phase 2.** Everything else is designed-for but deferred. The implementation plan generated from this spec covers Phase 0–2.

## Open questions for the implementation plan

- Exact surface for triage invocation — slash command, skill, both?
- Whether a `skills/` folder inside the vault (portable, travels with the vault) or skill files under `~/.claude/` (per-user, not vault-coupled) — depends on preferred portability.
- Whether triage commits are one-commit-per-session or one-per-item.
- Day One access path — CLI vs MCP — determined when Phase 3 is planned.

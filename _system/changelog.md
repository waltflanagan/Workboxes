# Vault Changelog

Append-only log of structural changes: workboxes created or archived, rules
added or removed, schema tweaks. Newest entries at the bottom.

## Entries

### 2026-04-18 — System initialized

- Created vault folder skeleton (Task 1).
- Seeded seven starter Areas: Work, Family, Relationships, Home, Health,
  Finances, Personal.
- Seeded `_system/` operating files.

### 2026-04-18 — Triage session

- Triaged 3 items: 3 routed, 0 new projects created, 0 discarded, 0 deferred.
- New rules: none.
- New workboxes: none.

### 2026-04-19 — Phase 6a: Growth area

- Scaffolded `Areas/Growth/` with seven content subfolders (Values,
  Goals, Intentions, Themes, Therapy, Practices, Reviews) and their
  `_index.md` MOCs.
- Extended `_system/workbox-schema.md` with Growth content type
  schemas, naming rules, the append exception, and status lifecycle.
- Added `_system/growth-prompts.md` (daily/weekly/quarterly review
  prompts; monthly and annual deferred to Phase 6b).
- Added `_system/growth-surfacing.md` (Day-1 slug/alias scan rules
  for triage; lexicons and fuzzy matching deferred to Phase 6b).
- Added slash commands: `/growth-new`, `/review-daily`,
  `/review-weekly`, `/review-quarterly`.
- Extended `/triage` with Route-to-Growth disposition (new
  reflection / new theme / append) and growth-surfacing scans.
- Updated `_system/README.md` with Growth area, operating files, and
  command inventory.

### 2026-05-07 — School email triage pipeline

- Created `Areas/Family/Projects/{oscar,nigel,avery}/_index.md`.
- Added `_system/school-email-senders.md` (sender config; user must fill in real addresses).
- Added `.claude/commands/school-email-triage.md` (dedicated command).
- Extended `_system/workbox-schema.md` with `school-email` frontmatter type.
- Activated Gmail in `_system/capture-sources.md` (school email pipeline, not general inbox).
- Added 4 routing rules to `_system/triage-rules.md` (one per child + district).
- Updated `Areas/Family/_index.md` with three child projects.
- Seeded `_system/sync-state.json` with null high-water marks for all senders.

### 2026-04-20 — Agent entrypoint

- Added `AGENTS.md` at vault root pointing agents to `_system/README.md`.
- Symlinked `CLAUDE.md` → `AGENTS.md` so Claude Code and opencode both
  auto-load the entrypoint. Fixes silent failure where agents started in
  the vault without reading the operating manual.

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

### 2026-05-08 — Amanda Fisher + therapist search details

- Created `Resources/People/amanda-fisher.md` (first People entry).
- Updated `Resources/People/_index.md` with her entry.
- Created `Areas/Family/Projects/oscar-therapist-search/timeline.md` with
  event log, follow-up dates, and full referral profile from Amanda's message.
- Updated `oscar-therapist-search/_index.md` and `log.md` with referral details.

### 2026-05-08 — Resources wiki

- Created `Resources/_index.md` (wiki root MOC).
- Created `Resources/People/_index.md` (CRM-style people directory).
- Extended `_system/workbox-schema.md` with `Resources/` layout, `person`
  frontmatter schema, body structure, and update rules.
- Updated `_system/README.md` with Resources wiki section.

### 2026-05-08 — Oscar therapist search project

- Created `Areas/Family/Projects/oscar-therapist-search/_index.md` (project MOC).
- Created `log.md` and `candidates.md` stubs within the project.
- Updated `Areas/Family/_index.md` to reference the new project.

### 2026-05-08 — Robert L. Green school project + first Weekly Notes

- Created `Areas/Family/Projects/robert-l-green/_index.md` (school project for Nigel & Avery).
- Filed first Weekly Notes email: `2026-05-08-1200-robert-l-green-weekly-notes.md`.
- Updated `_system/school-email-senders.md`: replaced Nigel/Avery placeholder entries with real BrightArrow query for Robert L. Green.
- Updated `_system/sync-state.json` with high-water mark for `gmail_nigel-avery_robert-l-green-weekly-notes`.
- Updated `Areas/Family/_index.md`, `Nigel.md`, `Avery.md` with recent activity.
- Schema note: `child` field extended to `children: [nigel, avery]` for multi-child school notes.

### 2026-04-20 — Agent entrypoint

- Added `AGENTS.md` at vault root pointing agents to `_system/README.md`.
- Symlinked `CLAUDE.md` → `AGENTS.md` so Claude Code and opencode both
  auto-load the entrypoint. Fixes silent failure where agents started in
  the vault without reading the operating manual.

### 2026-05-10 — Summer Vacation 2026 project

- Created `Areas/Family/_index.md` (Family area MOC — previously missing despite area being referenced).
- Created `Areas/Family/Projects/summer-vacation-2026/_index.md` (project MOC).
- Created `Areas/Family/Projects/summer-vacation-2026/destinations.md` (destination comparison note; decision TBD).

### 2026-05-08 — Vulnerability practice

- Created `Areas/Growth/Practices/vulnerability.md` (situational cadence)
  from a Brené-Brown-framed YouTube transcript. Sections: definition, why,
  iceberg model for how to practice, examples, reflection questions
  (self-check / event / current-problem), long-term self-check.
- Extended `vulnerability.md` with a "Green flags" section from Heidi
  Priebe's "10 green flags for vulnerability" video. Organized by other /
  self / relationship, with a quick-filter checklist for in-the-moment
  decisions about when and with whom to share.
- Extended `vulnerability.md` with an "Overdoing vulnerability" section
  from Heidi Priebe's "Are you overdoing vulnerability?" video. Five
  signs (slow recovery, argument-spiral, resentment-without-anger,
  feeling out-of-control, "vulnerability isn't working") with
  course-correct moves and a quick checklist. Frame: secure vs. reckless
  vulnerability.
- Extended `vulnerability.md` with a "When vulnerability backfires"
  section from Heidi Priebe's integrity-frame video. Two-ingredient
  model (sharing-not-pitching + receptive receiver), reception spectrum
  including the dissociative-but-well-meaning middle zone, three
  attachment-style responses to bad reception (anxious / avoidant /
  secure), the "I'm okay, you're okay" worldview, and a five-question
  diagnostic for after a share that went sideways.
- Created `Areas/Growth/Practices/validation.md` (situational cadence)
  from the "Ricky and Jimmy on Relationships" validation podcast.
  Sections: definition, sharer-vs-receiver skills, NVC formula, validating
  phrases, invalidating phrases (never-say list), anxious/avoidant
  guessing nuance, naming defensiveness, kitchen-sink rule, weekly
  check-in, internal/external processor protocol, Gottman 15:1 ratio,
  reflection questions, long-term self-check. Wikilinked to
  [[vulnerability]].

### 2026-05-12 — Top-of-mind dashboard

- Created `top-of-mind.md` at vault root — living snapshot of the most
  active threads across all areas. One section per area with wikilinks
  to underlying project/content files.

### 2026-05-10 — Schema refactor

- Extracted all per-type schemas from `_system/workbox-schema.md` into
  individual files under `_system/schemas/`:
  - `inbox-item.md`
  - `workbox-content.md` (content files + MOC types + MOC templates + file-type conventions)
  - `school-email.md`
  - `resources.md` (Resources wiki + person)
  - `growth.md` (all Growth content types + lifecycle rules)
- `workbox-schema.md` now contains only folder layout, naming conventions,
  evolution rules, and a schema map table pointing to the individual files.

### 2026-05-14 — GitHub issues backend

- Added `_system/github-issues-backend.md` documenting GitHub Issues as the
  durable backlog/context ledger and Things as the execution layer.
- Added GitHub issue label vocabulary for item kind, area, waiting/blocked
  status, top-of-mind visibility, and Things next-action linkage.
- Updated `_system/README.md` to include the GitHub issues backend operating
  file.

### 2026-05-14 — GitHub project dashboard strategy

- Updated `_system/github-issues-backend.md` to define the default GitHub
  Projects as three operational dashboards: Life, Family, and Home.
- Clarified that child/person views, including Oscar, should be saved views
  filtered by `person:*` labels rather than separate top-level GitHub Projects
  by default.
- Expanded the base label vocabulary to match Workboxes areas and add
  `person:oscar`, `person:nigel`, `person:avery`, and `person:roxy`.

### 2026-05-14 — Daily log workflow

- Added `_system/daily-log-workflow.md` to define the whole-workspace daily
  logging flow across Workboxes, GitHub, calendar, email, Slack, Things,
  Drafts, and Day One.
- Added `Daily/_template.md` for `Daily/YYYY-MM-DD.md` daily logs.
- Added `.agents/skills/source-command-daily-start/` and
  `.agents/skills/source-command-daily-review/`.
- Added `/daily-start` and `/daily-review` Claude command shims.
- Updated `_system/README.md` and `AGENTS.md` with the new daily commands.

### 2026-05-14 — GitHub Projects configured

- Renamed GitHub Project #5 from `Oscar` to `Family` and set its description
  and readme around kids, parenting, school, trips, and co-parenting views.
- Renamed GitHub Project #4 from an untitled project to `Life` and set its
  description and readme around self, growth, health, relationships, work, and
  personal-direction views.
- Renamed GitHub Project #3 from an untitled project to `Home` and set its
  description and readme around household systems, maintenance, money/admin,
  repairs, and home ideas.
- Created GitHub issues #2, #3, and #4 as manual saved-view setup checklists
  for Life, Family, and Home respectively, and added each to its matching
  project.

### 2026-05-15 — System rebuild: _system/ -> .agents/docs/

- Rewrote `AGENTS.md` as the single source of truth: absorbed `_system/README.md`,
  added vault layout map, updated skills list, removed /morning and /sync.
- Created `.agents/docs/` directory for all reference docs.
- Consolidated `_system/workbox-schema.md` and all `_system/schemas/*.md` (5 files)
  into a single `.agents/docs/schema.md`.
- Moved `TOP_OF_MIND.md`, `triage-rules.md`, `changelog.md` to `.agents/docs/`.
- Trimmed and moved `github-issues-backend.md` -> `.agents/docs/github-issues.md`.
- Trimmed `capture-sources.md` (removed one-time setup instructions) -> `.agents/docs/`.
- Moved `sync-state.json` to `.agents/sync-state.json`.
- Deprecated `morning` and `sync` skills (stale vault paths and Python scripts).
- Updated all active skill files: removed `_system/` paths, `voice.md` refs,
  `README.md` reads; pointed to new `.agents/docs/` paths.
- Cleared all old `_system/` files in-place (filesystem permissions prevent delete;
  files contain MOVED notices pointing to new locations).

### 2026-05-15 — _index.md -> AGENTS.md migration

- Renamed all 21 `_index.md` files to `AGENTS.md` in their respective directories
  (areas, projects, Growth sub-folders, Resources). Content preserved exactly.
- Stubbed old `_index.md` files with MOVED notices.
- Updated all references: root `AGENTS.md`, `.agents/docs/schema.md`,
  `.claude/commands/triage.md`, `.claude/commands/review-daily.md`,
  `.claude/commands/review-weekly.md` (also fixed remaining `_system/` paths
  in triage.md and review commands while in those files).

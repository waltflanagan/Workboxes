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

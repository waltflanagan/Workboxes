# Workboxes — Phase 0 through 2 Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Stand up a working Obsidian-vault-based personal life-management system with capture (push-side) and conversational triage into workboxes.

**Architecture:** Vault-native, Claude-navigable. All state is markdown files under `/Users/msimons/Code/Workboxes/`, organized as an Obsidian vault with `_system/` operating instructions that Claude reads at session start. Triage is implemented as a Claude Code slash command `/triage` that lives in the vault (`.claude/commands/triage.md`).

**Tech Stack:** Git, markdown with YAML frontmatter, Obsidian (as the UI; no plugin code required), Claude Code slash commands, bash for verification. No application code or runtime dependencies.

**Scope:** Phase 0 (scaffolding) + Phase 1 (push capture, documented) + Phase 2 (conversational triage, first working run). Spec: [docs/superpowers/specs/2026-04-18-workboxes-design.md](../specs/2026-04-18-workboxes-design.md).

**Resolved open questions from spec (for this plan):**
- Triage invocation: **slash command** (`.claude/commands/triage.md` inside the vault).
- Command location: **inside the vault** — portable, travels with the vault, versioned in git.
- Commit granularity during triage: **one commit per triage session** (simpler; git history stays readable).
- Day One pull sync: **out of scope** (Phase 3).

---

## File Structure

Every file below is created or modified during this plan. Paths are relative to `/Users/msimons/Code/Workboxes/` unless noted.

**Created (Phase 0):**
- `.gitignore` — exclude `.DS_Store`, nothing else for now.
- `_system/README.md` — Claude's entry point; describes vault and links to other `_system/` files.
- `_system/workbox-schema.md` — condensed conventions: folder layout, frontmatter schemas, MOC template, slug rules.
- `_system/triage-rules.md` — seeded with format explanation; empty rule list.
- `_system/voice.md` — seeded near-empty with format explanation.
- `_system/capture-sources.md` — inventory and setup docs for capture sources.
- `_system/changelog.md` — seeded with a single "system initialized" entry.
- `_system/sync-state.json` — `{}`; the only non-markdown file, tracks per-source sync timestamps.
- `Inbox/.gitkeep`, `Archive/.gitkeep`, `Attachments/.gitkeep`, `Daily/.gitkeep` — placeholders so empty folders persist in git.
- `Areas/{Work,Family,Relationships,Home,Health,Finances,Personal}/_index.md` — seven area MOCs from template.
- `Areas/{Work,Family,Relationships,Home,Health,Finances,Personal}/Projects/.gitkeep` — placeholders.
- `.claude/commands/triage.md` — the `/triage` slash command definition.

**Modified:**
- None (greenfield).

**Responsibility boundaries:**
- `_system/README.md` is an index, not content. It should stay short (< 100 lines) and link to other files.
- `workbox-schema.md` is the authoritative conventions file — any other doc that duplicates schema info is a bug.
- `triage-rules.md`, `voice.md`, `changelog.md` are append-mostly during normal operation.
- `.claude/commands/triage.md` is the one file that contains the operational procedure; it is the only place that references specific file paths under `_system/`.

---

## Task 1: Create folder skeleton and `.gitignore`

**Files:**
- Create: `.gitignore`
- Create: `Inbox/.gitkeep`, `Archive/.gitkeep`, `Attachments/.gitkeep`, `Daily/.gitkeep`
- Create: `Areas/{Work,Family,Relationships,Home,Health,Finances,Personal}/Projects/.gitkeep` (seven of them)
- Create: `_system/` (empty directory — filled in later tasks)

- [ ] **Step 1: Create all directories**

Run:
```bash
cd /Users/msimons/Code/Workboxes && \
mkdir -p _system Inbox Archive Attachments Daily \
  Areas/Work/Projects Areas/Family/Projects Areas/Relationships/Projects \
  Areas/Home/Projects Areas/Health/Projects Areas/Finances/Projects \
  Areas/Personal/Projects
```

Expected: no output, exit 0.

- [ ] **Step 2: Create `.gitignore`**

File: `.gitignore`
```
.DS_Store
.claude/settings.local.json
```

- [ ] **Step 3: Create `.gitkeep` files for empty folders**

Run:
```bash
cd /Users/msimons/Code/Workboxes && \
touch Inbox/.gitkeep Archive/.gitkeep Attachments/.gitkeep Daily/.gitkeep \
  Areas/Work/Projects/.gitkeep Areas/Family/Projects/.gitkeep \
  Areas/Relationships/Projects/.gitkeep Areas/Home/Projects/.gitkeep \
  Areas/Health/Projects/.gitkeep Areas/Finances/Projects/.gitkeep \
  Areas/Personal/Projects/.gitkeep
```

Expected: no output, exit 0.

- [ ] **Step 4: Verify structure**

Run: `cd /Users/msimons/Code/Workboxes && find . -type d -not -path './.git*' -not -path './docs*' | sort`

Expected output includes:
```
.
./Archive
./Areas
./Areas/Family
./Areas/Family/Projects
./Areas/Finances
./Areas/Finances/Projects
./Areas/Health
./Areas/Health/Projects
./Areas/Home
./Areas/Home/Projects
./Areas/Personal
./Areas/Personal/Projects
./Areas/Relationships
./Areas/Relationships/Projects
./Areas/Work
./Areas/Work/Projects
./Attachments
./Daily
./Inbox
./_system
```

- [ ] **Step 5: Commit**

```bash
cd /Users/msimons/Code/Workboxes && \
git add .gitignore Inbox Archive Attachments Daily Areas && \
git commit -m "Create vault folder skeleton

Seven starter Areas (Work, Family, Relationships, Home, Health,
Finances, Personal), each with a Projects/ subfolder. Top-level
Inbox, Archive, Attachments, Daily.
"
```

---

## Task 2: Write `_system/workbox-schema.md`

This is the authoritative conventions file. Claude reads it when it needs to create new workbox content or understand the taxonomy.

**Files:**
- Create: `_system/workbox-schema.md`

- [ ] **Step 1: Write the file**

File: `_system/workbox-schema.md`

````markdown
# Workbox Schema

Conventions for the Workboxes vault. Claude reads this file when creating new
workboxes, scaffolding index files, or validating frontmatter.

## Folder layout

```
Workboxes/
├── _system/                            (operating manual)
├── Inbox/                              (raw captures, flat)
├── Areas/<area>/                       (active area)
│   ├── _index.md                       (area MOC)
│   ├── Projects/<project-slug>/        (active project within area)
│   │   ├── _index.md                   (project MOC)
│   │   └── <content>.md                (notes, logs, decisions)
│   └── <area-level-note>.md            (optional; for notes not scoped to a project)
├── Archive/                            (mirrors Areas/; archived workboxes)
├── Attachments/                        (binaries; referenced via relative links)
└── Daily/                              (Obsidian daily notes)
```

## Naming

- Folders and files: `kebab-case-slugs`. Lowercase, hyphens only, no punctuation.
- Inbox item filename: `YYYY-MM-DD-HHMM-<slug>.md`. `<slug>` is derived from title or first words.
- Index files: always `_index.md` (underscore prefix sorts to top in Obsidian).

## Frontmatter — inbox item

```yaml
---
type: inbox-item
captured: 2026-04-18T14:30:00-04:00    # ISO 8601 with offset
source: dayone | drafts | web-clipper | raw | gmail | things | voice-memo | messages
source_id: <original id if applicable>
source_url: <url if applicable>
tags: []
---
```

A raw-text drop with no frontmatter is tolerated. Triage treats it as:
```yaml
type: inbox-item
captured: <file mtime, ISO 8601>
source: raw
tags: []
```

## Frontmatter — workbox content

```yaml
---
type: note | log-entry | decision | reference
created: 2026-04-18T14:30:00-04:00
source: <carried from inbox if applicable>
source_id: <carried from inbox if applicable>
tags: []
links: [[related-note]]
---
```

## Frontmatter — index file (MOC)

```yaml
---
type: area-index | project-index
status: active | dormant | archived
owner_notes: <free text; Claude updates with current focus, waiting-on items>
---
```

## MOC template — area

```markdown
---
type: area-index
status: active
owner_notes:
---

# <Area Name>

## Claude: Read this first

Projects for this area live in `Projects/`. Route inbox items here when they
relate to <area description — filled in per-area>.

## Open loops

(none)

## Projects

(none yet)

## Recent activity

(none yet)
```

## MOC template — project

```markdown
---
type: project-index
status: active
owner_notes:
---

# <Project Name>

## Claude: Read this first

(Narrative of what this project is and how its contents are organized. Grows
as the project matures.)

## Open loops

(none)

## Recent activity

(none yet)
```

## File-type conventions

- `log.md` (optional, in a project folder) — reverse-chronological log of
  threads, updates, quick notes that don't warrant their own file.
- `decisions.md` (optional, in a project folder) — one entry per decision,
  with date, context, decision, and rationale.
- `memory.md` (reserved for Phase 7) — distilled durable knowledge for a
  project. Not created in Day 1.

## Evolution rules

All structural changes follow **propose → approve → execute → log**:

1. Claude proposes the change (new area, new project, new rule, schema tweak).
2. User approves.
3. Claude executes: create/move files, update parent indices.
4. Claude appends an entry to `_system/changelog.md`.

Never make silent structural changes.
````

- [ ] **Step 2: Verify file exists and has valid structure**

Run: `cd /Users/msimons/Code/Workboxes && wc -l _system/workbox-schema.md`

Expected: line count > 80, exit 0.

- [ ] **Step 3: Commit**

```bash
cd /Users/msimons/Code/Workboxes && \
git add _system/workbox-schema.md && \
git commit -m "Add workbox schema — conventions for folders, frontmatter, MOCs"
```

---

## Task 3: Write the remaining `_system/` files

Six small files, grouped into one task because each is templated and none is independently useful.

**Files:**
- Create: `_system/README.md`
- Create: `_system/triage-rules.md`
- Create: `_system/voice.md`
- Create: `_system/capture-sources.md` (placeholder; filled in Task 5)
- Create: `_system/changelog.md`
- Create: `_system/sync-state.json`

- [ ] **Step 1: Write `_system/README.md`**

File: `_system/README.md`

```markdown
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
- `Areas/Finances/` — money, taxes, accounts, major decisions.
- `Areas/Personal/` — hobbies, reading, learning, personal projects.

Archived areas and projects live under `Archive/` mirroring the `Areas/`
structure.

## How to run triage

Invoke `/triage` (Claude Code slash command, defined in
`.claude/commands/triage.md`). It implements the procedure from the spec:
load context, propose dispositions, walk items with the user, execute,
extract rules, log.

## How the system evolves

All structural changes follow **propose → approve → execute → log**. Never
silent. See `workbox-schema.md` for the full evolution rules.
```

- [ ] **Step 2: Write `_system/triage-rules.md`**

File: `_system/triage-rules.md`

````markdown
# Triage Rules

Learned routing rules. Claude consults this file during triage to find
high-confidence matches and proposes new rules when user overrides reveal a
pattern.

## Rule format

Each rule is an H2 block:

```markdown
## Rule: <short description>
Added: YYYY-MM-DD
Condition: <human-readable condition, e.g., source = dayone AND tags contains "gratitude">
Action: route to <path> | create new project "<name>" under <area> | discard
Confidence: high | medium | low
```

Rules are evaluated top-to-bottom; first match wins. Rules can reference
frontmatter fields (`source`, `source_id`, `source_url`, `tags`) or content
substrings. Keep conditions narrow enough to avoid false positives — when in
doubt, downgrade confidence from `high` to `medium`.

## Rules

(none yet)
````

- [ ] **Step 3: Write `_system/voice.md`**

File: `_system/voice.md`

```markdown
# Voice

User voice, preferences, and communication style. Claude consults this file
before producing user-facing text.

## Format

Free-form bullets or short paragraphs. Add a preference here after the user
gives explicit feedback — "be terser," "use first names," "don't summarize at
the end." Each entry should be short and actionable.

## Preferences

(none yet — grow from feedback)
```

- [ ] **Step 4: Write `_system/capture-sources.md` (placeholder)**

File: `_system/capture-sources.md`

```markdown
# Capture Sources

Inventory of capture sources, their provenance fields, and setup quirks.
Filled in per-source as pipelines are wired up.

## Active sources

(none yet — configured in Task 5)

## Deferred sources

- Gmail
- Things
- Voice Memos (requires transcription)
- Messages / iMessage
```

- [ ] **Step 5: Write `_system/changelog.md`**

File: `_system/changelog.md`

```markdown
# Vault Changelog

Append-only log of structural changes: workboxes created or archived, rules
added or removed, schema tweaks. Newest entries at the bottom.

## Entries

### 2026-04-18 — System initialized

- Created vault folder skeleton (Task 1).
- Seeded seven starter Areas: Work, Family, Relationships, Home, Health,
  Finances, Personal.
- Seeded `_system/` operating files.
```

- [ ] **Step 6: Write `_system/sync-state.json`**

File: `_system/sync-state.json`

```json
{}
```

- [ ] **Step 7: Verify all files exist**

Run: `cd /Users/msimons/Code/Workboxes && ls _system/`

Expected output:
```
README.md
capture-sources.md
changelog.md
sync-state.json
triage-rules.md
voice.md
workbox-schema.md
```

- [ ] **Step 8: Verify `sync-state.json` is valid JSON**

Run: `cd /Users/msimons/Code/Workboxes && python3 -c "import json; json.load(open('_system/sync-state.json'))"`

Expected: no output, exit 0.

- [ ] **Step 9: Commit**

```bash
cd /Users/msimons/Code/Workboxes && \
git add _system/ && \
git commit -m "Seed _system/ operating files

README (entry point), triage-rules (empty), voice (empty),
capture-sources (placeholder), changelog (initial entry),
sync-state.json (empty).
"
```

---

## Task 4: Write Area `_index.md` files

Seven files, same template, one H2 per area with area-specific routing guidance.

**Files:**
- Create: `Areas/Work/_index.md`
- Create: `Areas/Family/_index.md`
- Create: `Areas/Relationships/_index.md`
- Create: `Areas/Home/_index.md`
- Create: `Areas/Health/_index.md`
- Create: `Areas/Finances/_index.md`
- Create: `Areas/Personal/_index.md`

- [ ] **Step 1: Write `Areas/Work/_index.md`**

```markdown
---
type: area-index
status: active
owner_notes:
---

# Work

## Claude: Read this first

Projects for this area live in `Projects/`. Route inbox items here when they
relate to Etsy, team work, career development, or work-adjacent professional
matters. If an item is about a specific work project, route to that project
folder (create if needed with user approval). If it's area-level (e.g., a
career reflection not tied to a project), land it here as an area note.

## Open loops

(none)

## Projects

(none yet)

## Recent activity

(none yet)
```

- [ ] **Step 2: Write `Areas/Family/_index.md`**

```markdown
---
type: area-index
status: active
owner_notes:
---

# Family

## Claude: Read this first

Projects for this area live in `Projects/`. Route inbox items here when they
relate to partner, kids, parenting, or extended family. Ongoing parenting
topics (school, activities, medical) usually warrant their own project.

## Open loops

(none)

## Projects

(none yet)

## Recent activity

(none yet)
```

- [ ] **Step 3: Write `Areas/Relationships/_index.md`**

```markdown
---
type: area-index
status: active
owner_notes:
---

# Relationships

## Claude: Read this first

Projects for this area live in `Projects/`. Route inbox items here when they
relate to friends, community, social commitments, or non-family relationships.
Items about specific people may warrant per-person project folders as the
system grows.

## Open loops

(none)

## Projects

(none yet)

## Recent activity

(none yet)
```

- [ ] **Step 4: Write `Areas/Home/_index.md`**

```markdown
---
type: area-index
status: active
owner_notes:
---

# Home

## Claude: Read this first

Projects for this area live in `Projects/`. Route inbox items here when they
relate to the house, maintenance, moves, or household logistics (not finances
— those live in `Finances/`). Ongoing projects like renovations or organizing
should be their own project folders.

## Open loops

(none)

## Projects

(none yet)

## Recent activity

(none yet)
```

- [ ] **Step 5: Write `Areas/Health/_index.md`**

```markdown
---
type: area-index
status: active
owner_notes:
---

# Health

## Claude: Read this first

Projects for this area live in `Projects/`. Route inbox items here when they
relate to physical health, mental health, appointments, fitness, or medical
matters. Medical conditions or ongoing therapies may warrant their own
project folders. Note: Phase 6 may restructure how therapy content is handled.

## Open loops

(none)

## Projects

(none yet)

## Recent activity

(none yet)
```

- [ ] **Step 6: Write `Areas/Finances/_index.md`**

```markdown
---
type: area-index
status: active
owner_notes:
---

# Finances

## Claude: Read this first

Projects for this area live in `Projects/`. Route inbox items here when they
relate to money, taxes, accounts, investments, or major financial decisions.
Tax years and big decisions (e.g., house purchase) warrant their own projects.

## Open loops

(none)

## Projects

(none yet)

## Recent activity

(none yet)
```

- [ ] **Step 7: Write `Areas/Personal/_index.md`**

```markdown
---
type: area-index
status: active
owner_notes:
---

# Personal

## Claude: Read this first

Projects for this area live in `Projects/`. Route inbox items here when they
relate to hobbies, reading, learning, or personal projects that aren't work,
family, home, health, or finance. If an item is about personal growth, life
goals, values, or therapy work, leave it here as an area note for now —
Phase 6 will design a dedicated structure for that material.

## Open loops

(none)

## Projects

(none yet)

## Recent activity

(none yet)
```

- [ ] **Step 8: Verify all seven files exist**

Run: `cd /Users/msimons/Code/Workboxes && ls Areas/*/_index.md`

Expected output:
```
Areas/Family/_index.md
Areas/Finances/_index.md
Areas/Health/_index.md
Areas/Home/_index.md
Areas/Personal/_index.md
Areas/Relationships/_index.md
Areas/Work/_index.md
```

- [ ] **Step 9: Verify each file has valid YAML frontmatter**

Run:
```bash
cd /Users/msimons/Code/Workboxes && \
for f in Areas/*/_index.md; do \
  python3 -c "
import sys
text = open('$f').read()
assert text.startswith('---\n'), '$f: missing opening ---'
end = text.find('\n---\n', 4)
assert end > 0, '$f: missing closing ---'
import yaml
meta = yaml.safe_load(text[4:end])
assert meta.get('type') == 'area-index', '$f: type is not area-index'
assert meta.get('status') == 'active', '$f: status is not active'
print('$f: OK')
" || exit 1; \
done
```

Expected: seven "OK" lines, exit 0.

(If `pyyaml` is not installed, run `pip3 install pyyaml` first.)

- [ ] **Step 10: Commit**

```bash
cd /Users/msimons/Code/Workboxes && \
git add Areas/*/_index.md && \
git commit -m "Seed Area _index.md files for seven starter areas

Each MOC has a Claude-facing routing hint, empty Open loops,
empty Projects list, empty Recent activity. Hints are specific
per area to reduce misrouting.
"
```

---

## Task 5: Document capture pipelines in `_system/capture-sources.md`

Phase 1 is documentation — the system is ready to receive captures once the user has configured Obsidian Web Clipper and a Drafts action per this file. Raw-text captures work without configuration.

**Files:**
- Modify: `_system/capture-sources.md`

- [ ] **Step 1: Overwrite `_system/capture-sources.md` with full content**

File: `_system/capture-sources.md`

````markdown
# Capture Sources

Inventory of capture sources, their provenance fields, and setup instructions.
All sources converge on `Inbox/` with the uniform frontmatter shape defined
in `workbox-schema.md`.

## Active sources (Day 1)

### Raw text

Drop any `.md` file into `Inbox/`. Works without setup.

- If frontmatter is missing, triage treats the file as `source: raw` and
  infers `captured` from the file's mtime.
- Filename convention: `YYYY-MM-DD-HHMM-<slug>.md`. If a dropped file does
  not follow this convention, triage will still handle it; a rename may
  happen during routing.

### Obsidian Web Clipper

The Obsidian Web Clipper extension saves web pages and selections into a
chosen folder using a user-defined template.

**Setup (manual, one-time):**

1. Install the Obsidian Web Clipper extension in your browser of choice.
2. In the extension's settings, set the **Vault** to the Workboxes vault.
3. Set the **Folder** to `Inbox`.
4. Set the **Template** to use the following **note content** and **filename format**:

**Filename format:**
```
{{date:YYYY-MM-DD}}-{{date:HHmm}}-{{title|slug}}
```

**Properties (frontmatter) — add these fields:**

| Name | Value |
|---|---|
| `type` | `inbox-item` |
| `captured` | `{{date:YYYY-MM-DDTHH:mm:ssZZ}}` |
| `source` | `web-clipper` |
| `source_url` | `{{url}}` |
| `tags` | (leave empty) |

**Note content template:**
```
{{content}}
```

5. Save the template and test-clip a page. Confirm a file appears in `Inbox/`
   with correctly-shaped frontmatter.

### Drafts → Obsidian action

A Drafts action that writes the current draft to the Workboxes `Inbox/` with
provenance frontmatter. Works on iOS and macOS.

**Setup (manual, one-time):**

1. Open Drafts. In the Actions directory, search for an "Append to Obsidian"
   or "Save to Obsidian" action, or create a new action of type **Script →
   File Append** / **Obsidian URL**.
2. Configure the action to write to the Workboxes vault, folder `Inbox`, with
   the following template as the file content:

```
---
type: inbox-item
captured: [[date|%Y-%m-%dT%H:%M:%S%z]]
source: drafts
source_id: [[draft_uuid]]
tags: [[tags]]
---

[[draft]]
```

(The exact template syntax depends on the Drafts action implementation
chosen. The key requirement is that the resulting file has frontmatter
matching `workbox-schema.md`'s inbox-item spec.)

3. Filename template: `[[date|%Y-%m-%d]]-[[date|%H%M]]-[[title|slug]].md`.
4. Test from the Drafts share sheet (iOS) and the Drafts app directly (macOS).
   Confirm files land in `Inbox/`.

**Known quirks:** Drafts sometimes writes without a trailing newline — triage
tolerates this. Voice-dictated drafts may lack a useful title; the filename
slug will be derived from the first words.

## Deferred sources

Documented here with intended provenance fields so future pipelines stay
consistent.

- **Day One** (Phase 3) — `source: dayone`, `source_id: <entry uuid>`. Pull
  sync via a `sync-dayone` script; photos inlined to `Attachments/`.
- **Gmail** (Phase 5) — `source: gmail`, `source_id: <message id>`,
  `source_url: <gmail permalink>`.
- **Things** (Phase 5) — `source: things`, `source_id: <task uuid>`. Task
  state sync-back (if any) is a Phase 5 design decision.
- **Voice Memos** (Phase 5) — `source: voice-memo`, `source_id: <recording
  filename>`. Requires a transcription step upstream of inbox-write.
- **Messages / iMessage** (Phase 5) — `source: messages`,
  `source_id: <message guid>`.

## Adding a new source

When a new source is wired up:

1. Update this file with setup instructions and provenance fields.
2. If it's a pull source, add a top-level key to `_system/sync-state.json`.
3. Append an entry to `_system/changelog.md`.
4. No changes to triage logic are required — all sources land in `Inbox/`
   with the standard frontmatter shape.
````

- [ ] **Step 2: Verify file is well-formed markdown**

Run: `cd /Users/msimons/Code/Workboxes && grep -c "^## " _system/capture-sources.md`

Expected: 3 (Active sources, Deferred sources, Adding a new source).

- [ ] **Step 3: Commit**

```bash
cd /Users/msimons/Code/Workboxes && \
git add _system/capture-sources.md && \
git commit -m "Document Phase 1 capture pipelines

Raw text (works without setup), Obsidian Web Clipper
(manual extension config), Drafts → Obsidian action (manual
Drafts action config). Deferred sources listed with intended
provenance fields so future pipelines stay consistent.
"
```

---

## Task 6: Create the `/triage` slash command

The operational procedure from the spec, written as a Claude Code slash command. Lives inside the vault at `.claude/commands/triage.md` so it's portable and git-versioned.

**Files:**
- Create: `.claude/commands/triage.md`

- [ ] **Step 1: Create the `.claude/commands/` directory**

Run: `cd /Users/msimons/Code/Workboxes && mkdir -p .claude/commands`

Expected: no output, exit 0.

- [ ] **Step 2: Write `.claude/commands/triage.md`**

File: `.claude/commands/triage.md`

````markdown
---
description: Triage the Workboxes inbox — propose dispositions for each captured item and route to workboxes on approval.
---

You are triaging the Workboxes vault inbox. Follow this procedure exactly.

## 1. Load context

Read these files in order:

1. `_system/README.md` — entry point.
2. `_system/workbox-schema.md` — conventions.
3. `_system/triage-rules.md` — learned rules.
4. `_system/voice.md` — user voice/preferences.
5. Each file matching `Areas/*/_index.md` — for each, read only the YAML
   frontmatter and the first two sections ("# <Name>" heading and "## Claude:
   Read this first"). Do not read `## Open loops`, `## Projects`, or
   `## Recent activity` at this stage — they are not needed for routing.

## 2. List inbox items

Run: `ls -1 Inbox/*.md 2>/dev/null | sort` to list inbox files in chronological
order (filenames start with date+time). If zero matches, report "Inbox is
empty." and stop.

For each file, read its full contents (frontmatter + body). If a file has no
frontmatter, treat it as:
```yaml
type: inbox-item
captured: <file mtime as ISO 8601>
source: raw
tags: []
```

## 3. Propose a disposition for each item

For each item, choose one:

- **Route** — move to a specific `Areas/<area>/` (area-level note) or
  `Areas/<area>/Projects/<project>/` (project file). Give a one-line rationale.
- **New project** — propose creating `Areas/<area>/Projects/<slug>/` and
  routing the item there. Explain why this doesn't fit an existing project.
- **New area** — propose creating `Areas/<slug>/`. Rare; only when the item
  genuinely belongs to a domain outside the seven starter areas.
- **Discard** — duplicate, test, or genuinely throwaway. Requires explicit
  approval.
- **Defer** — insufficient signal; leave in `Inbox/` and add a
  `triaged_deferred_at: <ISO 8601 timestamp>` field to the item's frontmatter.

### Applying triage rules

Before proposing, scan `_system/triage-rules.md`. If an item matches a rule
top-to-bottom, mark the proposal **high-confidence** and prefix the rationale
with "(rule: <rule name>)". Otherwise mark **medium** or **low** confidence
based on how specifically the item matches a workbox.

## 4. Walk the user through proposals

Present items one at a time (or batch obvious high-confidence ones together —
ask the user's preference at the start of the session). For each:

- Show: filename, source, one-line content summary, proposed disposition,
  rationale, confidence.
- User responses: **approve**, **change to <X>**, **defer**, **skip**.
- Keep the conversation terse (consult `_system/voice.md`).

## 5. Execute approved dispositions

For each approved item, perform these actions:

### Route

1. Determine the target path.
2. If routing to a project and the project folder does not exist, fail —
   routing to a nonexistent project should have been a "New project"
   proposal in step 3.
3. Rewrite the file's frontmatter: change `type` from `inbox-item` to
   `note` (or `log-entry` / `decision` / `reference` if the user specifies).
   Preserve `source`, `source_id`, `source_url`. Rename `captured` to
   `created`. Keep `tags`.
4. Move the file: `git mv Inbox/<filename>.md <target path>/<filename>.md`.
5. Append to the target workbox's `_index.md` under `## Recent activity`:
   `- YYYY-MM-DD — <one-line summary>`.

### New project

1. Ask user for a kebab-case project slug if not already provided.
2. Create directory: `Areas/<area>/Projects/<slug>/`.
3. Create `Areas/<area>/Projects/<slug>/_index.md` from the project MOC
   template in `_system/workbox-schema.md`. Fill in `# <Project Name>` and
   a seeded "Claude: Read this first" paragraph describing the project in
   1-2 sentences based on what you know from the inbox items being routed
   here.
4. Append to `Areas/<area>/_index.md` under `## Projects`:
   `- [[<slug>]] — <one-line description>`.
5. Route the item(s) into the new project (see Route above).

### New area

1. Confirm with the user that this genuinely doesn't fit an existing area.
2. Ask for a kebab-case area slug.
3. Create directory: `Areas/<slug>/Projects/`.
4. Create `Areas/<slug>/_index.md` from the area MOC template.
5. Append to `_system/README.md` under `## Active areas` in the correct
   alphabetical position.
6. Route the item (see Route above).

### Discard

Run: `git rm Inbox/<filename>.md`.

### Defer

Edit the file's frontmatter to add `triaged_deferred_at: <now ISO 8601>`.
Leave the file in `Inbox/`.

## 6. Extract rules

For each case where the user overrode your proposal in a way that looks
pattern-y (e.g., "always route Day One gratitude entries to
Personal/Gratitude"), propose a new rule for `_system/triage-rules.md` in
the format specified in that file. Present the rule to the user; on approval,
append it to `triage-rules.md`.

Do not propose rules for one-off decisions. Ask yourself: "would this same
decision apply to three or more similar items?" If unsure, don't propose.

## 7. Log the session

Append an entry to `_system/changelog.md` under `## Entries`:

```markdown
### <YYYY-MM-DD> — Triage session

- Triaged <N> items: <X> routed, <Y> new projects created, <Z> discarded,
  <W> deferred.
- New rules: <list rule names, or "none">.
- New workboxes: <list paths, or "none">.
```

## 8. Commit

One commit per triage session. Run:

```bash
cd /Users/msimons/Code/Workboxes && \
git add -A && \
git commit -m "Triage session <YYYY-MM-DD>: <N> items triaged

<brief per-disposition summary>
"
```

If there are no changes to commit (e.g., all items were deferred with no
frontmatter changes), skip the commit step.

## Tone

Consult `_system/voice.md` for user voice. Default to terse proposals. No
trailing summaries unless the user asks. Respect the user's time — this is
a tool they use to reduce cognitive load, not generate more of it.
````

- [ ] **Step 3: Verify the file exists and has the frontmatter description**

Run: `cd /Users/msimons/Code/Workboxes && head -3 .claude/commands/triage.md`

Expected output:
```
---
description: Triage the Workboxes inbox — propose dispositions for each captured item and route to workboxes on approval.
---
```

- [ ] **Step 4: Commit**

```bash
cd /Users/msimons/Code/Workboxes && \
git add .claude/commands/triage.md && \
git commit -m "Add /triage slash command

Implements the triage procedure from the design spec:
load context, propose dispositions, walk user, execute,
extract rules, log, commit. One commit per triage session.
"
```

---

## Task 7: End-to-end triage verification

Seed the inbox with test items, run `/triage`, verify the system works and iterate on rough edges.

**Files:**
- Create (temporary, verification only): `Inbox/2026-04-18-0900-test-work-meeting.md`
- Create (temporary, verification only): `Inbox/2026-04-18-0930-test-home-repair.md`
- Create (temporary, verification only): `Inbox/2026-04-18-1000-test-raw-drop.md`

- [ ] **Step 1: Seed an inbox item from a "Web Clipper" source**

File: `Inbox/2026-04-18-0900-test-work-meeting.md`

```markdown
---
type: inbox-item
captured: 2026-04-18T09:00:00-04:00
source: web-clipper
source_url: https://example.com/meeting-notes
tags: []
---

Notes from the Q2 planning kickoff. Team discussed OKRs, ownership of the
search relevance workstream, and the tradeoffs around the new infra
migration. Action item: follow up with Priya on the latency targets.
```

- [ ] **Step 2: Seed an inbox item from a "Drafts" source**

File: `Inbox/2026-04-18-0930-test-home-repair.md`

```markdown
---
type: inbox-item
captured: 2026-04-18T09:30:00-04:00
source: drafts
source_id: 0000-test-uuid
tags: []
---

Kitchen faucet is dripping again. Need to replace the cartridge — same one
we did two years ago. Part number is on the fridge. Call plumber if it
doesn't fix it.
```

- [ ] **Step 3: Seed a raw-text inbox item (no frontmatter)**

File: `Inbox/2026-04-18-1000-test-raw-drop.md`

```markdown
Random thought: I want to read more fiction this year. Let me make a list of
books I've been meaning to get to — The Overstory, Piranesi, Station Eleven.
```

- [ ] **Step 4: Verify three items in Inbox**

Run: `cd /Users/msimons/Code/Workboxes && ls -1 Inbox/*.md`

Expected output (three lines):
```
Inbox/2026-04-18-0900-test-work-meeting.md
Inbox/2026-04-18-0930-test-home-repair.md
Inbox/2026-04-18-1000-test-raw-drop.md
```

- [ ] **Step 5: Run `/triage`**

Invoke the slash command in Claude Code: `/triage`.

Expected behavior:
- Claude reads `_system/` files and `Areas/*/_index.md`.
- Claude lists three items and proposes a disposition for each.
- User is prompted to approve/change/defer/skip each one.
- On approval, files are moved to the right workboxes (or projects are created).
- A changelog entry is appended.
- One commit is made.

Expected example dispositions (actual proposals may vary):
- `test-work-meeting.md` → `Areas/Work/` area-level note OR a new `Areas/Work/Projects/q2-planning/` project.
- `test-home-repair.md` → `Areas/Home/` area-level note OR a new `Areas/Home/Projects/kitchen-faucet/` project.
- `test-raw-drop.md` → `Areas/Personal/` area-level note OR a new `Areas/Personal/Projects/reading/` project.

- [ ] **Step 6: Verify inbox is empty after triage**

Run: `cd /Users/msimons/Code/Workboxes && ls -1 Inbox/*.md 2>/dev/null | wc -l`

Expected: `0` (unless you intentionally deferred an item).

- [ ] **Step 7: Verify routed files have correct frontmatter**

For each routed file, confirm:
- `type` is no longer `inbox-item` (should be `note`, `log-entry`, `decision`, or `reference`).
- `captured` was renamed to `created`.
- `source`, `source_id`, `source_url` are preserved.

Run (adjust paths to wherever files were routed):
```bash
cd /Users/msimons/Code/Workboxes && \
for f in $(git log -1 --name-only --pretty=format: | grep -v '^$' | grep '\.md$'); do \
  echo "--- $f ---"; \
  head -10 "$f"; \
done
```

- [ ] **Step 8: Verify `_system/changelog.md` has a triage session entry**

Run: `cd /Users/msimons/Code/Workboxes && tail -20 _system/changelog.md`

Expected: an H3 entry with today's date and a "Triage session" title, with
per-disposition counts.

- [ ] **Step 9: Verify parent `_index.md` files were updated**

For each area or project that received items, confirm its `_index.md` has a
new line under `## Recent activity`.

Run: `cd /Users/msimons/Code/Workboxes && grep -l "2026-04-18" Areas/*/_index.md Areas/*/Projects/*/_index.md 2>/dev/null`

Expected: lists the index files that were updated.

- [ ] **Step 10: Verify exactly one commit was produced by the triage session**

Run: `cd /Users/msimons/Code/Workboxes && git log --oneline -5`

Expected: the top commit's message starts with `Triage session 2026-04-18:`.
Subsequent commits are the Task 1-6 commits.

- [ ] **Step 11: Capture and address friction**

If anything during the triage run felt off — Claude misunderstood the
vault, got confused about routing, produced bad proposals, or the voice was
wrong — edit the relevant `_system/` file (README.md, workbox-schema.md,
voice.md, or the `/triage` command itself) to fix the friction. Re-run
`/triage` on a fresh seeded inbox if the fix is material.

This step is the key Day-1 learning loop. Do not skip it.

- [ ] **Step 12: Commit any friction-driven fixes**

If Step 11 produced changes:

```bash
cd /Users/msimons/Code/Workboxes && \
git add -A && \
git commit -m "Refine triage after first end-to-end run

<one-line summary of what was changed and why>
"
```

---

## Completion criteria

- [ ] All seven tasks complete; every checkbox ticked.
- [ ] `git log` shows at least seven commits (one per task + any Task 7 follow-ups).
- [ ] The vault directory structure matches the File Structure section above.
- [ ] A test triage run routed three seeded inbox items end-to-end without manual file editing.
- [ ] `_system/changelog.md` contains the "System initialized" entry and the "Triage session" entry.

At completion, Day 1 is done: the vault exists, the `_system/` operating manual is in place, push captures are documented, and `/triage` works. Subsequent phases (Day One sync, hybrid triage, additional sources, Phase 6 growth extension, Phase 7 long-term memory) each need their own plan.

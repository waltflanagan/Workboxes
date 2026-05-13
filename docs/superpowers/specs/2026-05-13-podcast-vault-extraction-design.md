# Podcast Vault Extraction Design

**Date:** 2026-05-13
**Status:** Approved

## Goal

Extract all podcast-related material from `relationship-agent` into a new dedicated repo at `/Volumes/💩/ai/podcast-vault`. The new repo is a standalone podcast archive with its own processing pipeline. The listening log in Workboxes stays in place and can reference content in podcast-vault by path.

## New Repo Structure

```
/Volumes/💩/ai/podcast-vault/
  podcasts/          ← episode summaries + transcripts (~27 shows, ~143MB)
  listening-notes/   ← personal episode reflections (private)
  assets/            ← raw audio + .txt transcripts (~19MB)
  scripts/           ← processing pipeline (fetch, summarize, intake, etc.)
  CLAUDE.md
```

## Migration

| Source | Destination |
|--------|-------------|
| `relationship-agent/vault/learning/podcasts/` | `podcast-vault/podcasts/` |
| `relationship-agent/vault/learning/listening-notes/` | `podcast-vault/listening-notes/` |
| `relationship-agent/assets/` | `podcast-vault/assets/` |
| `relationship-agent/scripts/learning/` | `podcast-vault/scripts/` |

After migration, these are deleted from relationship-agent:
- `relationship-agent/vault/learning/` (entire directory)
- `relationship-agent/assets/` (entire directory)
- `relationship-agent/scripts/learning/` (directory only; `scripts/` itself stays)

`Workboxes/Areas/Personal/podcast-log.md` stays in Workboxes — it is the active listening log, not archive content.

`scan_journals.py` moves with the rest of `scripts/learning/` to `podcast-vault/scripts/`. Its internal path updates (INBOX_DIR, vault scan targets) are out of scope and will be handled separately.

## Reference Updates

1. **`relationship-agent/CLAUDE.md`** — remove `learning/` from domain structure, remove `vault/learning/` commands, remove `assets/` from layout
2. **`relationship-agent/scripts/learning/CLAUDE.md`** — deleted with the directory; new `podcast-vault/scripts/CLAUDE.md` written with updated paths
3. **`relationship-agent/.claude/skills/adhd-coach/SKILL.md`** + `.agents/` copy — update `vault/podcasts/` reference to `/Volumes/💩/ai/podcast-vault/podcasts/`
4. **`podcast-vault/scripts/feeds.yaml`** — update `ignore_folders` from `vault/learning/podcasts` → `podcasts` and `vault/learning/listening-notes` → `listening-notes`

## Script Path Updates

The scripts in `scripts/learning/` compute paths relative to their location. Currently they resolve `../../vault/learning/` to reach the podcast archive. After the move to `podcast-vault/scripts/`, paths resolve to `../` (one level up, no `learning/` subdirectory). All affected path computations in the scripts must be updated.

## New podcast-vault CLAUDE.md

A lightweight CLAUDE.md covering:
- Repo purpose (podcast archive + processing pipeline)
- Directory structure
- Script commands (same as current `scripts/learning/CLAUDE.md`)
- Key gotchas (inherited from current CLAUDE.md)

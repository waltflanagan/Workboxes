# Podcast Vault Extraction Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Extract all podcast content and tooling from `relationship-agent` into a new standalone repo at `/Volumes/💩/ai/podcast-vault`.

**Architecture:** New repo gets the content flat at top level (`podcasts/`, `listening-notes/`, `assets/`, `scripts/`). Scripts' internal path references are updated to match the new depth. Existing episode markdown files get their `audio:` frontmatter paths patched in bulk. Three CLAUDE.md/SKILL.md files in two other repos get reference updates.

**Tech Stack:** Python 3, git, bash

---

### Task 1: Initialize podcast-vault repo

**Files:**
- Create: `/Volumes/💩/ai/podcast-vault/CLAUDE.md`

- [ ] **Step 1: Create the repo directory and initialize git**

```bash
mkdir /Volumes/💩/ai/podcast-vault
cd /Volumes/💩/ai/podcast-vault
git init
```

Expected: `Initialized empty Git repository in /Volumes/💩/ai/podcast-vault/.git/`

- [ ] **Step 2: Write CLAUDE.md**

Create `/Volumes/💩/ai/podcast-vault/CLAUDE.md` with this exact content:

```markdown
# podcast-vault

Podcast archive and processing pipeline. Episode summaries, transcripts, personal listening notes, and the scripts that generate them.

## Structure

```
podcasts/          ← episode summaries + transcripts, organized by show
listening-notes/   ← personal episode reflections (private — don't summarize without instruction)
assets/            ← raw audio (.mp3/.m4a) and transcript (.txt) files
scripts/           ← processing pipeline
CLAUDE.md
```

Use `podcasts/_index.md` as the primary entry point for topic-based lookup. Don't scan individual episode files unless the index points you there.

Don't modify episode summaries or transcripts — they are generated outputs. Flag corrections rather than editing directly.

## Commands

```bash
python3 scripts/fetch.py                    # download new episodes from RSS
python3 scripts/summarize.py                # summarize pending episodes
python3 scripts/summarize.py --reindex      # rebuild READMEs and topic index
python3 scripts/migrate.py --dry-run        # preview un-migrated files (should be 0)
python3 scripts/intake.py                   # process all inbox files
python3 scripts/intake.py --episode "URL or name" --notes "text"
python3 scripts/intake.py --re-summarize "path/to/notes.md"
```

## Key Frontmatter Fields

- `podcast-archive-type: summary | transcript | listening-notes`
- `status: downloaded | summarized` — episode files
- `status: in-progress | complete` — listening-notes files
- `audio: ../../assets/Podcast Name/slug.mp3` — relative from summary file (2 levels up to repo root)
- `episode-file: "podcasts/Podcast Name/episode-slug"` — plain string path in listening-notes (for Bases filtering)
- `entry-count: N` — number of log entries in a listening-notes file

## Gotchas

- `rmdir` on empty dirs may fail with `[Errno 16] Resource busy` on macOS (Spotlight) — use try/except OSError, not shutil.rmtree
- Scripts process thousands of files and run slowly; expect background execution
- `_index.md` is the primary entry point for topic-based podcast search — load it before scanning individual files
- `summarize.py` backend is configured in `scripts/feeds.yaml` under `summarizer.backend` (claude or omlx)
- All scripts use `python3`, not `python`
- PyYAML parses bare YAML dates as `datetime.date` objects — normalize with `.isoformat()` before string operations
- `intake.py` moves failed inbox items to `podcasts/_inbox/_unresolved/` with an error comment; re-add an RSS URL to frontmatter and re-run to retry
- `scan_journals.py` is in this repo but its vault scan targets still point to relationship-agent — path updates are a separate follow-up
```

- [ ] **Step 3: Stage and commit**

```bash
cd /Volumes/💩/ai/podcast-vault
git add CLAUDE.md
git commit -m "init: podcast-vault repo with CLAUDE.md"
```

---

### Task 2: Copy and update scripts

**Files:**
- Copy: `relationship-agent/scripts/learning/` → `podcast-vault/scripts/`
- Modify: `podcast-vault/scripts/fetch.py` — lines 24, 25, 221
- Modify: `podcast-vault/scripts/summarize.py` — line 20
- Modify: `podcast-vault/scripts/intake.py` — lines 25, 26, 27, 241, 243
- Modify: `podcast-vault/scripts/feeds.yaml` — `ignore_folders` block
- Create: `podcast-vault/scripts/CLAUDE.md`

- [ ] **Step 1: Copy scripts directory**

```bash
cp -r /Volumes/💩/ai/relationship-agent/scripts/learning /Volumes/💩/ai/podcast-vault/scripts
```

Verify:

```bash
ls /Volumes/💩/ai/podcast-vault/scripts/
```

Expected: `CLAUDE.md  __init__.py  feeds.yaml  fetch.py  intake.py  migrate.py  reorganize.py  requirements.txt  resolve.py  scan_journals.py  summarize.py`

- [ ] **Step 2: Update fetch.py path constants**

In `podcast-vault/scripts/fetch.py`, replace lines 24–25:

Old:
```python
MARKDOWN_DIR = BASE_DIR.parent.parent / "vault" / "learning" / "podcasts"
ASSETS_DIR = BASE_DIR.parent.parent / "assets"
```

New:
```python
MARKDOWN_DIR = BASE_DIR.parent / "podcasts"
ASSETS_DIR = BASE_DIR.parent / "assets"
```

- [ ] **Step 3: Update fetch.py audio path string**

In `podcast-vault/scripts/fetch.py`, find the `fm` dict construction in `fetch_feed` (around line 215). Replace:

Old:
```python
            "audio": f"../../../../assets/{safe}/{slug}{ext}",
```

New:
```python
            "audio": f"../../assets/{safe}/{slug}{ext}",
```

- [ ] **Step 4: Update summarize.py path constant**

In `podcast-vault/scripts/summarize.py`, replace line 20:

Old:
```python
MARKDOWN_DIR = BASE_DIR.parent.parent / "vault" / "learning" / "podcasts"
```

New:
```python
MARKDOWN_DIR = BASE_DIR.parent / "podcasts"
```

- [ ] **Step 5: Update intake.py path constants**

In `podcast-vault/scripts/intake.py`, replace lines 25–27:

Old:
```python
VAULT_DIR = BASE_DIR.parent.parent / "vault"
LISTENING_NOTES_DIR = VAULT_DIR / "learning" / "listening-notes"
INBOX_DIR = VAULT_DIR / "learning" / "podcasts" / "_inbox"
```

New:
```python
VAULT_DIR = BASE_DIR.parent
LISTENING_NOTES_DIR = VAULT_DIR / "listening-notes"
INBOX_DIR = VAULT_DIR / "podcasts" / "_inbox"
```

- [ ] **Step 6: Update intake.py audio path strings**

In `podcast-vault/scripts/intake.py`, find the two audio path strings in `process_entry` (around lines 241–243). Replace:

Old:
```python
            audio_rel = f"../../../assets/{safe}/{slug}{ext}"
        else:
            audio_rel = f"../../../assets/{safe}/{slug}.mp3"
```

New:
```python
            audio_rel = f"../../assets/{safe}/{slug}{ext}"
        else:
            audio_rel = f"../../assets/{safe}/{slug}.mp3"
```

- [ ] **Step 7: Update feeds.yaml ignore_folders**

In `podcast-vault/scripts/feeds.yaml`, find the `ignore_folders` block at the bottom:

Old:
```yaml
  ignore_folders:
    - vault/learning/podcasts
    - vault/learning/listening-notes
```

New:
```yaml
  ignore_folders:
    - podcasts
    - listening-notes
```

- [ ] **Step 8: Write scripts/CLAUDE.md**

Replace the contents of `podcast-vault/scripts/CLAUDE.md` with:

```markdown
# scripts — podcast processing pipeline

All scripts for downloading, transcribing, summarizing, and indexing podcast episodes.

## Commands

```bash
python3 scripts/fetch.py                    # download new episodes from RSS
python3 scripts/summarize.py                # summarize pending episodes
python3 scripts/summarize.py --reindex      # rebuild READMEs and topic index
python3 scripts/migrate.py --dry-run        # preview un-migrated files (should be 0)
python3 scripts/intake.py                   # process all inbox files
python3 scripts/intake.py --episode "URL or name" --notes "text"
python3 scripts/intake.py --re-summarize "path/to/notes.md"
python3 scripts/scan_journals.py            # scan vault for podcast references (paths need update — see below)
python3 scripts/scan_journals.py --since 2026-04-01
python3 scripts/scan_journals.py --dry-run
```

## Configuration

`scripts/feeds.yaml` — RSS feed URLs, summarizer config, journal_scan folders.
Add a new feed by appending an entry under `feeds:` with `url` and `name` fields.
Summarizer backend: set `summarizer.backend` to `claude` or `omlx`.

## scan_journals.py — paths not yet updated

`scan_journals.py` scans relationship-agent vault folders for podcast references. Its internal `VAULT_DIR` and journal `folders` config still point to relationship-agent paths — update these before running.

## Gotchas

- `rmdir` on empty dirs may fail with `[Errno 16] Resource busy` on macOS (Spotlight). Use try/except OSError.
- Scripts process thousands of files — expect slow runs. Use background execution.
- `_index.md` is the primary entry point for topic-based search.
- PyYAML parses bare YAML dates as `datetime.date` objects — normalize with `.isoformat()` before string operations.
- `intake.py` moves failed inbox items to `podcasts/_inbox/_unresolved/` with an error comment.
- `scan_journals.py` is read-only — it never modifies journal source files.
```

- [ ] **Step 9: Verify scripts import cleanly**

```bash
cd /Volumes/💩/ai/podcast-vault
python3 -c "import sys; sys.path.insert(0, 'scripts'); import fetch; print('MARKDOWN_DIR:', fetch.MARKDOWN_DIR); print('ASSETS_DIR:', fetch.ASSETS_DIR)"
```

Expected output:
```
MARKDOWN_DIR: /Volumes/💩/ai/podcast-vault/podcasts
ASSETS_DIR: /Volumes/💩/ai/podcast-vault/assets
```

- [ ] **Step 10: Commit**

```bash
cd /Volumes/💩/ai/podcast-vault
git add scripts/
git commit -m "add: processing scripts with updated paths"
```

---

### Task 3: Migrate content

**Files:**
- Copy then delete: `relationship-agent/vault/learning/podcasts/` → `podcast-vault/podcasts/`
- Copy then delete: `relationship-agent/vault/learning/listening-notes/` → `podcast-vault/listening-notes/`
- Copy then delete: `relationship-agent/assets/` → `podcast-vault/assets/`

- [ ] **Step 1: Copy podcasts/**

```bash
cp -r /Volumes/💩/ai/relationship-agent/vault/learning/podcasts /Volumes/💩/ai/podcast-vault/podcasts
```

This is ~143MB and may take a moment.

- [ ] **Step 2: Copy listening-notes/**

```bash
cp -r /Volumes/💩/ai/relationship-agent/vault/learning/listening-notes /Volumes/💩/ai/podcast-vault/listening-notes
```

- [ ] **Step 3: Copy assets/**

```bash
cp -r /Volumes/💩/ai/relationship-agent/assets /Volumes/💩/ai/podcast-vault/assets
```

This is ~19MB.

- [ ] **Step 4: Verify counts match**

```bash
echo "podcasts dirs:" && ls /Volumes/💩/ai/relationship-agent/vault/learning/podcasts | wc -l && ls /Volumes/💩/ai/podcast-vault/podcasts | wc -l
echo "assets dirs:" && ls /Volumes/💩/ai/relationship-agent/assets | wc -l && ls /Volumes/💩/ai/podcast-vault/assets | wc -l
```

Expected: both counts match for each pair.

- [ ] **Step 5: Commit content to podcast-vault**

```bash
cd /Volumes/💩/ai/podcast-vault
git add podcasts/ listening-notes/ assets/
git commit -m "migrate: podcasts, listening-notes, and assets from relationship-agent"
```

Note: this commit may be large (143MB+). Git will handle it — just allow time.

- [ ] **Step 6: Delete migrated content from relationship-agent**

```bash
rm -rf /Volumes/💩/ai/relationship-agent/vault/learning
rm -rf /Volumes/💩/ai/relationship-agent/assets
rm -rf /Volumes/💩/ai/relationship-agent/scripts/learning
```

- [ ] **Step 7: Verify relationship-agent structure**

```bash
ls /Volumes/💩/ai/relationship-agent/vault/
ls /Volumes/💩/ai/relationship-agent/scripts/
```

Expected: `vault/` no longer contains `learning/`. `scripts/` no longer contains `learning/` but still has `self/`, `tests/`, `utils/`.

- [ ] **Step 8: Commit relationship-agent deletions**

```bash
cd /Volumes/💩/ai/relationship-agent
git add -A
git commit -m "remove: learning domain (podcast vault extracted to podcast-vault repo)"
```

---

### Task 4: Patch audio paths in migrated episode files

All existing episode markdown files have `audio: ../../../../assets/...` (correct for the old 4-level depth). In the new repo they live at `podcasts/ShowName/episode.md` — only 2 levels from repo root — so the path must become `../../assets/...`.

**Files:**
- Modify (bulk): all `*.md` files under `podcast-vault/podcasts/` that contain `audio: ../../../../assets/`

- [ ] **Step 1: Run the patch script**

```bash
python3 - <<'EOF'
import re
from pathlib import Path

podcasts_dir = Path("/Volumes/💩/ai/podcast-vault/podcasts")
old = re.compile(r'^(audio:\s*)\.\.\/\.\.\/\.\.\/\.\.\/(assets/)', re.MULTILINE)
new_repl = r'\1../../\2'
patched = 0

for md in podcasts_dir.rglob("*.md"):
    text = md.read_text(encoding="utf-8")
    updated = old.sub(new_repl, text)
    if updated != text:
        md.write_text(updated, encoding="utf-8")
        patched += 1

print(f"Patched {patched} files.")
EOF
```

Expected: `Patched N files.` where N is in the hundreds.

- [ ] **Step 2: Spot-check a patched file**

```bash
grep "^audio:" /Volumes/💩/ai/podcast-vault/podcasts/$(ls /Volumes/💩/ai/podcast-vault/podcasts | grep -v "^_" | head -1)/$(ls /Volumes/💩/ai/podcast-vault/podcasts/$(ls /Volumes/💩/ai/podcast-vault/podcasts | grep -v "^_" | head -1)/ | grep "\.md$" | grep -v README | head -1)
```

Expected: `audio: ../../assets/...`

- [ ] **Step 3: Verify no old paths remain**

```bash
grep -r "audio: \.\./\.\./\.\./\.\./assets/" /Volumes/💩/ai/podcast-vault/podcasts/ | wc -l
```

Expected: `0`

- [ ] **Step 4: Commit patched files**

```bash
cd /Volumes/💩/ai/podcast-vault
git add podcasts/
git commit -m "fix: update audio frontmatter paths to new 2-level depth"
```

---

### Task 5: Update relationship-agent CLAUDE.md

**Files:**
- Modify: `/Volumes/💩/ai/relationship-agent/CLAUDE.md`

- [ ] **Step 1: Remove learning domain from Domain Structure block**

In `relationship-agent/CLAUDE.md`, remove the `learning/` line from the vault structure and the `scripts/learning/` line from the scripts structure and the `assets/` line from the layout:

Old block:
```
vault/
  conversations/   ← durable record: iMessage threads, voice-memo transcripts, summaries
  relationship/    ← marriage KB, discernment, therapy, topic patterns
  self/            ← ADHD + addiction self-management, daily tracking
  learning/        ← podcast archive + personal listening notes
  parenting/       ← per-child profiles, goals, newsletters; calendar via MCP
  inbox/           ← top-level staging inbox (unrouted items)

scripts/
  learning/        ← podcast fetch, summarize, intake, journal scan
  self/            ← daily note management
  tests/
  utils/

assets/            ← raw audio + .txt transcript files
.claude/           ← Claude config + skills
docs/              ← specs, plans
```

New block:
```
vault/
  conversations/   ← durable record: iMessage threads, voice-memo transcripts, summaries
  relationship/    ← marriage KB, discernment, therapy, topic patterns
  self/            ← ADHD + addiction self-management, daily tracking
  parenting/       ← per-child profiles, goals, newsletters; calendar via MCP
  inbox/           ← top-level staging inbox (unrouted items)

scripts/
  self/            ← daily note management
  tests/
  utils/

.claude/           ← Claude config + skills
docs/              ← specs, plans
```

- [ ] **Step 2: Remove the Learning commands block**

Remove the entire `# Learning (podcast archive)` command block from the Commands section:

Old:
```bash
# Learning (podcast archive)
python3 scripts/learning/fetch.py                    # download new episodes
python3 scripts/learning/summarize.py                # summarize pending episodes
python3 scripts/learning/summarize.py --reindex      # rebuild READMEs and topic index
python3 scripts/learning/migrate.py --dry-run        # preview un-migrated files (should be 0)
python3 scripts/learning/intake.py                   # process all inbox files
python3 scripts/learning/intake.py --episode "URL or name" --notes "text"
python3 scripts/learning/intake.py --re-summarize "path/to/notes.md"
python3 scripts/learning/scan_journals.py            # scan vault for podcast references
python3 scripts/learning/scan_journals.py --since 2026-04-01
python3 scripts/learning/scan_journals.py --dry-run

# Self
python3 scripts/self/daily.py                        # create/update today's current note
```

New:
```bash
# Self
python3 scripts/self/daily.py                        # create/update today's current note
```

- [ ] **Step 3: Remove the Key Frontmatter Fields section**

Remove the entire `## Key Frontmatter Fields (learning domain)` section:

Old (remove this entire block):
```markdown
## Key Frontmatter Fields (learning domain)

- `podcast-archive-type: summary | transcript | listening-notes`
- `status: downloaded | summarized` — episode files
- `status: in-progress | complete` — listening-notes files
- `audio: ../../../assets/Podcast Name/slug.mp3` — relative from summary file
- `episode-file: "podcasts/Podcast Name/episode-slug"` — plain string path in listening-notes (for Bases filtering)
- `entry-count: N` — number of log entries in a listening-notes file
```

- [ ] **Step 4: Remove learning-specific gotchas**

In the `## Gotchas` section, remove these three lines:

```
- Scripts process thousands of files and run slowly; expect background execution
- `_index.md` is the primary entry point for topic-based podcast search — load it before scanning individual files
- `summarize.py` backend is configured in `scripts/learning/feeds.yaml` under `summarizer.backend` (claude or omlx)
```

Also update the intake.py gotcha to reflect new path:

Old:
```
- `intake.py` moves failed inbox items to `vault/learning/podcasts/_inbox/_unresolved/` with an error comment; re-add an RSS URL to frontmatter and re-run to retry
```

Remove this line entirely (intake.py is no longer in relationship-agent).

- [ ] **Step 5: Commit**

```bash
cd /Volumes/💩/ai/relationship-agent
git add CLAUDE.md
git commit -m "update: remove learning domain from CLAUDE.md (extracted to podcast-vault)"
```

---

### Task 6: Update adhd-coach SKILL.md files

Three copies contain `vault/podcasts/` references that now resolve to the wrong repo. All three get the same update: `vault/podcasts/` → `/Volumes/💩/ai/podcast-vault/podcasts/`.

**Files:**
- Modify: `/Volumes/💩/ai/relationship-agent/.claude/skills/adhd-coach/SKILL.md`
- Modify: `/Volumes/💩/ai/relationship-agent/.agents/skills/adhd-coach/SKILL.md`
- Modify: `/Volumes/💩/ai/Workboxes/.claude/skills/adhd-coach/SKILL.md`

- [ ] **Step 1: Update relationship-agent .claude copy**

In `/Volumes/💩/ai/relationship-agent/.claude/skills/adhd-coach/SKILL.md`, replace all occurrences of `vault/podcasts/` with `/Volumes/💩/ai/podcast-vault/podcasts/`.

There are three occurrences:
1. In the `description:` frontmatter field: `...both in vault/podcasts/.`
2. In the Purpose paragraph: `...both archived in \`vault/podcasts/\``
3. In the Hard Refusals section: `...source material in \`vault/podcasts/\``

- [ ] **Step 2: Update relationship-agent .agents copy**

In `/Volumes/💩/ai/relationship-agent/.agents/skills/adhd-coach/SKILL.md`, make the same three replacements.

- [ ] **Step 3: Commit relationship-agent skill updates**

```bash
cd /Volumes/💩/ai/relationship-agent
git add .claude/skills/adhd-coach/SKILL.md .agents/skills/adhd-coach/SKILL.md
git commit -m "update: adhd-coach skill paths to podcast-vault"
```

- [ ] **Step 4: Update Workboxes .claude copy**

In `/Volumes/💩/ai/Workboxes/.claude/skills/adhd-coach/SKILL.md`, make the same three replacements.

- [ ] **Step 5: Commit Workboxes skill update**

```bash
cd /Volumes/💩/ai/Workboxes
git add .claude/skills/adhd-coach/SKILL.md
git commit -m "update: adhd-coach skill paths to podcast-vault"
```

---

### Self-Review

**Spec coverage:**
- ✅ New repo `podcast-vault` at `/Volumes/💩/ai/podcast-vault` — Task 1
- ✅ Top-level structure (podcasts/, listening-notes/, assets/, scripts/) — Tasks 1, 2, 3
- ✅ Scripts moved and path-updated — Task 2
- ✅ Content migrated and deleted from relationship-agent — Task 3
- ✅ Existing audio frontmatter paths patched — Task 4
- ✅ relationship-agent CLAUDE.md updated — Task 5
- ✅ adhd-coach SKILL.md updated in all 3 copies — Task 6
- ✅ feeds.yaml ignore_folders updated — Task 2, Step 7
- ✅ podcast-vault gets its own CLAUDE.md — Task 1
- ✅ scan_journals.py moves with scripts but path updates are out of scope — noted in Task 3 Step 6 and scripts/CLAUDE.md
- ✅ Workboxes podcast-log.md stays in place — not touched

**Out of scope (noted, not implemented):**
- scan_journals.py internal path updates (VAULT_DIR, journal scan folders)

# Life Management Consolidation Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Consolidate five overlapping life-management repos into two — Workboxes (daily hub) and relationship-agent (deep work) — while retiring Memory, Relationship, and Relationship2.

**Architecture:** Workboxes gets a rewritten CLAUDE.md absorbing the identity layer from Memory, a new TOP_OF_MIND.md dashboard, trimmed _system/ and slash commands, migrated family and relationship content into Areas/, and four everyday skills. relationship-agent is unchanged structurally; it receives deep background content from retiring repos. Memory, Relationship, and Relationship2 are retired via rename/delete after content is verified.

**Tech Stack:** Markdown files, git, macOS filesystem (cp/mv/rm). No code changes.

---

## File Map

**Create:**
- `Workboxes/_system/TOP_OF_MIND.md`
- `Workboxes/Areas/Relationships/roxy-profile.md`
- `Workboxes/Areas/Relationships/Projects/commitments.md`
- `Workboxes/.claude/skills/morning/SKILL.md`
- `Workboxes/.claude/skills/sync/SKILL.md`
- `Workboxes/.claude/skills/adhd-coach/SKILL.md` + `references/`
- `Workboxes/.claude/skills/vulnerability-coach/SKILL.md` + `references/`
- `relationship-agent/vault/relationship/memory-background.md` (new, from Memory deep background)

**Modify:**
- `Workboxes/CLAUDE.md` — full rewrite with identity layer
- `Workboxes/_system/README.md` — remove voice.md reference
- `Workboxes/Areas/Family/_index.md` — add contacts, calendar, deep background
- `Workboxes/Areas/Family/Oscar.md` — add active crisis thread
- `Workboxes/Areas/Relationships/_index.md` — add behavioral reminders + open items

**Move:**
- `Workboxes/_system/growth-prompts.md` → `Workboxes/Areas/Growth/growth-prompts.md`
- `Workboxes/_system/growth-surfacing.md` → `Workboxes/Areas/Growth/growth-surfacing.md`

**Delete:**
- `Workboxes/_system/voice.md`
- `Workboxes/_system/school-email-senders.md`
- `Workboxes/.claude/commands/review-quarterly.md`
- `Workboxes/.claude/commands/growth-new.md`
- `Workboxes/.claude/commands/school-email-triage.md`

**Retire (rename at root):**
- `/Volumes/💩/ai/Relationship2/` → delete (empty)
- `/Volumes/💩/ai/Memory/` → `_retired-Memory/`
- `/Volumes/💩/ai/Relationship/` → `_retired-Relationship/`

---

## Task 1: Rewrite Workboxes/CLAUDE.md

**Files:**
- Modify: `Workboxes/CLAUDE.md`

- [ ] **Step 1: Overwrite CLAUDE.md with the unified identity document**

Write the following content exactly — this merges Memory/CLAUDE.md + voice.md + the existing Workboxes CLAUDE.md into one authoritative file:

```markdown
# Workboxes — Claude Instructions

## Who I Am
- **Mike** — Staff iOS Engineer at Etsy, born June 21, 1985
- Family: wife **Roxy**, kids **Oscar** (14, 8th grade), **Nigel** (10, 4th grade), **Avery** (7, 1st grade)
- I have **ADHD** — treat this as my external brain and executive assistant

## Values

1. **Intentionality** — Living and choosing on purpose, not reacting. Every other value is filtered through this one.
2. **Wholeness / Authenticity** — Showing up as my whole self across all areas — not a different person at work vs. home vs. with Roxy vs. as a parent.
3. **Reflection & Growth** — Self-awareness with forward motion. Reflection that translates into change, not just insight.
4. **Family** — Deep care for Roxy, Oscar, Nigel, and Avery.
5. **Joy** — Restoration that actually restores. The signal that the rest of the system is working.

Use values as a prioritization filter: when two priorities collide, the one closer to a top value wins.

## Communication Style
- Direct and warm. Don't over-explain — I'll ask if I need more.
- Challenge me if I'm overcomplicating things or there's a better approach.
- Short responses by default. Prose over bullets unless I ask.
- Don't recap what I just said. Get to the point.

## My Stack
- **Work**: iOS/Swift (staff engineer at Etsy)
- **Productivity**: Things 3, Drafts, Day One, Craft, Bear Notes
- **Calendar/Email**: Google Suite — push school email events to iCloud **Personal** calendar (`4b08b37e403d9a74ad653ca58c80184cba6e7f3d`), not the Google default
- **Ecosystem**: Apple-first (iPhone, Mac, iCloud)
- **Scripts**: Swift preferred for automation

## Executive Assistant Rules
- **Always read `_system/TOP_OF_MIND.md` first** — before any planning, prioritization, or advice
- Check `Areas/<area>/_index.md` and project files before responding about ongoing situations
- Surface what I might be dropping; flag things close to deadlines
- When I'm scattered, help me pick ONE next thing
- Default to action over analysis — if something is clear enough to start, say so
- Update `TOP_OF_MIND.md` when you learn something new that belongs there; hard cap of 15 items — prompt review before adding when at 14+

## Available Skills
- `/morning` — daily briefing
- `/sync` — external app sync
- `/adhd-coach` — reset, urge surf, trace-backward, weekly review
- `/vulnerability-coach` — vulnerability work

## Vault Structure
Read `_system/README.md` for full operating conventions (triage rules, schema, slash commands).
```

- [ ] **Step 2: Verify**

```bash
head -5 /Volumes/💩/ai/Workboxes/CLAUDE.md
```

Expected: `# Workboxes — Claude Instructions`

- [ ] **Step 3: Commit**

```bash
cd /Volumes/💩/ai/Workboxes
git add CLAUDE.md
git commit -m "rewrite CLAUDE.md: absorb identity layer from Memory and voice.md"
```

---

## Task 2: Create TOP_OF_MIND.md

**Files:**
- Create: `Workboxes/_system/TOP_OF_MIND.md`

- [ ] **Step 1: Create the file with current items from Memory**

Write this content to `Workboxes/_system/TOP_OF_MIND.md`:

```markdown
# Top of Mind

> Flat list of what's live across all areas. Check this at the start of any planning session.
> Limit: **15 items**. When full, review what to deprioritize before adding anything new.

*Updated: 2026-05-13 — 11 of 15 items*

---

1. **[family]** ⚠️ Oscar Cedar Point trip — permission slip + $100 due **May 7th** (likely passed — confirm status with Oscar/Roxy)

2. **[family]** ⚠️ Avery planetarium field trip **May 8th** — Roxy selected as chaperone; confirm her background check is on file with school office

3. **[family]** Oscar meds — Dr. Kennedy appointment Tue May 5. Script expired; refill expected. Confirm received.

4. **[family]** Oscar behavior contract — stalled; both Mike and Roxy want to restart. Locate draft (was on Roxy's laptop) and schedule time to work on it together.

5. **[family]** Oscar at school under in-school supervision — school is holding escalating consequences. Mike trying to let them land without softening.

6. **[family]** Mike's parenting limits with Oscar — write them down before the next flare-up. Don't invent them under pressure.

7. **[relationship]** Roxy's anger hasn't faded the way she expected — she said she's "waiting to see how long." This is live and unresolved.

8. **[relationship]** Don't bring grief or therapeutic processing to Roxy — she named it directly on May 1. Find other containers (Kim, journal, friends).

9. **[relationship]** Roxy's coworker concern — left open at end of May 1 conversation. She may want to revisit. Don't push; let her bring it up.

10. **[relationship]** Kim session — no follow-up scheduled. Mike wanted to discuss format with Roxy before booking. Still needs to happen.

11. **[self]** ADHD grief work — mentioned with Tanya on May 1. Not yet scheduled or structured. Worth making explicit.
```

- [ ] **Step 2: Verify**

```bash
grep "15 items" /Volumes/💩/ai/Workboxes/_system/TOP_OF_MIND.md
```

Expected: `> Limit: **15 items**`

- [ ] **Step 3: Commit**

```bash
cd /Volumes/💩/ai/Workboxes
git add _system/TOP_OF_MIND.md
git commit -m "add _system/TOP_OF_MIND.md: live dashboard from Memory"
```

---

## Task 3: Slim _system/ — delete voice.md, move growth files, drop orphaned school file

**Files:**
- Delete: `Workboxes/_system/voice.md`
- Delete: `Workboxes/_system/school-email-senders.md`
- Move: `Workboxes/_system/growth-prompts.md` → `Workboxes/Areas/Growth/growth-prompts.md`
- Move: `Workboxes/_system/growth-surfacing.md` → `Workboxes/Areas/Growth/growth-surfacing.md`
- Modify: `Workboxes/_system/README.md`

- [ ] **Step 1: Delete voice.md and school-email-senders.md**

```bash
cd /Volumes/💩/ai/Workboxes
rm _system/voice.md _system/school-email-senders.md
```

- [ ] **Step 2: Move growth files to Areas/Growth/**

```bash
cd /Volumes/💩/ai/Workboxes
git mv _system/growth-prompts.md Areas/Growth/growth-prompts.md
git mv _system/growth-surfacing.md Areas/Growth/growth-surfacing.md
```

- [ ] **Step 3: Remove voice.md reference from _system/README.md**

Read `_system/README.md` and find any mention of `voice.md` or the Voice section. Remove those references. The voice/communication content now lives in `CLAUDE.md`. Do not change anything else.

- [ ] **Step 4: Verify _system/ no longer contains deleted or moved files**

```bash
ls /Volumes/💩/ai/Workboxes/_system/
```

Expected: README.md, capture-sources.md, changelog.md, schemas/, sync-state.json, triage-rules.md, workbox-schema.md, TOP_OF_MIND.md — no voice.md, no school-email-senders.md, no growth-prompts.md, no growth-surfacing.md

- [ ] **Step 5: Commit**

```bash
cd /Volumes/💩/ai/Workboxes
git add -A
git commit -m "slim _system/: drop voice.md, school-email-senders; move growth files to Areas/Growth/"
```

---

## Task 4: Trim slash commands

**Files:**
- Delete: `Workboxes/.claude/commands/review-quarterly.md`
- Delete: `Workboxes/.claude/commands/growth-new.md`
- Delete: `Workboxes/.claude/commands/school-email-triage.md`

- [ ] **Step 1: Delete the three unused commands**

```bash
cd /Volumes/💩/ai/Workboxes
rm .claude/commands/review-quarterly.md .claude/commands/growth-new.md .claude/commands/school-email-triage.md
```

- [ ] **Step 2: Verify only the three kept commands remain**

```bash
ls /Volumes/💩/ai/Workboxes/.claude/commands/
```

Expected: `review-daily.md  review-weekly.md  triage.md` (plus `settings.local.json` in `.claude/`)

- [ ] **Step 3: Commit**

```bash
cd /Volumes/💩/ai/Workboxes
git add -A
git commit -m "trim slash commands: keep triage, review-daily, review-weekly only"
```

---

## Task 5: Migrate Memory/family/ → Areas/Family/

**Source files to read:**
- `Memory/family/MEMORY.md`
- `Memory/family/RESOURCES.md`
- `Memory/family/CALENDAR.md`
- `Memory/family/Oscar/` (all files)

**Files:**
- Modify: `Workboxes/Areas/Family/_index.md`
- Modify: `Workboxes/Areas/Family/Oscar.md`

- [ ] **Step 1: Read all Memory/family/ source files**

```bash
find /Volumes/💩/ai/Memory/family -type f | sort
```

Read each file. Note:
- Contact information (school emails, therapist Jason, Dr. Kennedy contact)
- Birthday dates for all family members
- Calendar scaffold (recurring annual events)
- Oscar active thread details (behavior contract, medication status, in-school supervision, Cedar Point, parenting limits)
- Deep background (Roxy's brother's death by suicide shapes her urgency re: Oscar; band trip as trust anchor; Oscar's ADHD/entitlement pattern)

- [ ] **Step 2: Update Areas/Family/_index.md**

Keep the existing frontmatter and structure. Add sections below the existing content:

```markdown
## Key Contacts

*Read `/Volumes/💩/ai/Memory/family/RESOURCES.md` and fill in the actual values below:*

- **Oscar's therapist**: Jason — [phone/email from RESOURCES.md]
- **Oscar's prescribing doctor**: Dr. Kennedy — quarterly med check-ins — [contact from RESOURCES.md]
- **Schools**:
  - MacDonald Middle School (Oscar) — [office email from RESOURCES.md]
  - Robert L. Green Elementary (Nigel, Avery) — [office email from RESOURCES.md]

## Birthdays & Anniversaries

- Roxy: Feb 2
- Oscar: March 25
- Nigel: April 15
- Avery: Jan 4
- Mike: June 21
- Wedding anniversary: Aug 6

## Annual Calendar

[Copy the recurring event scaffold from Memory/family/CALENDAR.md]

## Background (rarely changes)

- **Roxy's brother** died by suicide. His entitlement and boundary-crossing shape her urgency and zero-tolerance around Oscar's behavior. This is load-bearing context.
- **Band trip (2024)** — Mike renegotiated an agreement with Oscar under pressure. Roxy withdrew and has held the memory. This is the anchor trust-breach event in their co-parenting relationship.
- **Oscar's ADHD** (shares with Mike) — pattern: dysregulation → entitlement → demands reparations. He is currently in in-school supervision.
- Roxy's co-parenting ask: not identical values, but a boundary parent she can predict, respect, and negotiate with.
```

Fill in the exact contact details by reading Memory/family/RESOURCES.md.

- [ ] **Step 3: Update Areas/Family/Oscar.md**

Note: The spec listed `Areas/Family/Projects/` as the destination, but Oscar's crisis content is status/context across multiple issues (medication, contract, supervision) rather than a single project. `Oscar.md` is the existing per-kid profile file and the correct home for this. The behavior contract specifically may warrant its own `Projects/oscar-behavior-contract/` later, but for now consolidate here.

Read the existing `Workboxes/Areas/Family/Oscar.md`. Add a section at the top (before any existing content) called `## Active Thread` with the current crisis content:

```markdown
## Active Thread

*As of May 2026*

- Under in-school supervision — school holding escalating consequences; Mike letting them land without softening
- Behavior contract: stalled. Both Mike and Roxy want to restart. Locate draft (was on Roxy's laptop). Schedule time together.
- Medication: Dr. Kennedy manages quarterly. Script expired May 2026 — refill expected from May 5 appointment. Mike has backup dose.
- Cedar Point trip: permission slip + $100 was due May 7. Confirm status.
- **Mike's task**: Write down parenting limits with Oscar before the next flare-up. Don't invent them under pressure.
```

- [ ] **Step 4: Verify**

```bash
grep "Dr. Kennedy" /Volumes/💩/ai/Workboxes/Areas/Family/_index.md
grep "Active Thread" /Volumes/💩/ai/Workboxes/Areas/Family/Oscar.md
```

Both should return matches.

- [ ] **Step 5: Commit**

```bash
cd /Volumes/💩/ai/Workboxes
git add Areas/Family/_index.md Areas/Family/Oscar.md
git commit -m "migrate Memory/family/: add contacts, calendar, background, Oscar active thread"
```

---

## Task 6: Migrate Memory/relationship/ → Areas/Relationships/ + relationship-agent

**Source files to read:**
- `Memory/relationship/MEMORY.md`
- `Memory/relationship/RESOURCES.md`

**Files:**
- Modify: `Workboxes/Areas/Relationships/_index.md`
- Create: `relationship-agent/vault/relationship/memory-background.md`

- [ ] **Step 1: Read source files**

```bash
cat /Volumes/💩/ai/Memory/relationship/MEMORY.md
cat /Volumes/💩/ai/Memory/relationship/RESOURCES.md
```

Note:
- **Daily behavioral reminders** (go to Workboxes): Don't bring grief/therapy processing to Roxy — she named it May 1. Action > conversation for rebuilding credibility. Don't push for more contact than she can hold. "Avoid Mike" is her current decompression — respect it.
- **Open action items** (go to Workboxes): behavior contract ownership, Kim therapy format discussion, coworker conversation follow-up
- **Deep background** (go to relationship-agent): bank statement moment definition, Roxy's nervous system history (gaslit into ignoring her signals — she will not suppress fight/flight/freeze), Mike's ADHD emotional avoidance pattern (hard feelings boxed up, expressed as anger — rooted in childhood), grief/visible processing reducing Roxy's attraction

- [ ] **Step 2: Update Areas/Relationships/_index.md**

Keep existing frontmatter. Replace `(none)` placeholders with:

```markdown
## Daily Reminders (read before any interaction planning)

- **Don't bring grief or therapeutic processing to Roxy.** She named this directly May 1. It lands as excuses. Use Kim, journal, or friends as containers.
- **Action over conversation.** Parenting credibility with Roxy is rebuilt by doing things, not discussing them.
- **Don't push for more than she can hold.** "Avoid Mike" is her current decompression strategy. Respect it.
- **Roxy's anger is not fading on the expected timeline.** She said she's "waiting to see how long." This is unresolved and active.

## Open Items

- Behavior contract: needs restart. Locate draft (was on Roxy's laptop). Needs clear ownership.
- Kim session format: Mike wanted to discuss with Roxy before booking next session. Still pending.
- Roxy's coworker concern: left open at end of May 1 conversation. Let her bring it up; don't push.

## Projects

[add Workboxes project links here as they are created]
```

- [ ] **Step 3: Create relationship-agent/vault/relationship/memory-background.md**

Check what already exists in `relationship-agent/vault/relationship/`:

```bash
ls /Volumes/💩/ai/relationship-agent/vault/relationship/
```

Create `memory-background.md` with the deep background that doesn't already exist in the vault:

```markdown
# Relationship Background (from Memory migration)

*Migrated 2026-05-13 from Memory/relationship/*

## The Trust Rupture

Named by Roxy as the "bank statement moment." An alcohol situation where Mike could not stop for nearly a week despite Roxy expressing fear and impact. This is the primary breach.

## Roxy's Nervous System

Roxy has a history of being gaslit into ignoring her nervous system signals. She will not suppress fight/flight/freeze responses. This is non-negotiable and shapes how she interprets every interaction.

## Mike's Emotional Avoidance Pattern

Rooted in childhood — hard feelings get boxed up and expressed as anger. This is a documented recurring dynamic. Mike's visible emotional processing and grief is currently reducing Roxy's attraction.

## Communication Channel

iMessage is the primary channel. Roxy reads but does not always respond quickly. Don't follow up the same day.
```

Before creating, check if any of this content already exists in existing vault files to avoid duplication.

- [ ] **Step 4: Verify**

```bash
grep "Daily Reminders" /Volumes/💩/ai/Workboxes/Areas/Relationships/_index.md
grep "bank statement" /Volumes/💩/ai/relationship-agent/vault/relationship/memory-background.md
```

Both should match.

- [ ] **Step 5: Commit both repos**

```bash
cd /Volumes/💩/ai/Workboxes
git add Areas/Relationships/_index.md
git commit -m "migrate Memory/relationship/: add daily reminders and open items to Relationships area"

cd /Volumes/💩/ai/relationship-agent
git add vault/relationship/memory-background.md
git commit -m "add memory-background.md: deep background migrated from Memory/relationship/"
```

---

## Task 7: Migrate Relationship/topics/ → Areas/Relationships/ + relationship-agent

**Source files to read:**
- `Relationship/topics/preferences.md`
- `Relationship/topics/desires.md`
- `Relationship/topics/triggers.md`
- `Relationship/topics/milestones.md`
- `Relationship/topics/requests.md`
- `Relationship/topics/commitments.md`
- `Relationship/topics/open-questions.md`

**Files:**
- Create: `Workboxes/Areas/Relationships/roxy-profile.md`
- Create: `Workboxes/Areas/Relationships/Projects/commitments.md`
- Merge: `Relationship/topics/open-questions.md` → `relationship-agent/vault/relationship/`

- [ ] **Step 1: Read all source topic files**

```bash
for f in preferences desires triggers milestones requests commitments open-questions; do
  echo "=== $f ===" && cat "/Volumes/💩/ai/Relationship/topics/$f.md"
done
```

Also read the existing `Workboxes/Areas/Relationships/desire.md` — it may overlap with desires.md from Relationship/. Consolidate rather than duplicate.

- [ ] **Step 2: Create Areas/Relationships/roxy-profile.md**

Combine preferences.md, desires.md (reconciled with existing desire.md), triggers.md, and milestones.md into a single profile file:

```markdown
---
type: reference
area: Relationships
updated: 2026-05-13
---

# Roxy — Profile

## Preferences
[Content from Relationship/topics/preferences.md]

## Desires
[Content from Relationship/topics/desires.md — check against existing desire.md in this dir and consolidate]

## Triggers
[Content from Relationship/topics/triggers.md]

## Milestones
[Content from Relationship/topics/milestones.md]
```

After creating this file, delete `Workboxes/Areas/Relationships/desire.md` only if its content is fully incorporated into roxy-profile.md.

- [ ] **Step 3: Create Areas/Relationships/Projects/commitments.md**

```bash
mkdir -p /Volumes/💩/ai/Workboxes/Areas/Relationships/Projects
```

Create the file:

```markdown
---
type: project
area: Relationships
status: active
updated: 2026-05-13
---

# Relationship Commitments

## Roxy's Active Requests
[Content from Relationship/topics/requests.md]

## Mike's Commitments
[Content from Relationship/topics/commitments.md]
```

- [ ] **Step 4: Copy open-questions.md to relationship-agent**

Check if `relationship-agent/vault/relationship/` already has an open-questions file:

```bash
ls /Volumes/💩/ai/relationship-agent/vault/relationship/
```

If no existing open-questions file, copy directly:

```bash
cp /Volumes/💩/ai/Relationship/topics/open-questions.md \
   /Volumes/💩/ai/relationship-agent/vault/relationship/open-questions.md
```

If a file already exists there, read both and manually merge — don't overwrite.

- [ ] **Step 5: Copy timeline/ and inbox/ to relationship-agent (if not already there)**

```bash
# Check what's in relationship-agent/vault/relationship/ first
ls /Volumes/💩/ai/relationship-agent/vault/relationship/

# Check Relationship/timeline/ content
find /Volumes/💩/ai/Relationship/timeline -type f | sort

# Copy only if no overlap
cp -rn /Volumes/💩/ai/Relationship/timeline/ \
       /Volumes/💩/ai/relationship-agent/vault/relationship/timeline-from-relationship-repo/
```

Use `-n` flag to never overwrite existing files.

- [ ] **Step 6: Verify**

```bash
grep "type: reference" /Volumes/💩/ai/Workboxes/Areas/Relationships/roxy-profile.md
ls /Volumes/💩/ai/Workboxes/Areas/Relationships/Projects/
grep "type: project" /Volumes/💩/ai/Workboxes/Areas/Relationships/Projects/commitments.md
```

- [ ] **Step 7: Commit both repos**

```bash
cd /Volumes/💩/ai/Workboxes
git add Areas/Relationships/
git commit -m "migrate Relationship/topics/: add roxy-profile.md and commitments project"

cd /Volumes/💩/ai/relationship-agent
git add vault/relationship/
git commit -m "merge Relationship/ content: open-questions, timeline into vault/relationship/"
```

---

## Task 8: Copy skills to Workboxes

**Files:**
- Create: `Workboxes/.claude/skills/morning/SKILL.md`
- Create: `Workboxes/.claude/skills/sync/SKILL.md`
- Create: `Workboxes/.claude/skills/adhd-coach/SKILL.md` + `references/`
- Create: `Workboxes/.claude/skills/vulnerability-coach/SKILL.md` + `references/`

Note: These skills may reference vault paths like `vault/adhd/` which exist only in relationship-agent, not Workboxes. The conversational functions (urge surf, reset, vulnerability work) operate without vault access. Vault-dependent features (checking commitments.md, weekly review against daily notes) require opening relationship-agent. This is acceptable per the design — for deep ADHD or vulnerability sessions, open relationship-agent.

- [ ] **Step 1: Create .claude/skills/ directory and copy morning + sync**

```bash
mkdir -p /Volumes/💩/ai/Workboxes/.claude/skills/morning
mkdir -p /Volumes/💩/ai/Workboxes/.claude/skills/sync

cp /Volumes/💩/ai/relationship-agent/.claude/skills/morning/SKILL.md \
   /Volumes/💩/ai/Workboxes/.claude/skills/morning/SKILL.md

cp /Volumes/💩/ai/relationship-agent/.claude/skills/sync/SKILL.md \
   /Volumes/💩/ai/Workboxes/.claude/skills/sync/SKILL.md
```

- [ ] **Step 2: Copy adhd-coach (SKILL.md + all references)**

```bash
cp -r /Volumes/💩/ai/relationship-agent/.claude/skills/adhd-coach \
      /Volumes/💩/ai/Workboxes/.claude/skills/adhd-coach
```

- [ ] **Step 3: Copy vulnerability-coach (SKILL.md + all references)**

```bash
cp -r /Volumes/💩/ai/relationship-agent/.claude/skills/vulnerability-coach \
      /Volumes/💩/ai/Workboxes/.claude/skills/vulnerability-coach
```

- [ ] **Step 4: Verify all four skills are present**

```bash
find /Volumes/💩/ai/Workboxes/.claude/skills -name "SKILL.md" | sort
```

Expected output (4 lines):
```
/Volumes/💩/ai/Workboxes/.claude/skills/adhd-coach/SKILL.md
/Volumes/💩/ai/Workboxes/.claude/skills/morning/SKILL.md
/Volumes/💩/ai/Workboxes/.claude/skills/sync/SKILL.md
/Volumes/💩/ai/Workboxes/.claude/skills/vulnerability-coach/SKILL.md
```

- [ ] **Step 5: Commit**

```bash
cd /Volumes/💩/ai/Workboxes
git add .claude/skills/
git commit -m "add skills: morning, sync, adhd-coach, vulnerability-coach from relationship-agent"
```

---

## Task 9: Retire repos

**Order matters:** delete Relationship2 first (trivial), then retire Memory and Relationship only after verifying content landed.

- [ ] **Step 1: Delete Relationship2 (empty, safe)**

```bash
rm -rf /Volumes/💩/ai/Relationship2
```

Verify:
```bash
ls /Volumes/💩/ai/
```

`Relationship2` should no longer appear.

- [ ] **Step 2: Verify Memory content landed in Workboxes**

Run these checks — all must pass before renaming:

```bash
# Identity doc
grep "Intentionality" /Volumes/💩/ai/Workboxes/CLAUDE.md

# TOP_OF_MIND exists and has items
grep "\[family\]" /Volumes/💩/ai/Workboxes/_system/TOP_OF_MIND.md

# Family contacts present
grep "Dr. Kennedy" /Volumes/💩/ai/Workboxes/Areas/Family/_index.md

# Oscar active thread present
grep "behavior contract" /Volumes/💩/ai/Workboxes/Areas/Family/Oscar.md

# Relationship reminders present
grep "Don't bring grief" /Volumes/💩/ai/Workboxes/Areas/Relationships/_index.md

# Deep background in relationship-agent
grep "bank statement" /Volumes/💩/ai/relationship-agent/vault/relationship/memory-background.md
```

All six must return matches. If any fail, go back and fix before proceeding.

- [ ] **Step 3: Retire Memory/**

```bash
mv /Volumes/💩/ai/Memory /Volumes/💩/ai/_retired-Memory
```

- [ ] **Step 4: Verify Relationship content landed**

```bash
# Roxy profile present
grep "type: reference" /Volumes/💩/ai/Workboxes/Areas/Relationships/roxy-profile.md

# Commitments project present
ls /Volumes/💩/ai/Workboxes/Areas/Relationships/Projects/commitments.md

# Open questions in relationship-agent
ls /Volumes/💩/ai/relationship-agent/vault/relationship/open-questions.md
```

All three must pass before proceeding.

- [ ] **Step 5: Retire Relationship/**

```bash
mv /Volumes/💩/ai/Relationship /Volumes/💩/ai/_retired-Relationship
```

- [ ] **Step 6: Final verification — confirm clean state**

```bash
ls /Volumes/💩/ai/
```

Expected: `Workboxes/`, `relationship-agent/`, `_retired-Memory/`, `_retired-Relationship/`, `.superpowers/`, `docs/` (if any)

NOT expected: `Memory/`, `Relationship/`, `Relationship2/`

- [ ] **Step 7: Commit Workboxes final state**

```bash
cd /Volumes/💩/ai/Workboxes
git add -A
git commit -m "consolidation complete: Memory and Relationship retired"
```

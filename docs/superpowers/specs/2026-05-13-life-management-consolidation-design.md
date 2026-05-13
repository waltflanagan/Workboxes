# Life Management Consolidation Design

**Date:** 2026-05-13  
**Status:** Approved  
**Author:** Mike Simons + Claude

---

## Problem

Five repos exist in `/Volumes/💩/ai/` that were built as separate attempts to manage different slices of life. They overlap heavily, create confusion about where to look, and the most-used one (Workboxes) carries unnecessary complexity. The relationship domain alone spans three repos simultaneously.

**Current repos:**
- `Memory/` — External brain (family, relationship, home, self workboxes)
- `Relationship/` — Marriage knowledge base (Roxy's preferences, commitments, triggers, etc.)
- `Relationship2/` — Empty placeholder, never populated
- `Workboxes/` — Life management vault with 8 areas, triage rules, slash commands
- `relationship-agent/` — Full "Life OS" with vault, Python scripts, Claude skills

---

## Decision

**Option B: Simplify + Consolidate**

Workboxes becomes the single daily life management hub. relationship-agent stays as a dedicated deep-work space for intentional sessions. Memory, Relationship, and Relationship2 are retired.

**Core principle:** You never need both open at once. Workboxes is for everything you touch on a normal day. relationship-agent is where you go deliberately — disclosure prep, ADHD trace-backs, vulnerability coaching.

---

## Architecture

### Workboxes/ — Daily Life Management Hub

**CLAUDE.md (rewritten)** — single authoritative identity doc, absorbing:
- Identity: Mike Simons, Staff iOS Engineer at Etsy, ADHD, born June 21 1985
- Family: Roxy (wife), Oscar (14), Nigel (10), Avery (7)
- Values hierarchy: Intentionality → Wholeness/Authenticity → Reflection & Growth → Family → Joy
- Communication style: direct + warm, short prose over bullets, challenge when appropriate (from voice.md)
- Tool stack: Things 3, Drafts, Day One, Craft, Bear, Google Suite, Apple-first
- Executive assistant rules: check TOP_OF_MIND first, surface what's dropping, default to action, pick ONE next thing when scattered
- Skills available: /morning, /sync, /adhd-coach, /vulnerability-coach

**_system/ (slimmed):**
- Keep: `README.md`, `workbox-schema.md`, `triage-rules.md`, `changelog.md`, `sync-state.json`, `schemas/`
- Add: `TOP_OF_MIND.md` (15-item capped live dashboard, populated from Memory's current 11 items)
- Absorb into CLAUDE.md: `voice.md`
- Move to Areas/Growth/: `growth-prompts.md`, `growth-surfacing.md`
- Drop: nothing else

**Slash commands (trimmed to 3):**
- Keep: `/triage`, `/review-daily`, `/review-weekly`
- Drop: `/review-quarterly`, `/growth-new`, `/school-email-triage`

**Areas/ (unchanged structure, new content):**
- `Areas/Family/` — Oscar crisis thread, behavior contract, Dr. Kennedy, Cedar Point; school contacts, therapist Jason; birthdays, annual calendar scaffold; deep background (Roxy's brother, band trip, Oscar's ADHD pattern)
- `Areas/Relationships/` — `roxy-profile.md` (preferences, triggers, desires, milestones) + `Projects/commitments.md` (Roxy's active requests, Mike's commitments); daily behavioral reminders (don't bring grief to Roxy, action > conversation)
- `Areas/Work/`, `Health/`, `Home/`, `Finances/`, `Growth/`, `Personal/` — existing structure kept as-is

**Skills (4 everyday tools):**
- `/morning` — daily briefing
- `/sync` — external app sync
- `/adhd-coach` — reset, urge surf, trace-backward, weekly review
- `/vulnerability-coach` — vulnerability work

---

### relationship-agent/ — Deep Work Space (unchanged)

Open intentionally, not as daily ops. Owns:

**Deep knowledge:**
- `vault/relationship/discernment/` — full question set, answers, rubrics (the core knowledge base)
- `vault/relationship/` — conversation logs + schema, deep background (bank statement moment, Roxy's nervous system history, Mike's ADHD emotional avoidance pattern)
- `vault/adhd/` — foundations, commitments, weekly review structure
- `vault/learning/` — podcast pipeline, summaries

**Scripts:**
- `scripts/self/daily.py` — daily note creation + Day One / Drafts / Things sync
- `scripts/learning/` — RSS fetch, AI summarize, intake pipeline

**Skills (deep-work, stays here only):**
- `/discernment-coach` — all five modes
- `/feeling-analysis` — conversation/text analysis
- `/adhd-coach` — also available here for deep-work ADHD sessions
- `/vulnerability-coach` — also available here
- `/morning` — also available here
- `/sync` — also available here

---

## Content Migration Map

### From Memory/ → Workboxes/

| Source | Content | Destination |
|---|---|---|
| `Memory/CLAUDE.md` | Identity, values, comms style, tool stack, exec assistant rules | `Workboxes/CLAUDE.md` (rewritten to incorporate) |
| `Memory/TOP_OF_MIND.md` | 11 live items (family + relationship + self) | `Workboxes/_system/TOP_OF_MIND.md` |
| `Memory/family/` Oscar thread | Crisis, behavior contract, Dr. Kennedy, Cedar Point | `Areas/Family/Projects/` |
| `Memory/family/` contacts + calendar | Schools, therapist, birthdays, annual scaffold | `Areas/Family/_index.md` |
| `Memory/family/` deep background | Roxy's brother, band trip, Oscar's ADHD pattern | `Areas/Family/_index.md` (Background section) |
| `Memory/relationship/MEMORY.md` | Daily behavioral reminders + open action items | `Areas/Relationships/_index.md` |
| `Memory/relationship/` deep background | Bank statement moment, nervous system history, conversation logs | `relationship-agent/vault/relationship/` |
| `Memory/home/` + `Memory/self/` | Both empty scaffolds | Drop |
| `Memory/workboxes/SCHEMA.md` + `validate.swift` | Workboxes has its own schema | Drop |

### From Relationship/ → Workboxes/Areas/Relationships/

| Source | Content | Destination |
|---|---|---|
| `topics/preferences.md`, `topics/desires.md`, `topics/triggers.md`, `topics/milestones.md` | Roxy's profile — who she is, what she needs | `Areas/Relationships/roxy-profile.md` |
| `topics/requests.md`, `topics/commitments.md` | What Roxy asked for + Mike's commitments | `Areas/Relationships/Projects/commitments.md` |
| `topics/open-questions.md` | Discernment/relationship open questions | `relationship-agent/vault/relationship/` |
| `timeline/` + `inbox/` | Conversation logs + raw intake | `relationship-agent/vault/relationship/` |
| `skills/feeling-analysis/` | Already in relationship-agent | No action |

### Repos to Retire

- `Memory/` — rename to `_retired-Memory/` after confirming content landed correctly in Workboxes
- `Relationship/` — rename to `_retired-Relationship/` after confirming content landed correctly in Workboxes + relationship-agent
- `Relationship2/` — delete immediately (empty, no content to verify)

---

## What Doesn't Change

- `relationship-agent/` structure and scripts — no reorganization; deep background content from Memory and Relationship is merged into existing `vault/relationship/` directories, not new ones
- `Workboxes/Areas/` structure — 8 areas stay, only content is added
- Workboxes schema and triage rules — kept as-is
- The two-repo split: daily ops (Workboxes) vs. intentional deep work (relationship-agent)

---

## Success Criteria

- Opening Workboxes in Claude Code gives full context about Mike's identity, values, active family threads, and relationship behavioral reminders without reading anything else
- `TOP_OF_MIND.md` is the first thing Claude checks — 15-item capped, always current
- Memory, Relationship, Relationship2 are archived/deleted
- No duplication: relationship knowledge lives in exactly one place depending on type (daily ops vs. deep background)
- Workboxes CLAUDE.md is the single identity document — no other file defines "who Mike is"

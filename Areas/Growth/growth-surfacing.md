# Growth Surfacing Rules

Rules that `/triage` uses to suggest Growth links and dispositions.
Surfacing is **suggestive only** — triage still routes items to their
best-fit workbox; Growth links are added to the item body or the
routing rationale, not used as an alternate destination (except when
the disposition is explicitly Route-to-Growth).

## When to apply

During `/triage` step 1 (load context), read this file. For each inbox
item during step 3 (propose a disposition), run the scan patterns
below before settling on a proposal.

## Scan patterns (Day 1: simple slug/alias matching)

**1. Value match**

For each file in `Areas/Growth/Values/*.md`:
- Read the file's frontmatter (specifically `aliases`) and its filename
  slug.
- If the inbox item body contains the slug or any alias as a word, the
  match fires.

On match: suggest `[[<value-slug>]]` be added to the item body. The
item still routes normally via its best-fit disposition.

**2. Goal match**

For each file in `Areas/Growth/Goals/*.md`:
- Read the filename slug.
- If the inbox item body contains the slug or any prominent word from
  the goal's `# <title>` heading, the match fires.

On match: consider the **append** disposition if the item reads like a
short progress note (≤ 3 sentences, no distinct new topic). Otherwise
suggest `[[<goal-slug>]]` in the item body while routing elsewhere.

**3. Practice match**

Same as goal match, against `Areas/Growth/Practices/*.md`. Short "I
did this today" notes are strong append candidates.

**4. Therapy-homework match**

If the item body contains any of: "my therapist", "my therapist said",
"between sessions", "homework" (case-insensitive), the match fires.

On match: suggest **append** to `Areas/Growth/Therapy/homework.md`
under `## Active`, or suggest routing to a therapy session file if
the item is clearly tied to a specific session (has a session date).

**5. Theme match**

For each folder in `Areas/Growth/Themes/*/`:
- Read the folder slug.
- If the inbox item body contains the slug, the match fires.

On match: propose Route-to-Growth with destination
`Areas/Growth/Themes/<slug>/` as a new reflection file. If no theme
slug matches but the item reads reflective/introspective, propose
creating a new theme (requires a proposed slug and approval).

## Match priority

If multiple scans fire, present all matches in the rationale. The user
picks which link(s) to keep. Theme route proposals take precedence
over value/goal/practice link suggestions — a reflective item
belongs under Themes, not linked from a value.

## Deferred (Phase 6b)

- Emotion/feeling lexicons to match themes without a slug match.
- Multi-word phrase matching ("I want to run a marathon" → goal
  `run-half-marathon.md`).
- Fuzzy matching on slugs (`meditation` matching
  `morning-meditation`).

If Day-1 matching feels thin after real use, propose a rule addition
via the normal propose-approve-execute flow and append to this file.

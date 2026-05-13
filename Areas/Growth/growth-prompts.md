# Growth Review Prompts

Prompts and load rules for the `/review-*` slash commands. Each command
reads its own section here.

Reviews are **always written** — if the user has nothing to say, the
review file is still created with a short body (e.g. "nothing came up
today"). Skipped-review files become evidence later.

---

## Daily

**Purpose:** quick morning check-in. Surfaces today's one-pagers.

**What to load (before asking):**

- `Areas/Growth/Intentions/` — the intention file whose `period`
  contains today's date, if one exists.
- `Areas/Growth/Practices/*.md` — randomly pick one active practice
  to highlight.
- `Areas/Growth/Goals/*.md` — randomly pick one active goal to
  highlight.

**Prompts (ask one at a time):**

1. "Today's intention for this period is: `<quote the intention's focus
   points>`. Anything you want to hold in mind today?"
2. "One active practice to remember: `<practice name>`. Doing it today?
   Anything to note?"
3. "One active goal to remember: `<goal name>`. Any move you can make
   today?"

**Writes:** `Areas/Growth/Reviews/YYYY-MM-DD-daily.md` with the user's
answers.

---

## Weekly

**Purpose:** Sunday (or whichever day the user prefers) pass over the
week. Surfaces intentions, active goals, and therapy homework.

**What to load:**

- Current-period intention file.
- All active goals (`frontmatter.status == active`).
- For each active goal, tail of `## Activity` section (last ~10 lines
  if present).
- `Areas/Growth/Therapy/homework.md` — the full `## Active` section.
- `Areas/Growth/Reviews/` — list of daily reviews from the past seven
  days (filenames only).

**Prompts:**

1. "This week's intention: `<quote>`. How did the week feel against it?"
2. "Goals touched this week: `<list active goals with recent activity>`.
   Any to mark achieved / abandoned / paused? Anything to add to
   `## Activity` for any of them?"
3. "Active therapy homework: `<list from homework.md>`. Any to move to
   Completed? Anything new to add?"
4. "Anything from this week's daily reviews that wants a longer
   reflection or a new theme?"

**Writes:** `Areas/Growth/Reviews/YYYY-MM-DD-weekly.md`.

Any proposed status transitions (e.g. goal → achieved) or new content
(e.g. new reflection under a theme) run through the normal
propose-approve-execute pattern as separate steps after the review
body is written.

---

## Quarterly

**Purpose:** end-of-quarter reset. Archive current intention, draft
next, check in on values.

**What to load:**

- Current-period intention (by date — the one whose `period` contains
  today or ends today).
- All values (`Values/*.md`), status `active` or `archived`.
- All active goals, with full `## Activity` sections.
- All active themes, with reflection count per theme.

**Prompts:**

1. "Closing intention: `<quote current intention>`. Mark `status:
   completed` and archive? Any final notes for the body before we
   close it?"
2. "Drafting next quarter's intention. Slug will be `<next-period>`.
   1–5 focus points?"
3. "Values check-in: for each active value, does it still feel true?
   Any to archive? Any new values emerging that deserve their own
   file?"
4. "Goals retrospective: `<list active goals>`. Any to mark achieved,
   abandoned, or paused going into next quarter?"
5. "Themes landscape: `<list active themes with reflection counts>`.
   Any to mark dormant or resolved?"

**Writes:** `Areas/Growth/Reviews/YYYY-MM-DD-quarterly.md`.

Status transitions and the new intention file are created after the
review body, via approve-then-execute.

---

## Monthly

<!-- deferred to Phase 6b -->

---

## Annual

<!-- deferred to Phase 6b -->

# Schema: Resources Wiki

Cross-area reference content lives under `Resources/` at the vault root —
never inside an `Areas/` subfolder.

## Folder layout

```
Resources/
├── _index.md           (wiki root MOC)
└── People/
    ├── _index.md       (people directory)
    └── <firstname-lastname>.md
```

## Frontmatter — person

```yaml
---
type: person
name: Full Name
role: therapist | doctor | teacher | coach | vendor | other
org:                    # practice, school, company
phone:
email:
address:
insurance: in-network | OON | self-pay | N/A | unknown
areas: []               # vault areas this person is relevant to, e.g. [Family, Health]
tags: []
created: 2026-05-08
---
```

## Person file body structure

```markdown
## Contact

| Field | Value |
|---|---|
| Phone | |
| Email | |
| Address | |
| Insurance | |

## Background

(Specialty, approach, notes on fit, referral source.)

## Interactions

Reverse-chronological. One bullet per contact.

- YYYY-MM-DD — <what happened>

## Links

Vault notes that reference this person.

- [[Areas/...]]
```

## Rules for People entries

- Filename: `<firstname-lastname>.md` in kebab-case.
  Unknown last name: `<firstname-descriptor>.md`.
- Always search `Resources/People/` before creating — no duplicates.
- Update `## Interactions` on every new contact, even a brief one.
- After adding a new person, update `Resources/People/_index.md` under
  `## People` (alphabetical by last name) and `## Recently updated`.
- Link *to* Resources from area notes; never move a person entry into an area.

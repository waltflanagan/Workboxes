# Schema: School Email

Created by `/school-email-triage` and filed directly into a child project folder.
Never lands in `Inbox/`.

## Frontmatter

```yaml
---
type: school-email
created: 2026-05-07T14:30:00-04:00    # ISO 8601, time email was processed
source: gmail
source_id: <gmail message id>
source_url: <gmail permalink>
child: oscar | nigel | avery | all
email_type: newsletter | school-info | district-info
sender: <from address>
subject: <email subject line>
has_events: true | false
has_actions: true | false
tags: []
---
```

## Body structure

```markdown
## Events

- YYYY-MM-DD — <event description> [→ calendar]

## Action items

- <task description> — due YYYY-MM-DD [→ Things]

## Full email

<full email body, lightly formatted>
```

If no events were found, omit `## Events`. If no action items, omit
`## Action items`. Always include `## Full email`.

# Schema: Inbox Item

## Frontmatter

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

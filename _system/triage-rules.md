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

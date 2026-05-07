# School Email Senders

Configuration for `/school-email-triage`. Maps Gmail sender addresses (or
subject-line patterns) to a child and an email type.

**Fill in your actual sender addresses before running the command.**
The `gmail_query` field is passed directly to Gmail search, so you can use
any Gmail search syntax (`from:`, `subject:`, `list:`, etc.).

---

## Oscar

```yaml
child: oscar
senders:
  - label: Oscar's school newsletter
    gmail_query: "from:newsletter@oscars-school.example.com"
    email_type: newsletter

  - label: Oscar's school general info
    gmail_query: "from:office@oscars-school.example.com"
    email_type: school-info
```

---

## Nigel

```yaml
child: nigel
senders:
  - label: Nigel's school newsletter
    gmail_query: "from:newsletter@nigels-school.example.com"
    email_type: newsletter

  - label: Nigel's school general info
    gmail_query: "from:office@nigels-school.example.com"
    email_type: school-info
```

---

## Avery

```yaml
child: avery
senders:
  - label: Avery's school newsletter
    gmail_query: "from:newsletter@averys-school.example.com"
    email_type: newsletter

  - label: Avery's school general info
    gmail_query: "from:office@averys-school.example.com"
    email_type: school-info
```

---

## District-wide (all kids)

```yaml
child: all
senders:
  - label: School district newsletter
    gmail_query: "from:communications@district.example.com"
    email_type: district-info
```

---

## Notes

- `email_type` must be one of: `newsletter`, `school-info`, `district-info`.
- `child: all` routes the note to each child's project folder.
- Gmail queries are evaluated against unread threads newer than the
  high-water mark stored in `_system/sync-state.json`.
- After updating this file, run `/school-email-triage` — it will pick
  up any threads matching the new queries from the last 30 days if no
  high-water mark exists yet.

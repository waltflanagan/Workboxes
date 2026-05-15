# Capture Sources

Active sources that feed `Inbox/`. All use the `inbox-item` frontmatter shape from `schema.md`.

---

## Raw text drop

Drop any `.md` file into `Inbox/`. No setup required.

Provenance: `source: raw`, `captured: <file mtime>` if frontmatter is missing.

---

## Obsidian Web Clipper

Saves web pages and selections directly into `Inbox/`.

Provenance fields: `source: web-clipper`, `source_url: <url>`, `captured: <datetime>`.

---

## Drafts

A Drafts action writes the current draft to `Inbox/` with frontmatter.

Provenance: `source: drafts`, `source_id: <draft_uuid>`, `tags: <draft tags>`.

---

## Gmail — school email pipeline

Pull source for school and district newsletters. Operated by `/school-email-triage`. Notes are filed directly into child folders (`Areas/Family/<Child>/`) — they never land in `Inbox/`.

Provenance: `source: gmail`, `source_id: <message id>`, `source_url: <permalink>`, `child: oscar|nigel|avery|all`, `email_type: newsletter|school-info|district-info`.

High-water marks stored in `.agents/sync-state.json` under `gmail_<child>_<label-slug>` keys.

---

## Adding a new source

1. Update this file with provenance fields.
2. If it's a pull source, add a top-level key to `.agents/sync-state.json`.
3. Append an entry to `.agents/docs/changelog.md`.

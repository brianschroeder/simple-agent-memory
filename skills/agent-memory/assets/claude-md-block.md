<!-- agent-memory:start -->
## Agent memory

<!-- simple-agent-memory:instructions=3 -->
Shared repo learnings live in `./agent-memory/`, an
[Open Knowledge Format](https://github.com/GoogleCloudPlatform/open-knowledge-format/blob/main/SPEC.md)
(OKF) v0.2 bundle: one Markdown concept file per learning, plus the reserved
`index.md` and `log.md`. Read it on demand; do not import it automatically.

Before non-trivial work and whenever stuck, read `agent-memory/index.md`, then
search the bundle for the symptom, error text, and tags. Open matching concepts
and follow their links before investigating from scratch. `status: draft` is
unconfirmed, `deprecated` is history, a concept without `verified` is
unverified, and one past its `stale_after` is a lead to re-check, not a fact.

Record durable root causes, gotchas, conventions, and useful dead ends while
fresh. Search first, and update a matching concept instead of duplicating it.
Write each new learning to `agent-memory/<slug>.md`: the title without status
markers, lowercased, with every run of characters other than `a-z` and `0-9`
replaced by one hyphen and no hyphen at either end (`v3.2` becomes `v3-2`). Use
a name no other concept uses, and keep it stable because links use it.

```markdown
---
type: Gotcha                # or Root Cause, Convention, Dead End, Guide
title: "<the symptom a stuck agent would search for>"
description: "<one sentence>"
tags: [<few>, <lowercase>, <tags>]
status: stable              # draft if unconfirmed; deprecated once superseded
discovered: YYYY-MM-DD      # original discovery date; omit if unknown
generated:
  by: <tool>/<model>        # or human:<handle> for a person
  at: <YYYY-MM-DDTHH:MM:SSZ>
---

# Symptom
# What to know
# Why it matters
# Related
```

- Add `sources` (entries with `id`, `resource`, `title`, cited in the body as
  `[^id]`) for real evidence, a `verified` entry (`by`, `at`) only after
  actually re-checking the claim, and `stale_after` when the finding depends on
  something that changes. Use block-style YAML, not `{ }` flow mappings. Omit
  what you do not know. Never invent dates, evidence, or verification.
- Update `generated` when you change a concept's meaning. Preserve frontmatter
  keys you do not recognize. Link concepts with relative paths (`other.md`).
- List each concept in `index.md` under its type's plural heading (`Root Cause`
  under `# Root causes`; add a heading for a new type), new concepts at the top,
  as `* [Title](slug.md) - description`. Record creations, supersessions, renames,
  and migrations in `log.md` under a `## YYYY-MM-DD` heading, newest first.
- To supersede, set `status: deprecated`, link the replacement, move its index
  line under `# Deprecated`, and keep the file and its reasoning.
- Never record secrets, credentials, emails, or transient task state, or repeat
  what code and docs already cover; link to those instead.

### First-run setup and migration

Before writing memory, inspect `agent-memory/`, a legacy `agent-memory.md`,
filename case variants, and any custom memory path these instructions name
(any path other than `./agent-memory.md`). Honor a custom path; if its bundle
location is unclear, stop and ask. Do not repurpose an `agent-memory/`
directory that another tool owns.

- No memory yet: create `agent-memory/index.md` whose only frontmatter is
  `okf_version: "0.2"`, with one heading per type plus `# Deprecated`, and
  `log.md` (no frontmatter) with an Initialization entry. Create no placeholder
  concepts.
- Before converting a legacy file, preserve its exact current bytes: a commit
  that contains them (offer to commit uncommitted edits first), or a backup
  outside the repo whose location you report. Never commit backups or local
  paths.
- Create the bundle as above if it is missing, then convert each legacy entry
  into one concept, listed in the legacy log's order. Its heading, minus status
  markers such as `SUPERSEDED:` or `(unconfirmed)`, becomes `title`. The rest
  of the entry is the body, verbatim and not restructured; you may add a
  `# Related` section. Choose `type`, `description`, and `tags` from the
  entry's content. Take dates and status only from what it states:
  `Discovered YYYY-MM-DD` sets `discovered`, `SUPERSEDED:` sets `deprecated`,
  stated uncertainty sets `draft`, and anything else is `stable`. Reformatting
  is neither authorship nor verification, so omit `generated` and `verified`
  unless the entry names its author or checks. Keep an entry whole when its
  boundaries are unclear.
- Skip the template's example entry and any section that is unchanged template
  text. Keep every other non-entry section as a `type: Guide` concept; when
  unsure, keep it.
- When memory exists in more than one place, skip an entry only if its content
  is already present verbatim. Keep differing claims and flag contradictions.
  If a safe merge is unclear, leave every file intact and report.
- Before removing the legacy file, verify: every entry and custom section maps
  to a concept, every non-reserved `.md` in the bundle has frontmatter with a
  non-empty `type`, `index.md` lists every concept, and relative links resolve.
  Repair known repo references to the old file. Add a Migration entry to
  `log.md` naming the source file and the commit that holds its exact original,
  if one does. Then remove the legacy file.
- On a repeated or interrupted run, inspect the actual files and resume without
  duplicate concepts or index lines. Reconcile a legacy file that reappears the
  same way.
<!-- agent-memory:end -->

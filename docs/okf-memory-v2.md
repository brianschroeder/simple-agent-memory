# OKF memory bundle v2 proposal

Status: proposed; merge requires Brian's confirmation of this plan and spec.

## Decision

Replace the default `agent-memory.md` log with an `agent-memory/` directory at
the repository root that is a conformant
[Open Knowledge Format v0.2](https://github.com/GoogleCloudPlatform/open-knowledge-format/blob/main/SPEC.md)
bundle. Each learning is one concept file with YAML frontmatter. The reserved
`index.md` lists every concept and declares `okf_version: "0.2"`; the reserved
`log.md` records bundle history. No runtime service, database, or required
dependency.

The [instruction block](../skills/agent-memory/assets/claude-md-block.md)
defines the concept format, reading, writing, and first-run migration. The
[index](../skills/agent-memory/assets/index.template.md) and
[log](../skills/agent-memory/assets/log.template.md) templates seed a new
bundle. The [setup skill](../skills/agent-memory/SKILL.md) upgrades existing
managed blocks rather than skipping them.

An earlier draft of this proposal kept one `MEMORY.md` file and described it as
OKF-inspired. It never shipped. Adopting OKF itself instead gives:

- Interoperability. Any OKF consumer, including Google's reference graph
  viewer, can read the bundle without a converter.
- Trust and lifecycle fields with defined meaning (`status`, `generated`,
  `verified`, `stale_after`, `sources`) in place of locally invented ones.
- Progressive disclosure. Agents read a short index, then open only the
  matching concepts, so reading cost stays flat as memory grows.
- Smaller diffs and fewer merge conflicts, since parallel sessions usually add
  different files instead of editing the top of one log.

The cost is more files, an index that must stay in step with the concepts, and
frontmatter that must stay valid YAML. The setup skill checks all three.

## Bundle layout

```text
agent-memory/
  index.md       # reserved: okf_version only in frontmatter; one line per concept
  log.md         # reserved: date-grouped history, newest first
  <slug>.md      # one concept per learning, kebab-case, stable once created
```

Concepts start flat at the bundle root, grouped by type in `index.md`. Topic
subdirectories (each with its own `index.md`) are allowed later if the index
gets hard to scan; moving a concept means repairing links to it.

## Concept format

| Field | OKF status | Use here |
|---|---|---|
| `type` | required | `Gotcha`, `Root Cause`, `Convention`, `Dead End`, or `Guide`; new descriptive types are fine |
| `title` | recommended | The symptom a stuck agent would search for |
| `description` | recommended | One sentence, repeated on the concept's `index.md` line |
| `tags` | recommended | A few lowercase tags |
| `status` | lifecycle | `stable` (confirmed), `draft` (hypothesis), `deprecated` (superseded); absent means `stable` |
| `generated` | trust | `{ by, at }`: who last wrote or meaningfully changed the content, as an OKF actor |
| `verified` | trust | `{ by, at }` entries added only after actually re-checking the claim |
| `sources` | provenance | `{ id, resource, title }` evidence, cited from the body with `[^id]` footnotes |
| `stale_after` | lifecycle | Set when the finding depends on something expected to change |
| `discovered` | extension | Original discovery date, `YYYY-MM-DD`; unchanged by later edits |

Body sections follow OKF's preference for structural Markdown: `# Symptom`,
`# What to know`, `# Why it matters`, and `# Related` where useful. None are
required.

### Worked example

`agent-memory/pytest-import-errors-in-ci.md`:

```markdown
---
type: Gotcha
title: "pytest passes locally but fails in CI with import errors"
description: "CI runs pytest from the repo root with PYTHONPATH unset; reproduce from the root."
tags: [testing, ci, python]
status: stable
discovered: 2026-03-14
generated:
  by: claude-code/claude-opus-5-5
  at: 2026-03-14T18:02:00Z
verified:
  - by: human:octocat
    at: 2026-03-15T09:30:00Z
sources:
  - id: ci-config
    resource: .github/workflows/test.yml
    title: Test workflow
---

# Symptom

Tests are green locally but red in CI, which cannot find a first-party module.

# What to know

CI runs from the repository root with `PYTHONPATH` unset.[^ci-config] Run
`pytest` from the root, not from `tests/`, to reproduce.

# Why it matters

Prevents debugging test code when the working directory is the cause.

[^ci-config]: Test workflow
```

Its line in `index.md`, under `# Gotchas`:

```markdown
* [pytest passes locally but fails in CI with import errors](pytest-import-errors-in-ci.md) - CI runs pytest from the repo root with PYTHONPATH unset; reproduce from the root.
```

Its line in `log.md`:

```markdown
## 2026-03-14
* **Creation**: [pytest passes locally but fails in CI with import errors](pytest-import-errors-in-ci.md)
```

### Choices within the spec

- **Relative links.** OKF recommends bundle-rooted links (`/x.md`), but the
  bundle lives in a subdirectory of a code repository, where Git hosts resolve
  `/x.md` from the repository root. Relative links (`x.md`, `../x.md`) are
  also valid OKF and work in both places.
- **Block-style YAML.** The OKF spec writes `generated` and `verified` as flow
  mappings such as `{ by: human:x, at: 2026-06-30T14:00:00Z }`. That is valid
  YAML 1.2, and current PyYAML accepts it, but older parsers reject the colons
  inside the unquoted values. libyaml 0.2.1, used by the Ruby that ships with
  macOS, fails on the spec's own examples and on Google's sample bundle.
  Block-style mappings carry the same data and parse everywhere.
- **`discovered` is an extension key.** OKF's `generated.at` is the last
  meaningful change, not the discovery date, and OKF timestamps are full
  datetimes. Legacy entries only carry a calendar date, so the original date
  goes in `discovered` rather than being padded into an invented time.
- **Migrated entries have no `generated` or `verified`.** The legacy log did not
  record authors or checks. Attributing imported text to the migrating agent
  would misstate provenance, and OKF treats a missing trust family as
  "unverified" rather than as an error.
- **The index is maintained by the writing agent,** not generated by a tool.
  OKF allows either. The setup skill checks that it lists every concept.
- **Unused OKF features.** Attested Computation concepts, `usage_count`, and
  `resource` bindings have no role in repo memory today. Agents preserve any
  frontmatter keys they do not recognize.

## Upgrade and compatibility

Ship plugin version 0.2.0 in both the standalone and bundled copies. Keep their
skill and assets identical. Keep each repository's installation instructions.
The marketplace's companion references name `agent-memory/` too.

A plugin update does not edit files in consuming repos. Existing users invoke
the updated setup skill once or install the new block manually. The block
contains all migration instructions so the next agent can migrate without an
installed plugin. Both existing instruction files get consistent blocks. The
block's marker is `instructions=3`; `2` was the unreleased single-file draft.

Migration is agent-guided, not an executable converter. Each legacy entry
becomes one concept: its heading (minus status markers) becomes the title, the
rest stays verbatim as the body, and dates and status come only from what the
entry states (`Discovered` date, `SUPERSEDED:`, stated uncertainty). The agent
classifies `type`, `description`, and `tags` from the content. Customized
non-entry sections become `Guide` concepts; unchanged template text and the
template's example entry are not carried over. The skill ships the v1 log
template and block under `assets/legacy/` so the agent can tell stock text
from customizations; when unsure, it keeps the section. The original bytes are recoverable from a commit or an
outside-the-repo backup before the legacy file is removed, and coverage and
conformance are checked first. Known repo references to `agent-memory.md`,
including the agent-references and agent-methods blocks, are repaired.

## Acceptance scenarios

| Initial state | Required outcome |
|---|---|
| No memory or instruction files | `agent-memory/index.md` (with `okf_version`) and `log.md`; one CLAUDE.md block; no placeholder concepts |
| Only AGENTS.md exists | Install there without unnecessarily creating CLAUDE.md |
| Both instruction files exist | Update both; preserve surrounding content; edit aliased files once |
| Legacy log with real entries | One concept per entry with verbatim text; supported frontmatter only; example skipped; legacy retired only after verification |
| Legacy entry marked `SUPERSEDED:` | `status: deprecated`, linked to its replacement, listed under `# Deprecated` |
| Customized non-entry section in the legacy log | Preserved as a `Guide` concept |
| Legacy file has uncommitted edits | Recovery must include those edits, not merely HEAD |
| `agent-memory/` exists but belongs to another tool | Leave intact; explain the collision instead of writing into it |
| Memory in both the legacy log and the bundle | Skip only verbatim duplicates; retain differing claims and flag contradictions |
| Repeated setup or interrupted migration | No duplicate blocks, concepts, or index lines; inspect actual state before resuming |
| Completed bundle plus a newly changed legacy file | Reconcile the legacy content instead of assuming migration is complete |
| Unknown dates, authors, or evidence | Omit `discovered`, `generated`, `sources`; never invent them |
| Two entries produce the same slug | Disambiguate the filename; never overwrite a concept |
| Case-only filename variant | Resolve deliberately without clobbering content |
| Custom path or customized managed block | Preserve compatible customization; ask when the bundle location is unclear |
| Malformed or duplicate block markers | Stop the ambiguous block edit without altering surrounding text |
| Existing links to legacy headings | Repair known references to point at the new concept files |

## Validation and limitations

Run manifest and skill validation, existing CI checks, local documentation-link
checks, and compare the two copies' skill and assets before merge. Check a
migrated bundle for OKF conformance (section 11 of the spec): every
non-reserved `.md` has parseable frontmatter with a non-empty `type`, and the
reserved files follow their structure. Review the scenarios above against the
instructions. These checks establish packaging and spec consistency; they do
not prove autonomous migrations against arbitrary user files. A representative
downstream migration should be reviewed before release.

No installed user repository is migrated by this pull request. No merge or
release is authorized until Brian reviews and confirms the proposal.

## Proposal validation record

The existing GitHub Actions shell checks, plus a new check on the OKF bundle
templates, pass locally in both repositories. `git diff --check` passes, and
the standalone and bundled skill, assets, and spec are byte-identical. The
Claude CLI validates both plugin copies; the standalone copy shows the same
warnings as before this change (missing marketplace description, no icon, no
README install line in the CLI's new form).

Two migrations were run end to end by fresh agents that saw only the skill,
against a fixture repository: a customized v1 log with five real entries, one
`SUPERSEDED:` entry, one unconfirmed entry, the template's example entry, a
custom "Read this first" section, an uncommitted entry, separate `CLAUDE.md`
and `AGENTS.md` blocks, and a README link to a legacy heading. Both runs
produced six concepts that pass an independent OKF v0.2 conformance check. Both
preserved entry text verbatim, set only stated dates and statuses, added no
`generated` or `verified`, skipped the example, kept the custom section as a
Guide, backed up the exact uncommitted bytes outside the repository, left text
outside the managed blocks byte-identical, and repaired the README link. The
instructions were tightened after each run where an agent had to guess (Guide
heading, slug rule, stock-text comparison, bundle creation during migration).

The flow-style YAML finding was reproduced with Ruby 2.6.10 / libyaml 0.2.1.
Google's sample bundles otherwise pass the same conformance check. Migrations
of real downstream repositories have not been performed.

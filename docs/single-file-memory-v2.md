# Single-file memory v2 proposal

Status: proposed; merge requires Brian's confirmation of this plan and spec.

## Decision

Replace the default `agent-memory.md` with uppercase `MEMORY.md`. Keep all
memories in that one file. Use consistent Markdown entry fields and links
between headings. No memory directory, per-entry files, graph database, runtime
service, or required dependencies.

The entry contract is defined in
[the template](../skills/agent-memory/assets/MEMORY.template.md). The installed
[first-run instructions](../skills/agent-memory/assets/claude-md-block.md) define
migration and ongoing use. The [setup skill](../skills/agent-memory/SKILL.md)
upgrades existing managed blocks rather than skipping them.

## Relationship to Open Knowledge Format

[OKF v0.2](https://github.com/GoogleCloudPlatform/open-knowledge-format/blob/main/SPEC.md)
uses a Markdown document per concept, YAML frontmatter for fields including
`type` and `tags`, and links between documents. Our entries are sections in one
file with plain Markdown fields. We borrow consistent metadata and relationships
without claiming per-entry OKF interoperability or using a misleading
`okf_version` declaration. Tags improve explicit searching; heading links help
navigation. Retrieval quality remains dependent on how the agent searches and
curates entries, and is not a measured performance claim.

## Upgrade and compatibility

Ship plugin version 0.2.0 in both the standalone and bundled copies. Keep their
skill and assets identical. Keep each repository's installation instructions.
The marketplace's companion references should name `MEMORY.md` too.

A plugin update does not edit files in consuming repos. Existing users invoke
the updated setup skill once or install the new block manually. The block
contains all migration instructions so the next agent can migrate without an
installed plugin. Both existing instruction files get consistent blocks.

Migration is agent-guided, not an executable converter. It preserves original
content and recovery evidence before removing the old file. Existing custom
paths and unrelated MEMORY.md files are protected. Missing metadata stays
unknown, conflicts remain explicit, and format completion is recorded only
after coverage checks. Backups are recovery artifacts outside the active store.

## Acceptance scenarios

| Initial state | Required outcome |
|---|---|
| No memory or instruction files | One MEMORY.md and one CLAUDE.md block; no fictitious entries |
| Only AGENTS.md exists | Install there without unnecessarily creating CLAUDE.md |
| Both instruction files exist | Update both; preserve surrounding content; edit aliased files once |
| Legacy log with real entries | Preserve exact source, every entry and custom section; add metadata; retire legacy only after verification |
| Legacy file has uncommitted edits | Recovery must include those edits, not merely HEAD |
| Existing MEMORY.md belongs to another system | Leave intact; explain collision instead of overwriting |
| Both memory files have distinct content | Preserve both, merge only when safe, retain differing claims |
| Repeated setup or interrupted migration | No duplicate blocks or entries; inspect actual state before resuming |
| Completed v2 plus newly changed legacy file | Reconcile legacy content instead of trusting the marker alone |
| Unknown dates or evidence | Keep unknown; never invent discovery dates or validation |
| Case-only filename variant | Resolve deliberately without clobbering content |
| Custom path or customized managed block | Preserve compatible customization; report conflicts |
| Malformed or duplicate block markers | Stop the ambiguous block edit without altering surrounding text |
| Existing links to legacy headings | Preserve headings where possible and repair known path/anchor references |

## Validation and limitations

Run manifest and skill validation, existing CI checks, local documentation-link
checks, and compare the two copies' skill/assets before merge. Review the above
scenarios against the instructions. These checks establish packaging and spec
consistency; they do not prove autonomous migrations against arbitrary user
files. A representative downstream migration should be reviewed before release.

No installed user repository is migrated by this pull request. No merge or
release is authorized until Brian reviews and confirms the proposal.

## Proposal validation record

The existing GitHub Actions shell checks passed locally in both repositories.
Skill validation, Markdown file-link checks, and `git diff --check` passed.
The standalone and bundled skill/assets are byte-identical. The Claude CLI
validates the memory plugin; the standalone marketplace passes with its existing
missing-description warning.

The bundled marketplace's overall CLI validation rejects all four bare plugin
source paths. The identical four failures reproduce against unchanged main.
This pre-existing issue is outside the memory migration change and remains
visible for review; the repository's own source-path checks pass.

Acceptance scenarios were reviewed against the instructions. No end-to-end
agent migration or installed-user migration has been performed.

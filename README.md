# simple-agent-memory

Durable repo learnings in an **`agent-memory/`** directory, shared across agent
sessions and reviewed in Git. The directory is an
[Open Knowledge Format (OKF) v0.2](https://github.com/GoogleCloudPlatform/open-knowledge-format/blob/main/SPEC.md)
bundle: one Markdown file per learning, with YAML frontmatter for its type,
tags, dates, status, and evidence. No database, runtime, or required tooling.

## The pattern

```text
agent-memory/
  index.md                               # okf_version, one line per concept, grouped by type
  log.md                                 # dated history: creations, supersessions, migrations
  pytest-import-errors-in-ci.md          # one concept per learning
  staging-deploys-need-the-vpn.md
```

- Each concept holds one symptom, root cause, useful dead end, setup quirk, or
  convention that isn't obvious from the code.
- A managed block in `CLAUDE.md` and/or `AGENTS.md` tells agents when to read,
  write, and migrate memory. Agents read `index.md` first, search the bundle for
  the symptom and tags, then open only the concepts that match.
- OKF's trust and lifecycle fields carry meaning: `status` separates confirmed
  findings from hypotheses and superseded ones, `verified` records who actually
  re-checked a claim, and `stale_after` flags findings that need a re-check.

Because the bundle is plain OKF, other OKF tools can read it, including the
graph viewer in Google's reference repository.
See the [v2 proposal and migration spec](docs/okf-memory-v2.md).

## Install

```text
/plugin marketplace add brianschroeder/simple-agent-memory
/plugin install simple-agent-memory
```

Ask the agent to "set up agent memory for this repo." It creates
`agent-memory/index.md` and `agent-memory/log.md` only when no memory exists and
installs the instruction block. If both `CLAUDE.md` and `AGENTS.md` already
exist, it updates both. If only one exists, it uses that file; otherwise it
defaults to `CLAUDE.md`.

## Upgrade an existing installation

After updating the plugin, ask the agent to "upgrade this repo's agent memory
to the OKF bundle format." Updating the plugin alone does not rewrite
previously installed repo instructions.

The skill upgrades the existing managed block and turns each entry in the old
`agent-memory.md` into its own concept. Original entry text stays verbatim;
frontmatter records only what the entry already states, so imported entries are
never marked verified or given invented dates. The skill verifies coverage and
a recoverable original before retiring `agent-memory.md`. Repeating setup does
not duplicate concepts or blocks.

## Manual setup

For a **fresh repository**, copy
[`index.template.md`](skills/agent-memory/assets/index.template.md) to
`agent-memory/index.md` and
[`log.template.md`](skills/agent-memory/assets/log.template.md) to
`agent-memory/log.md` (set today's date). Add the
[instruction block](skills/agent-memory/assets/claude-md-block.md) to your
`CLAUDE.md` or `AGENTS.md` (both if both are used).

For an **existing installation**, preserve surrounding instructions and upgrade
only the intact `agent-memory:start` / `agent-memory:end` block. Retain compatible
customizations; resolve conflicting custom paths or malformed markers before
editing. Let the agent follow the block's first-run migration rules. Never
create an empty bundle beside unmigrated memory. The block works without the
plugin.

## Keep it useful

Follow the concept format in the
[instruction block](skills/agent-memory/assets/claude-md-block.md). Search
before adding; update a matching concept instead of duplicating it. Supersede
obsolete findings with `status: deprecated` and a link to the replacement, and
keep the reasoning. Don't record secrets or transient task state. Link to
existing documentation.

Keep reading on demand rather than importing the bundle. Keep `index.md` in step
with the concepts; it is how agents decide what to open.

## License

MIT. See [LICENSE](LICENSE).

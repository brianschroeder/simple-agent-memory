# simple-agent-memory

Durable repo learnings in one **`MEMORY.md`**, shared across agent sessions and
reviewed in Git. Consistent headings, tags, dates, evidence, and related links
make known gotchas easier to find without a database or a memory directory.

## The pattern

- `MEMORY.md` holds the learnings: symptoms, root causes, useful dead ends,
  setup quirks, and conventions that aren't obvious from the code.
- A managed block in `CLAUDE.md` and/or `AGENTS.md` tells agents when to read,
  write, and migrate memory. Read memory before non-trivial work and when stuck.
  Search headings and tags, then read relevant entries and follow their links.

This is **OKF-inspired**, not an Open Knowledge Format bundle. Entries are
sections of a single Markdown file with readable metadata fields; there is no
per-entry YAML, automatic graph retrieval, or required tooling.
See the [v2 proposal and migration spec](docs/single-file-memory-v2.md).

## Install

```text
/plugin marketplace add brianschroeder/simple-agent-memory
/plugin install simple-agent-memory
```

Ask the agent to "set up agent memory for this repo." It creates `MEMORY.md`
only when no memory exists and installs the instruction block. If both
`CLAUDE.md` and `AGENTS.md` already exist, it updates both. If only one exists,
it uses that file; otherwise it defaults to `CLAUDE.md`.

## Upgrade an existing installation

After updating the plugin, ask the agent to "upgrade this repo's agent memory
to the single-file MEMORY.md format." Updating the plugin alone does not
rewrite previously installed repo instructions.

The skill upgrades the existing managed block and migrates entries while
preserving their original text, discovery dates, commands, and uncertainty.
It verifies coverage and a recoverable original before retiring the old
`agent-memory.md`. An existing `MEMORY.md` is never blindly replaced; ownership
conflicts are reported. Repeating setup does not duplicate entries or blocks.

## Manual setup

For a **fresh repository**, copy
[`MEMORY.template.md`](skills/agent-memory/assets/MEMORY.template.md) to
`MEMORY.md` only if no memory exists. Add the
[instruction block](skills/agent-memory/assets/claude-md-block.md) to your
`CLAUDE.md` or `AGENTS.md` (both if both are used).

For an **existing installation**, preserve surrounding instructions and upgrade
only the intact `agent-memory:start` / `agent-memory:end` block. Retain compatible
customizations; resolve conflicting custom paths or malformed markers before
editing. Let the agent follow the block's first-run migration rules. Never copy
the empty template over existing memory. The block works without the plugin.

## Keep it useful

Use the [entry format](skills/agent-memory/assets/MEMORY.template.md). Search
before adding; update a matching entry instead of duplicating it. Mark obsolete
findings superseded and retain the reasoning with a link to the replacement.
Don't record secrets or transient task state. Link to existing documentation.

Keep reading on demand rather than adding a full-file import. A single file
still has a practical size limit; curate it and reconsider splitting only if
real retrieval problems justify more structure.

## License

MIT. See [LICENSE](LICENSE).

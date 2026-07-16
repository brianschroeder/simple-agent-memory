<!-- agent-memory:start -->
## Agent memory

This repo keeps a durable, git-tracked learnings log at `./agent-memory.md`. It is the
shared memory for anyone (human or agent) working here.

- **Read `./agent-memory.md`** at the start of any non-trivial task, and **whenever you
  hit a persistent or recurring issue** — check whether it's already a known gotcha
  before investigating from scratch.
- **Append to `./agent-memory.md`** whenever you learn something a cold-start agent would
  have wanted to know: an environment or setup quirk, a non-obvious build/test/run
  invocation, the root cause of a resolved (especially recurring) bug, a dead end worth
  avoiding, or a repo-specific convention not enforced by code. **Before appending, scan
  the existing Log for an entry covering the same symptom or topic** — if one exists,
  merge your update into it or mark it `SUPERSEDED:` and add the corrected entry above it,
  rather than adding a new entry that duplicates or silently contradicts it. Use the entry
  format documented at the top of that file, add the entry while the detail is fresh, and
  keep it short and high-signal.
- **Never record secrets, transient task state, or anything already well covered by the
  code or docs** — link to those instead.
- **Using a tool that follows the `AGENTS.md` convention instead of
  `CLAUDE.md`** (e.g. Cursor, Aider)? Add this block to `AGENTS.md` instead
  of, or in addition to, `CLAUDE.md` — the read/append rules are identical
  either way.

> Read the file on demand (as above) rather than importing it with `@agent-memory.md`:
> imported files load in full at every session start and grow your baseline context as
> the log grows, whereas on-demand reads keep sessions lean and only pay the cost when
> the memory is actually needed.
<!-- agent-memory:end -->

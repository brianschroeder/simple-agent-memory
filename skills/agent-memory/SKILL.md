---
name: agent-memory
description: >-
  Set up repo-level agent memory: create an `agent-memory.md` learnings log at the
  repository root and wire up `CLAUDE.md` so agents read it when stuck and append durable
  learnings as they work. Use this whenever the user wants to add, set up, bootstrap,
  install, enable, or repair "agent memory," a "learnings file," a persistent
  notes/gotchas log for agents, or wants to make Claude Code carry lessons across sessions
  within a repo — even if they don't say the word "skill." Also use it to re-apply the
  pattern to a new repo.
---

# Agent Memory (repo-level)

This skill installs a lightweight, git-tracked memory pattern into a repository so that
learnings survive across fresh agent sessions. It sets up two things:

1. **`agent-memory.md`** at the repo root — a curated, high-signal log of learnings that
   a cold-start agent would have wanted to know (setup quirks, non-obvious invocations,
   resolved root causes, dead ends, repo conventions).
2. **A block in `CLAUDE.md`** instructing agents to *read* `agent-memory.md` at the start
   of non-trivial work and when stuck, and to *append* to it when they learn something
   durable — checking for and resolving duplicate or contradicting entries first, rather
   than stacking near-identical notes.

The division of labor matters: `CLAUDE.md` holds the *always-loaded behavior* (when to
read and write), and `agent-memory.md` holds the *accumulated content*, read on demand.

## Setting it up

Do this directly with your own file tools — read the bundled templates below, then create
or edit the files in the target repo. No script is needed; you create the files yourself.
Work at the **repository root** (the top of the project — confirm with
`git rev-parse --show-toplevel` if git is available, otherwise use the project's top
directory).

Both steps are deliberately **non-destructive — check before you write:**

1. **Create `agent-memory.md` at the repo root — only if it's missing.**
   - If `./agent-memory.md` already exists, leave it untouched. It holds accumulated
     learnings, and overwriting it would throw them away.
   - If it does not exist, create it with the exact contents of
     `assets/agent-memory.template.md`.

2. **Add the instruction block to `CLAUDE.md` — exactly once.**
   - Read `./CLAUDE.md` (create it if it doesn't exist).
   - If it already contains the marker `<!-- agent-memory:start -->`, leave it as-is so
     the block isn't duplicated.
   - Otherwise, append the full contents of `assets/claude-md-block.md`, preceded by a
     blank line if the file already has content.

Then report what changed (created the log? added the block? already present?) and remind
the user to commit both files.

## Manual example

For a fresh repo with no `CLAUDE.md` yet, the result is simply the two files at the root:
`agent-memory.md` (a copy of the template) and `CLAUDE.md` (containing the marker-wrapped
block). For a repo that already has a `CLAUDE.md`, the block is appended to the end of the
existing file and nothing else is touched.

## Customization notes

- **Different filename:** if the user wants a different name (e.g. `LEARNINGS.md`), rename
  the created file and update the path references inside the `CLAUDE.md` block to match.
- **Monorepos / per-package memory:** the pattern can be applied per subdirectory. A
  subdirectory `CLAUDE.md` loads on demand when an agent works in that directory, so a
  package-local `agent-memory.md` + a package-local `CLAUDE.md` block scopes memory to
  that package.
- **On-demand vs. always-loaded:** the default wires the file for *on-demand reading*. Do
  not switch to an `@agent-memory.md` import unless the log is small and always relevant —
  imports load the whole file into context at every session start and grow the baseline
  cost as the log grows.
- **Relationship to Claude Code's native auto memory:** native auto memory is per-user and
  lives outside the repo (not shared, not reviewed). This committed `agent-memory.md` is
  the *shared, versioned, review-gated* layer. They complement each other; this skill sets
  up the shared one.

## Bundled resources

- `assets/agent-memory.template.md` — the starter log, including the embedded rules for
  what/when to write and read, and the entry format. This is the substance of the pattern;
  copy it verbatim to create the log.
- `assets/claude-md-block.md` — the marker-wrapped block to append to `CLAUDE.md`.

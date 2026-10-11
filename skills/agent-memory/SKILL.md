---
name: agent-memory
description: >-
  Set up, upgrade, or repair a repository's single-file MEMORY.md and its
  CLAUDE.md / AGENTS.md instructions. Migrate legacy agent-memory.md entries
  without losing content; use for requests to install persistent repo learnings.
---

# Agent memory

Keep durable repo learnings in one `MEMORY.md` at the repository root. Use
[the memory template](assets/MEMORY.template.md) for its entry format and
[the instruction block](assets/claude-md-block.md) for reading, writing, and
first-run migration. Read both before editing. This is an OKF-inspired
single-file convention, not an Open Knowledge Format bundle.

## Setup or upgrade

1. Confirm the intended repository root. Read existing `CLAUDE.md`, `AGENTS.md`,
   `MEMORY.md`, and `agent-memory.md` before writing. Respect an explicitly
   configured custom memory path; don't silently create a second memory store.
   Inspect filename case variants as well, especially on macOS and Windows.
2. Follow the instruction block's first-run migration rules. For a fresh repo,
   copy the memory template only when no memory exists. For an existing repo,
   retain all actual entries, custom guidance, dates, commands, uncertainty, and
   superseded history. Add entry metadata only where the source supports it.
3. Install or upgrade the managed instruction block in both `CLAUDE.md` and
   `AGENTS.md` when both exist as separate files. If only one exists, use it.
   If neither exists, use the user's requested file, otherwise `CLAUDE.md`.
   If they resolve to the same file, edit it once. Preserve everything outside
   `<!-- agent-memory:start -->` / `<!-- agent-memory:end -->`.
4. An old marker is an upgrade target, not a reason to skip setup. Replace a
   single intact old block with the bundled version, retaining any compatible
   user customizations. Leave an identical current block unchanged. Append once
   when absent. For unmatched or multiple marker pairs, or custom instructions
   conflicting with the upgrade, stop that edit and explain the conflict.
5. Read back the result. Check entry preservation, same-file links, one managed
   block per instruction file, and consistent memory paths. Report whether this
   was a fresh install, upgrade, unchanged setup, or blocked migration. Report
   the recovery location when one was needed. Do not claim unfinished migration
   succeeded or commit an unresolved partial result.

Plugin updates alone do not edit previously installed repo instructions. Users
must invoke this updated setup skill once, or manually install the updated block.
The block is self-contained so its first-run migration does not need the plugin.

## Scope

Record durable conclusions, root causes, gotchas, and useful dead ends. Keep
repeatable procedures and volatile answers in their owning docs or tools and
link to them. Don't store secrets or transient task state. Read memory on demand;
do not introduce an automatic full-file import. Do not change tool-native memory
outside the requested repository. A user-requested package-local installation
uses the same rules within that package.

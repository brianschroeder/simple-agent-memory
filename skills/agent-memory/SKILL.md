---
name: agent-memory
description: >-
  Set up, upgrade, or repair a repository's agent memory: an Open Knowledge
  Format (OKF v0.2) bundle in agent-memory/ and its CLAUDE.md / AGENTS.md
  instructions. Migrate a legacy agent-memory.md log into one concept per
  learning without losing content; use for requests to install persistent repo
  learnings.
---

# Agent memory

Keep durable repo learnings in `agent-memory/` at the repository root, an
[OKF v0.2](https://github.com/GoogleCloudPlatform/open-knowledge-format/blob/main/SPEC.md)
bundle: one Markdown concept per learning with YAML frontmatter, plus the
reserved `index.md` and `log.md`. [The instruction block](assets/claude-md-block.md)
defines the concept format, reading, writing, and first-run migration. The
[index](assets/index.template.md) and [log](assets/log.template.md) templates
seed a new bundle. Read all three before editing.

## Setup or upgrade

1. Confirm the intended repository root. Read existing `CLAUDE.md`, `AGENTS.md`,
   `agent-memory/`, and `agent-memory.md` before writing. Respect an explicitly
   configured custom memory path; don't silently create a second memory store.
   Inspect filename case variants as well, especially on macOS and Windows,
   where existence tests match any case; compare names from a directory listing.
2. For a fresh repo, copy the index template to `agent-memory/index.md` and the
   log template to `agent-memory/log.md`, replacing `YYYY-MM-DD` with today's
   date. Create no concepts. For an existing repo, follow the instruction
   block's first-run migration rules. Retain all actual entries, custom
   guidance, dates, commands, uncertainty, and superseded history. Add
   frontmatter only where the source supports it. Upgrade the block (step 3)
   before removing the legacy file, so an interrupted run never leaves an old
   block pointing at a deleted file. To tell stock text from user
   customizations, compare with the shipped
   [legacy log template](assets/legacy/agent-memory.template.md) and
   [legacy block](assets/legacy/claude-md-block.md).
3. Install or upgrade the managed instruction block in both `CLAUDE.md` and
   `AGENTS.md` when both exist as separate files. If only one exists, use it.
   If neither exists, use the user's requested file, otherwise `CLAUDE.md`.
   If they resolve to the same file, edit it once. Preserve everything outside
   `<!-- agent-memory:start -->` / `<!-- agent-memory:end -->`.
4. An old block (one without `<!-- simple-agent-memory:instructions=3 -->`) is
   an upgrade target, not a reason to skip setup. Replace a single intact old
   block with the bundled version, retaining any compatible user customizations.
   Leave an identical current block unchanged. Append once when absent. For unmatched or multiple marker pairs, or custom instructions
   conflicting with the upgrade, stop that edit and explain the conflict.
5. Read back the result. Check OKF conformance (every non-reserved `.md` in the
   bundle has parseable frontmatter with a non-empty `type`; `index.md` carries
   only `okf_version`), that `index.md` lists every concept, that relative links
   resolve, entry coverage, one managed block per instruction file, and
   consistent memory paths. Report whether this was a fresh install, upgrade,
   unchanged setup, or blocked migration. Report the recovery location when one
   was needed. Do not claim unfinished migration succeeded or commit an
   unresolved partial result.

Plugin updates alone do not edit previously installed repo instructions. Users
must invoke this updated setup skill once, or manually install the updated block.
The block is self-contained so its first-run migration does not need the plugin.

## Scope

Record durable conclusions, root causes, gotchas, and useful dead ends. Keep
repeatable procedures and volatile answers in their owning docs or tools and
link to them. Don't store secrets or transient task state. Read memory on demand;
do not introduce an automatic import of the bundle. Do not change tool-native
memory outside the requested repository. A user-requested package-local
installation uses the same rules within that package.

# dedup/contradiction check on append — design

## Problem

A colleague asked (paraphrased): "How is the memory kept up to date to
resolve semantics?" The honest answer today is: it isn't, automatically.
`agent-memory.md` already documents a `SUPERSEDED:` convention and a
"merge duplicates" rule, but both live under a "Keep it curated" section
framed as *periodic* housekeeping — nothing requires an agent to check for
an existing, related entry at the moment it's about to append a new one.
Over time this lets near-duplicate or silently-contradicting entries stack
up, and the log's own stated failure mode ("past ~200 lines it gets
noticeably harder to use well") gets reached faster than necessary.

## Scope

Turn the existing periodic "merge duplicates / mark SUPERSEDED" advice into
a per-write check: before appending any entry, scan the Log for one that
already covers the same symptom/topic, and merge or supersede instead of
duplicating. Prompt-level only — no tooling, no CI, no automated semantic
diffing. This applies to `agent-memory.md` writes specifically, not to
edits of the `CLAUDE.md`/`AGENTS.md` block itself.

## Changes

**`skills/agent-memory/assets/claude-md-block.md`** — extend the existing
"Append to `./agent-memory.md`..." bullet with one added clause: before
appending, scan the Log for an entry covering the same symptom or topic; if
one exists, merge the update into it or mark it `SUPERSEDED:` and add the
corrected entry above it, rather than adding a duplicate or silently
contradicting entry. No new bullet, no restructuring — this is the
always-loaded block, so it should grow by a sentence, not a section.

**`skills/agent-memory/assets/agent-memory.template.md`** — add the same
check to the "Write to it when" section, immediately after the existing "add
the entry as soon as you learn it" sentence, and point it at the
already-documented `SUPERSEDED:` convention in "Keep it curated" rather than
re-explaining it — one canonical explanation of the convention, referenced
from two places.

**`skills/agent-memory/SKILL.md`** — update the one-sentence description of
what the `CLAUDE.md` block instructs agents to do (in the "This skill
installs..." intro, item 2) to mention the dedup/supersede check, so the
skill's own summary matches the block's actual behavior.

## Out of scope

- No CI workflow or automated duplicate detection (considered and explicitly
  deferred — this repo can't enforce checks in the repos that install the
  skill; a future version could ship an optional bundled CI asset).
- No change to the periodic "promote frequently-hit items" guidance, which
  remains a separate, occasional maintenance activity distinct from the
  mandatory per-write check.
- No change to `plugin.json` version — this is a content/wording change to
  the skill's guidance, not a new capability requiring a version bump
  decision beyond what's already tracked for the plugin as a whole.

## Validation

Same gate as the rest of this repo: `SKILL.md` frontmatter still parses
after the edit (no frontmatter touched, but verify nothing broke). Manual
read-through confirming the three files say the same thing at three
altitudes (always-loaded instruction, on-demand detail, skill summary) with
no contradiction between them.

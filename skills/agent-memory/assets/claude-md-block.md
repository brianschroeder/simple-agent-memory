<!-- agent-memory:start -->
## Agent memory

<!-- simple-agent-memory:instructions=2 -->
Keep shared repo learnings in `./MEMORY.md`, read on demand. Before non-trivial
work and whenever stuck, search its headings, tags, and symptoms; read matching
entries and follow related links before investigating from scratch.

Write durable findings while fresh. Use one unique heading per learning and
plain Markdown fields: Type, Tags, Discovered, Status, Trigger / symptom,
What to know / do, Why it matters, Evidence, and optional Related links.
Use a few lowercase tags. Types may include gotcha, convention, root-cause,
and dead-end. Discovered is the original YYYY-MM-DD or unknown. Status is
current, hypothesis, or superseded. Record unknown evidence as unknown.
Preserve uncertainty; formatting is not verification. Search before adding:
update duplicates, and retain superseded reasoning with a replacement link.
Never record secrets, transient task state, or facts already covered by docs;
link to those sources. Do not import the full file automatically.

### First-run migration

Before writing memory, inspect `MEMORY.md`, legacy `agent-memory.md`, filename
case variants, and any custom path named in existing instructions. Honor custom
paths; do not repurpose an unrelated `MEMORY.md` or overwrite another file.

- If neither file exists, create `MEMORY.md` with a Memory heading, these entry
  conventions, a Memories section, and `<!-- simple-agent-memory:format=2 -->`.
- Before converting any existing memory file, preserve its exact current bytes. An existing
  Git commit is sufficient only if it contains those exact bytes; otherwise
  make a recoverable backup outside the repo and report its location. Do not
  add backup copies to the active memory store or expose private data in commits.
- Convert existing entries in place or into `MEMORY.md`. Preserve all original
  entry text, commands, dates, caveats, custom guidance, and superseded history;
  add supported metadata without summarizing away detail. Preserve an original
  entry as a whole if its boundaries are ambiguous. Do not turn template examples
  into facts or use the migration date as the discovery date.
- When both files contain memory, preserve both before merging. Skip an entry
  only if its original content is already present verbatim. Keep differing
  entries and flag contradictions; do not silently resolve them. If ownership,
  collision, or contradictory instructions make a safe merge unclear, leave
  both files intact and report what needs resolution.
- Read back the result and account for every source entry and custom section.
  Check heading links, repairing known repo references to the old path without
  changing unrelated instructions. Remove the legacy file only after coverage
  is verified and recovery is available. Then add the format=2 marker if absent.
- A format=2 marker means formatting completed, not that legacy content can be
  ignored. If legacy memory remains or reappears, compare it using the same
  rules. If only completed v2 memory remains, do not reformat it on each run.
  On interruption, inspect actual files and resume without duplicating entries.

This is an OKF-inspired single-file convention, not a conformant OKF bundle.
The same rules apply in `CLAUDE.md` and `AGENTS.md`.
<!-- agent-memory:end -->

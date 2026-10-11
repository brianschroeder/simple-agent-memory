<!-- simple-agent-memory:format=2 -->
# Memory

Durable learnings for this repository. Read before non-trivial work and when
stuck. Search headings, tags, and symptoms, then read the matching entries and
related links. This is one shared, versioned Markdown file.

## Entry format

Use a unique, descriptive heading for each learning. Keep the body concise and
preserve existing headings that are link targets. The fields below are plain
Markdown, not per-entry YAML or a claim of OKF compatibility.

```markdown
### A symptom a future agent would search for
- **Type:** gotcha
- **Tags:** testing, environment
- **Discovered:** unknown
- **Status:** current
- **Trigger / symptom:** When the problem appears.
- **What to know / do:** The durable finding or fix.
- **Why it matters:** The failure or wasted work this prevents.
- **Evidence:** Link or describe the actual source/check; otherwise unknown.
- **Related:** Optional links to other headings in this file or relevant docs.
```

Use types such as `gotcha`, `convention`, `root-cause`, or `dead-end`; new types
are fine. Use a few consistent lowercase tags. Record the original discovery
date as YYYY-MM-DD when known, otherwise `unknown`; migration is not discovery.
Status is `current`, `hypothesis`, or `superseded`. These are local conventions,
not OKF lifecycle values. An imported entry is not newly verified merely
because it was reformatted. Preserve any original uncertainty.

Record discoveries while fresh. Before adding an entry, search for the same
symptom or topic. Update a matching entry instead of duplicating it. When a
finding becomes obsolete, mark it superseded and link to its replacement;
keep the historical reasoning. Preserve unique headings so links stay useful.

Never record secrets, transient task state, or speculative claims as facts.
Link to existing code and docs instead of duplicating them. Keep useful root
causes and dead ends even when the original problem is resolved. This file is
OKF-inspired; it is not a multi-document knowledge bundle or a search engine.

## Memories

<!-- Add real entries here, newest first. The fenced example above is not a memory. -->

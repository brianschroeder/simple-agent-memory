# Agent Memory

Durable, high-signal learnings for **this repository**. Read this before non-trivial
work and whenever you get stuck. Append to it whenever you learn something a cold-start
agent would have wanted to know from the beginning.

This file is committed to the repo on purpose: it is shared across every teammate,
every machine, and every agent session, and it is reviewed like any other change. Keep
it lean and true — it is a signal store, not a scratchpad.

---

## How to use this file

**Read it when:**
- You are **starting a non-trivial task** and want to avoid known traps.
- You **hit a persistent or recurring problem** — scan here *before* investigating from
  scratch. The answer to "why does X keep failing?" may already be written down. Spend
  30 seconds here before spending 30 minutes re-deriving it.

**Write to it when:**
- You solved something that took **real investigation** (more than a quick look).
- You discovered something **surprising, non-obvious, or counterintuitive** about this
  repo, its environment, its tooling, or its external dependencies.
- You catch yourself thinking *"I wish I'd known that at the start."* That thought is
  the trigger to add an entry.

Add the entry **as soon as you learn it**, while the detail is fresh — not "later." Before
adding it, check whether an existing entry already covers the same symptom or topic — if
so, merge into it or mark it `SUPERSEDED:` (see "Keep it curated" below) instead of
stacking a near-duplicate or silently contradicting entry.

---

## What to record (high value)

- **Environment & setup quirks:** required tool/runtime versions, environment variables,
  how credentials are wired, OS/WSL/container gotchas, proxy or network constraints.
- **Build / test / run invocations** that aren't obvious from a glance — and especially
  the ones that *look* right but fail, with the version that actually works.
- **Root causes of resolved bugs**, especially recurring ones, plus the fix that worked.
- **Dead ends:** approaches that look promising but don't work here, so nobody re-walks
  the same path. A well-documented dead end is worth as much as a working answer.
- **Repo-specific conventions not enforced by code:** naming, layout, "we do X not Y
  here," ordering requirements, things a linter won't catch.
- **External service behaviors:** rate limits, auth models, region/inference quirks,
  timeouts, retry semantics — the things you only learn by hitting them.

A good test: *would this have saved a brand-new agent time, and is it something they
could NOT quickly derive by reading the code?* If yes, record it.

## What NOT to record (noise or risk)

- **Secrets** — tokens, keys, passwords, connection strings, anything sensitive. Refer to
  *where* a secret lives (e.g. "in the `app-config` k8s Secret"), never the value itself.
- **Transient or one-off details** — "fixed a typo in line 12," today's ticket number,
  the state of a branch that will be gone next week.
- **Anything already well covered** by the code, README, or inline docs, or anything an
  agent will figure out within one session of working in the repo — link to it instead
  of copying it. Redundant entries dilute the signal and cost context on every read.
- **Speculation.** Record what you verified. If something is a strong hypothesis but
  unconfirmed, mark it as such so the next reader knows its weight.

---

## Entry format

Append new entries at the **top of the Log** (newest first). Keep each entry short and
skimmable, and lead with a symptom or claim that a stuck agent would actually search for.

```
### <symptom or learning, phrased the way a stuck agent would recognize it>
- **Trigger / symptom:** when this bites you (the observable thing)
- **What to know / do:** the durable insight, or the fix that works
- **Why it matters:** the failure mode it prevents or the time it saves
- _Discovered YYYY-MM-DD, while <one line of context>._
```

**Keep it curated.** This file is read on demand, but a bloated log still costs context
and buries the useful entries (as a rule of thumb, past ~200 lines it gets noticeably
harder to use well). So:
- **Merge duplicates** rather than stacking near-identical notes.
- When an entry becomes wrong or obsolete, don't silently delete the history — mark it
  `SUPERSEDED:` and put the corrected entry above it, so the reasoning isn't lost.
- Periodically promote frequently-hit items toward the top or into a short "Read this
  first" section.

---

## Log

<!-- Newest entries at the top. Delete the example below once you have real entries. -->

### (example) `pytest` passes locally but fails in CI with import errors
- **Trigger / symptom:** tests are green on your machine but red in CI, complaining it
  can't find a first-party module.
- **What to know / do:** CI runs from the repo root with `PYTHONPATH` unset. Run `pytest`
  from the repo root (not from inside `tests/`) to reproduce and fix.
- **Why it matters:** saves a long goose chase blaming the test code instead of the
  working directory.
- _Discovered 2026-01-01, while triaging a red build. (This is a placeholder — delete it
  once real entries exist.)_

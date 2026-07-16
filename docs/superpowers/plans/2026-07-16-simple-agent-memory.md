# simple-agent-memory Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Package the existing `agent-memory` skill (currently private files under `Downloads/`) as a standalone, installable, MIT-licensed Claude Code plugin at `github.com/brianschroeder/simple-agent-memory`.

**Architecture:** A single Claude Code plugin repo — `.claude-plugin/plugin.json` + `.claude-plugin/marketplace.json` self-registering the repo as its own marketplace, containing exactly one skill (`skills/agent-memory/`) with its `SKILL.md` and two asset templates, plus a README/LICENSE for GitHub presentation.

**Tech Stack:** Plain Markdown + JSON. No build, no runtime, no tests beyond JSON/frontmatter validation. Git + GitHub CLI (`gh`) for publishing.

## Global Constraints

- Repo root: `C:\Users\brian\OneDrive\Desktop\Code\Github\simple-agent-memory` (already `git init`'d).
- Local git identity for this repo is already set: `Brian Schroeder <54692447+brianschroeder@users.noreply.github.com>` — do not use the global NBA work email for commits here.
- License: MIT, copyright holder "Brian Schroeder", year 2026.
- Plugin/repo name: `simple-agent-memory`. Skill name inside it stays `agent-memory` (matches source `SKILL.md` frontmatter `name: agent-memory`).
- Source content to port (read-only references, already reviewed): `C:\Users\brian\Downloads\SKILL.md`, `C:\Users\brian\Downloads\agent-memory.template.md`, `C:\Users\brian\Downloads\claude-md-block.md`.
- `SKILL.md` and `agent-memory.template.md` are copied **verbatim** — no content changes.
- `claude-md-block.md` gets exactly one addition: a note that the block may target `AGENTS.md` instead of/alongside `CLAUDE.md`. No other wording changes.
- GitHub account: `brianschroeder` (confirmed via `gh auth status` / `gh api user`).
- Validation gate for every JSON/frontmatter file: must parse. No other CI.

---

## File Structure

```
simple-agent-memory/
├── .claude-plugin/
│   ├── plugin.json
│   └── marketplace.json
├── skills/
│   └── agent-memory/
│       ├── SKILL.md
│       └── assets/
│           ├── agent-memory.template.md
│           └── claude-md-block.md
├── docs/superpowers/{specs,plans}/   (already created — spec + this plan)
├── README.md
├── LICENSE
└── .gitignore
```

---

### Task 1: Plugin manifests

**Files:**
- Create: `.claude-plugin/plugin.json`
- Create: `.claude-plugin/marketplace.json`

**Interfaces:**
- Produces: a plugin named `simple-agent-memory`, installable via `/plugin marketplace add brianschroeder/simple-agent-memory` then `/plugin install simple-agent-memory`. Later tasks (the skill files) are referenced by this marketplace's `source: "."`.

- [ ] **Step 1: Write `plugin.json`**

```json
{
  "name": "simple-agent-memory",
  "version": "0.1.0",
  "description": "Repo-level agent memory for Claude Code: a git-tracked learnings log agents read when stuck and append to as they work.",
  "author": {
    "name": "Brian Schroeder",
    "url": "https://github.com/brianschroeder"
  },
  "homepage": "https://github.com/brianschroeder/simple-agent-memory",
  "repository": "https://github.com/brianschroeder/simple-agent-memory",
  "license": "MIT",
  "keywords": ["claude-code", "agent", "memory", "learnings", "context"]
}
```

- [ ] **Step 2: Write `marketplace.json`**

```json
{
  "name": "simple-agent-memory",
  "owner": {
    "name": "Brian Schroeder",
    "url": "https://github.com/brianschroeder"
  },
  "plugins": [
    {
      "name": "simple-agent-memory",
      "source": ".",
      "description": "Repo-level agent memory for Claude Code: a git-tracked learnings log agents read when stuck and append to as they work."
    }
  ]
}
```

- [ ] **Step 3: Validate both files parse as JSON**

Run:
```bash
cd "C:/Users/brian/OneDrive/Desktop/Code/Github/simple-agent-memory"
python3 -c "import json; json.load(open('.claude-plugin/plugin.json'))"
python3 -c "import json; json.load(open('.claude-plugin/marketplace.json'))"
```
Expected: both commands exit with no output (no exception).

- [ ] **Step 4: Commit**

```bash
cd "C:/Users/brian/OneDrive/Desktop/Code/Github/simple-agent-memory"
git add .claude-plugin/
git commit -m "feat: add plugin manifest and marketplace registration"
```

---

### Task 2: Port the agent-memory skill

**Files:**
- Create: `skills/agent-memory/SKILL.md` (verbatim copy of `C:\Users\brian\Downloads\SKILL.md`)
- Create: `skills/agent-memory/assets/agent-memory.template.md` (verbatim copy of `C:\Users\brian\Downloads\agent-memory.template.md`)
- Create: `skills/agent-memory/assets/claude-md-block.md` (copy of `C:\Users\brian\Downloads\claude-md-block.md` + one added note)

**Interfaces:**
- Consumes: nothing from Task 1.
- Produces: the skill content that `plugin.json`'s marketplace entry (Task 1) exposes. `SKILL.md` frontmatter must keep `name: agent-memory` — this is the identifier Claude Code uses to match/trigger the skill, and downstream repos installing this plugin will invoke it by that name.

- [ ] **Step 1: Copy `SKILL.md` verbatim**

Read `C:\Users\brian\Downloads\SKILL.md` and write its exact contents to `skills/agent-memory/SKILL.md`. Do not alter the frontmatter (`name: agent-memory`, `description: ...`) or body — this content was already reviewed and approved as tool-agnostic.

- [ ] **Step 2: Copy `agent-memory.template.md` verbatim**

Read `C:\Users\brian\Downloads\agent-memory.template.md` and write its exact contents to `skills/agent-memory/assets/agent-memory.template.md`.

- [ ] **Step 3: Copy `claude-md-block.md` with the AGENTS.md addition**

Read `C:\Users\brian\Downloads\claude-md-block.md`. Write it to `skills/agent-memory/assets/claude-md-block.md` with its content unchanged, except insert this new bullet immediately after the existing "Never record secrets..." bullet (before the closing blockquote note):

```markdown
- **Using a tool that follows the `AGENTS.md` convention instead of
  `CLAUDE.md`** (e.g. Cursor, Aider)? Add this block to `AGENTS.md` instead
  of, or in addition to, `CLAUDE.md` — the read/append rules are identical
  either way.
```

So the full file becomes:

```markdown
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
  avoiding, or a repo-specific convention not enforced by code. Use the entry format
  documented at the top of that file, add the entry while the detail is fresh, and keep
  it short and high-signal.
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
```

- [ ] **Step 4: Validate `SKILL.md` frontmatter parses**

Run:
```bash
cd "C:/Users/brian/OneDrive/Desktop/Code/Github/simple-agent-memory"
head -12 skills/agent-memory/SKILL.md
```
Expected: output shows a `---`-delimited YAML block containing `name: agent-memory` and a `description:` field, matching the source file's frontmatter shape.

- [ ] **Step 5: Commit**

```bash
cd "C:/Users/brian/OneDrive/Desktop/Code/Github/simple-agent-memory"
git add skills/
git commit -m "feat: port agent-memory skill (SKILL.md + templates)"
```

---

### Task 3: README

**Files:**
- Create: `README.md`

**Interfaces:**
- Consumes: the install command from Task 1 (`/plugin marketplace add brianschroeder/simple-agent-memory`), the file paths from Task 2 (`skills/agent-memory/assets/*.md`).
- Produces: nothing consumed by later tasks — this is the terminal user-facing doc.

- [ ] **Step 1: Write `README.md`**

```markdown
# simple-agent-memory

A repo-level memory pattern for AI coding agents. Learnings survive across
fresh agent sessions instead of being re-discovered from scratch every time.

## The problem

Every new agent session starts with a blank context. Environment quirks,
non-obvious build invocations, root causes of resolved bugs, and dead ends
all get re-learned — or worse, re-broken — session after session, unless
something persists them.

## The pattern

Two files, working together:

1. **`agent-memory.md`** at the repo root — a curated, git-tracked log of
   durable learnings (setup quirks, resolved bug root causes, dead ends,
   repo conventions not enforced by code).
2. **A block in `CLAUDE.md`** (or `AGENTS.md`) instructing agents to *read*
   `agent-memory.md` at the start of non-trivial work and when stuck, and to
   *append* to it when they learn something durable.

`CLAUDE.md`/`AGENTS.md` holds the always-loaded behavior (when to read and
write); `agent-memory.md` holds the accumulated content, read on demand — so
the memory log can grow without growing every session's baseline context
cost.

## Install (Claude Code plugin)

```
/plugin marketplace add brianschroeder/simple-agent-memory
/plugin install simple-agent-memory
```

Then, in any repo, ask Claude Code to set up agent memory (e.g. "set up
agent memory for this repo") and the skill will:
- create `agent-memory.md` at the repo root, if it doesn't already exist
- append the instruction block to `CLAUDE.md` (or `AGENTS.md`), if it isn't
  already there

Both steps are non-destructive — existing content is never overwritten.

## Manual install (no Claude Code plugins)

If you're not using Claude Code plugins, just copy the two files by hand:

1. Copy [`skills/agent-memory/assets/agent-memory.template.md`](skills/agent-memory/assets/agent-memory.template.md)
   to `agent-memory.md` at your repo root.
2. Copy the block from
   [`skills/agent-memory/assets/claude-md-block.md`](skills/agent-memory/assets/claude-md-block.md)
   into your `CLAUDE.md` or `AGENTS.md`.

Works with any agent that reads `CLAUDE.md`/`AGENTS.md` at session start —
not Claude Code-specific.

## Why on-demand, not `@import`

Don't switch to importing `agent-memory.md` (e.g. `@agent-memory.md`) into
`CLAUDE.md` unless the log is small and always relevant. Imports load the
whole file into context at every session start, so the baseline cost grows
as the log grows. Reading it on demand — at task start and when stuck —
keeps sessions lean and only pays the cost when the memory is actually
needed.

## License

MIT — see [LICENSE](LICENSE).
```

- [ ] **Step 2: Commit**

```bash
cd "C:/Users/brian/OneDrive/Desktop/Code/Github/simple-agent-memory"
git add README.md
git commit -m "docs: add README"
```

---

### Task 4: License and gitignore

**Files:**
- Create: `LICENSE`
- Create: `.gitignore`

**Interfaces:**
- Consumes: nothing.
- Produces: nothing consumed by later tasks.

- [ ] **Step 1: Write `LICENSE`**

```
MIT License

Copyright (c) 2026 Brian Schroeder

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

- [ ] **Step 2: Write `.gitignore`**

```
.DS_Store
Thumbs.db
```

- [ ] **Step 3: Commit**

```bash
cd "C:/Users/brian/OneDrive/Desktop/Code/Github/simple-agent-memory"
git add LICENSE .gitignore
git commit -m "chore: add MIT license and gitignore"
```

---

### Task 5: Publish to GitHub

**Files:** none (publish step only).

**Interfaces:**
- Consumes: all prior tasks' committed content.
- Produces: the public repo at `https://github.com/brianschroeder/simple-agent-memory`.

- [ ] **Step 1: Verify working tree is clean and all prior commits are present**

Run:
```bash
cd "C:/Users/brian/OneDrive/Desktop/Code/Github/simple-agent-memory"
git status
git log --oneline
```
Expected: `git status` shows "nothing to commit, working tree clean"; `git log` shows the spec commit plus Tasks 1–4 commits (5 total).

- [ ] **Step 2: Create the GitHub repo and push**

Run:
```bash
cd "C:/Users/brian/OneDrive/Desktop/Code/Github/simple-agent-memory"
gh repo create brianschroeder/simple-agent-memory --public --source=. --description "Repo-level agent memory for Claude Code and AGENTS.md-compatible tools." --push
```
Expected: command prints the new repo URL `https://github.com/brianschroeder/simple-agent-memory` and pushes the current branch.

- [ ] **Step 3: Verify the push**

Run:
```bash
gh repo view brianschroeder/simple-agent-memory --web=false
```
Expected: shows the repo description, default branch, and file listing matching the local tree.

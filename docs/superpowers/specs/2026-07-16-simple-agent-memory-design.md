# simple-agent-memory — design

## Problem

Coding agents (Claude Code and similar tools) start every session with a blank
context. Non-obvious setup quirks, resolved bug root causes, and dead ends get
re-discovered from scratch every session unless something persists them. There
is already a working pattern for this — a `CLAUDE.md` block that tells an
agent to read/write a git-tracked `agent-memory.md` log — but it currently
only exists as a private skill. This project packages it as a standalone,
installable, open source Claude Code plugin so anyone can adopt it in any
repo with one command.

## Scope

Exactly one skill: `agent-memory`. No skill suite, no framework, no
tool-agnostic spec document beyond what's needed to note that the pattern
also works with `AGENTS.md`.

## Repo

`github.com/brianschroeder/simple-agent-memory`, public, MIT licensed.

## Layout

```
simple-agent-memory/
├── .claude-plugin/
│   ├── plugin.json        # name "simple-agent-memory", MIT license, author brianschroeder
│   └── marketplace.json   # self-registering marketplace, source "."
├── skills/
│   └── agent-memory/
│       ├── SKILL.md
│       └── assets/
│           ├── agent-memory.template.md
│           └── claude-md-block.md
├── README.md
├── LICENSE
└── .gitignore
```

This mirrors the plugin shape already proven in the `cloudsre` monorepo
(`.claude-plugin/plugin.json` + `.claude-plugin/marketplace.json` +
`skills/<name>/SKILL.md`), so `/plugin marketplace add
brianschroeder/simple-agent-memory` followed by `/plugin install` works the
same way.

## Component behavior

**`SKILL.md` and `agent-memory.template.md`** — copied verbatim from the
already-validated source; both are already tool-agnostic (no NBA-specific or
personal content).

**`claude-md-block.md`** — same content as source, plus one added line
noting the block can be appended to `AGENTS.md` instead of, or alongside,
`CLAUDE.md` for tools that follow that convention (Cursor, Aider, etc.). The
skill's default install target stays `CLAUDE.md`; this is a note for
non-Claude-Code adopters, not a behavior change to the skill's setup logic.

**`plugin.json`** — `name: "simple-agent-memory"`, `version: "0.1.0"`,
description, `author: { name: "Brian Schroeder", url:
"https://github.com/brianschroeder" }`, `homepage`/`repository` pointing at
the new GitHub repo, `license: "MIT"`, keywords `["claude-code", "agent",
"memory", "learnings", "context"]`.

**`marketplace.json`** — one plugin entry, `source: "."`, same shape as
`cloudsre/.claude-plugin/marketplace.json`.

**`README.md`** — problem statement, quick install (plugin path: marketplace
add + install), manual install for non-plugin users (copy the two asset
files into your repo root, adapting paths), and a short callout on why the
memory file is read-on-demand rather than `@`-imported (pulled from the
skill's existing customization notes).

**`LICENSE`** — standard MIT text, copyright Brian Schroeder, 2026.

## Out of scope

- No CI/CD, no tests (the deliverable is markdown + two small JSON files).
- No support for other memory filenames beyond documenting the rename
  procedure already described in the skill's customization notes.
- No changes to the skill's non-destructive setup logic (check-before-write
  behavior for `agent-memory.md` and the `CLAUDE.md` marker).

## Validation

Before first commit: `plugin.json` and `marketplace.json` parse as valid
JSON; `SKILL.md` frontmatter (`name`, `description`) parses. Same gate the
`cloudsre` repo already uses for its own skills.

## Setup / publish steps

1. Create repo folder as a sibling under `Github/`, `git init`.
2. Write all files per the layout above.
3. Validate JSON + frontmatter.
4. Commit.
5. `gh repo create brianschroeder/simple-agent-memory --public --source=. --push`.

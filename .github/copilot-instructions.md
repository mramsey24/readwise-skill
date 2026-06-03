# Readwise skill workspace overview

This repository is a **Copilot skill/workspace repo**, not a conventional application codebase. The important behavior is split across a small set of markdown instruction files plus packaged skills under `.agents\skills`.

## Commands in repository docs

There are no repository-local build, lint, or test commands defined here.

The commands documented in this repo are for the external Readwise CLI:

```bash
npm install -g @readwise/cli
readwise login-with-token <token>
readwise --help
readwise reader-search-documents --help
readwise readwise-list-highlights --help
```

## High-level architecture

- `README.md` is the primary operator guide for the Readwise CLI workflow: document search/list/read/update flows, highlight operations, export flows, and common end-to-end workflows like inbox triage and quiz generation.
- `.agents\skills\readwise-cli\SKILL.md` packages the same Readwise CLI guidance as a reusable skill.
- `.agents\skills\build-persona\SKILL.md` defines a separate persona-building workflow that analyzes Reader documents and highlights, writes `reader_persona.md`, and is meant to be executed through a task subagent.
- `skills-lock.json` pins the installed skills to upstream GitHub sources and hashes.
- Workspace-root markdown files act as runtime policy/config:
  - `readwise_preferences.md` contains natural-language filtering rules that must be applied before presenting Readwise results.
  - `tagmap.md` contains natural-language auto-tagging rules.
  - `readwise_history.md` is the append-only action log for tagging and archiving work.

## Key conventions

- Always check `readwise_preferences.md` before fetching or presenting Readwise data, and apply its rules to all CLI-driven output.
- When performing **tagging** or **archiving**, update `readwise_history.md` in the root as a markdown table with timestamp, action text, and a document link.
- Only use `tagmap.md` when the user explicitly asks to auto-tag or process documents. Evaluate **metadata only** (`title`, `summary`, `author`); do not fetch full content just to tag.
- Prefer Readwise MCP tools when available. Fall back to the `readwise` CLI only when MCP access is unavailable.
- Treat user references to the Readwise **inbox** as location `new`.
- Use narrow `reader-list-documents` requests with minimal `--response-fields` first; fetch full document details only when the task actually requires content.
- For persona-building work, launch a task subagent, fetch all searches/lists in a single parallel batch, and summarize large JSON with one script rather than reading raw result files into the chat context.
- In the persona workflow, prefer highlight search over bulk highlight listing; `readwise_list_highlights` is explicitly discouraged there.

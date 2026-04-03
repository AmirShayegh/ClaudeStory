# claudestory

**[claudestory.com](https://claudestory.com)** | **[npm](https://www.npmjs.com/package/@anthropologies/claudestory)** | **[Privacy Policy](https://claudestory.com/privacy)**

**Cross-session context persistence for AI coding assistants.**

Every AI coding session starts from zero. The assistant doesn't know what was built yesterday, what's broken, what decisions were made, or what to work on next. Developers compensate with long startup prompts and scattered notes, but nothing carries forward automatically.

claudestory fixes this. A `.story/` directory in your project tracks tickets, issues, roadmap, handovers, and notes. Structured data that AI assistants read and write natively. Session 47 builds on session 46 instead of starting over.

## Quick Start

### 1. Install

```bash
npm install -g @anthropologies/claudestory
```

### 2. Set up the Claude Code skill and MCP server

```bash
claudestory setup-skill
```

### 3. Restart Claude Code, then type:

```
/story
```

On new projects, `/story` will analyze your codebase and offer to set up `.story/` with phases, tickets, and issues -- no manual init needed.

### Alternative: Install as Claude Code plugin

```
/plugin install story@claude-plugins-official
```

When installed as a plugin, the skill is available as `/story:go`.

## What It Creates

```
your-project/
└── .story/
    ├── config.json        # Project metadata
    ├── roadmap.json       # Phases with status derived from tickets
    ├── tickets/           # One JSON file per work item (T-001.json, T-002.json, ...)
    ├── issues/            # One JSON file per bug/gap (ISS-001.json, ISS-002.json, ...)
    ├── notes/             # Brainstorming and ideas (N-001.json, N-002.json, ...)
    ├── lessons/           # Process knowledge that persists across sessions (L-001.json, ...)
    ├── handovers/         # Session continuity documents (markdown)
    └── snapshots/         # State snapshots for session diffs
```

**Tickets** are planned work — features, tasks, refactors. **Issues** are discovered problems — bugs, inconsistencies, gaps. **Notes** are unstructured brainstorming — ideas, design thinking, explorations. **Lessons** are structured process knowledge — patterns learned from reviews, mistakes, and decisions. **Handovers** capture what each session accomplished so the next one picks up seamlessly.

## How It Works

```
Session start          Work                    Session end
     │                   │                          │
     ▼                   ▼                          ▼
  /story             tickets,              /story handover
  loads context      issues, notes          captures state
  from .story/       updated as             for next session
                     you work
```

1. **Start** — `/story` loads project status, latest handover, and development rules. You know exactly where things stand.
2. **Work** — Create tickets, log issues, update progress. The AI reads and writes `.story/` files as part of normal development.
3. **End** — A handover document captures what was done, decisions made, and what's next. Snapshots enable session diffs.
4. **Next session** — `/story` loads the handover. The new session has full context from the previous one. No re-explanation needed.

## Features

### Session Management
- **30 MCP tools** — Read and write project state from any MCP-compatible AI assistant
- **Full CLI** — `claudestory status`, `ticket next`, `recommend`, `snapshot`, `recap`, `export`, `validate`, and more
- **`/story` skill** — One-command session start for Claude Code
- **Snapshots + Recaps** — See what changed between sessions, with content diff tracking
- **PreCompact hooks** — Auto-snapshot before context compaction

### Project Tracking
- **Recommend engine** — Context-aware work suggestions mixing tickets and issues, with phase-proximity and unblock-impact awareness
- **Notes** — Unstructured brainstorming with tags, archiving, and search
- **Lessons** — Structured process knowledge that persists and ranks across sessions by reinforcement count
- **Export** — Self-contained project documents for sharing
- **Validation** — Reference integrity checks across all `.story/` files

### Autonomous Mode
- **`/story auto`** — Picks tickets, plans, reviews, implements, and commits in a loop until all work is done
- **Multi-backend code review** — Independent plan and code review via pluggable backends (Codex, agent) with adaptive depth
- **TDD stage** — Write tests before implementation, validate against baseline (configurable)
- **Endpoint verification** — Smoke test HTTP endpoints after code review (configurable)
- **Periodic checkpoints** — Auto-handover every N tickets for session continuity
- **Tiered access** — `/story review T-XXX`, `/story plan T-XXX`, `/story guided T-XXX` for scoped workflows

### Universal
- **Language-agnostic** — Works with any project, any language, any AI assistant that supports MCP

## CLI + MCP Reference

See [skills/go/reference.md](skills/go/reference.md) for the full list of CLI commands and MCP tools.

> The skill files in this repo are for reference. Authoritative versions are installed via `claudestory setup-skill`.

## Install

```bash
npm install -g @anthropologies/claudestory
```

[View on npm](https://www.npmjs.com/package/@anthropologies/claudestory)

---

© 2026 Amir Shayegh. Free for personal and noncommercial use under the [PolyForm Noncommercial License 1.0](LICENSE). For commercial licensing, contact shayegh@me.com.

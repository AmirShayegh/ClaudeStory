# claudestory

**Cross-session context persistence for AI coding assistants.**

Every AI coding session starts from zero. The assistant doesn't know what was built yesterday, what's broken, what decisions were made, or what to work on next. Developers compensate with long startup prompts and scattered notes, but nothing carries forward automatically.

claudestory fixes this. A `.story/` directory in your project tracks tickets, issues, roadmap, handovers, and notes — structured data that AI assistants read and write natively. Session 47 builds on session 46 instead of starting over.

## Without vs With

| | Without claudestory | With claudestory |
|---|---|---|
| **Session start** | Explain project from scratch | `/story` — full context in 3 seconds |
| **What to work on** | Ask the AI to guess | `recommend` suggests prioritized work with rationale |
| **Session end** | Context lost | Handover captures what was done, what's next |
| **Bug found last week** | Re-discovered, maybe re-introduced | Tracked as issue, visible every session |
| **Design decisions** | Re-debated | Preserved in handovers, never relitigated |

## Quick Start

```bash
# Install
npm install -g @anthropologies/claudestory

# Set up Claude Code skill + MCP server
claudestory setup-skill

# Initialize in your project
cd your-project
claudestory init --name my-project

# Restart Claude Code, then:
/story
```

## What It Creates

```
your-project/
└── .story/
    ├── config.json        # Project metadata
    ├── roadmap.json       # Phases with status derived from tickets
    ├── tickets/           # One JSON file per work item (T-001.json, T-002.json, ...)
    ├── issues/            # One JSON file per bug/gap (ISS-001.json, ISS-002.json, ...)
    ├── notes/             # Brainstorming and ideas (N-001.json, N-002.json, ...)
    ├── handovers/         # Session continuity documents (markdown)
    └── snapshots/         # State snapshots for session diffs
```

**Tickets** are planned work — features, tasks, refactors. **Issues** are discovered problems — bugs, inconsistencies, gaps. **Notes** are unstructured brainstorming — ideas, design thinking, "what if" explorations. **Handovers** capture what each session accomplished so the next one picks up seamlessly.

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

## Performance

Measured on a real project (82 tickets, 20 handovers, 10 phases):

| Metric | With MCP | Manual file reading |
|--------|----------|-------------------|
| **Time to context** | ~3 seconds | 1 min 41 sec |
| **Tokens consumed** | ~32,400 | ~46,500 |
| **Tool calls** | 3 | 100+ |

**34x faster. 30% fewer tokens.** The MCP server returns pre-computed summaries instead of requiring the AI to read and parse dozens of raw JSON files.

## Battle-Tested

This pattern emerged from 3 production projects:

- **Jamie Health** (healthcare AI) — 1,461 tests, 31 session handovers, 217 commits. Multi-agent code reviews caught 35+ real bugs per PR across 9 review rounds.
- **ChartThings** (macOS data viz) — 19 bugs caught in a single PR review cycle. Swift 6 concurrency patterns preserved across sessions.
- **claudestory itself** — Built using claudestory. 636 tests, 24 handovers, 100 tickets tracked.

## Features

- **27 MCP tools** — Read and write project state from any MCP-compatible AI assistant
- **Full CLI** — `claudestory status`, `ticket next`, `recommend`, `snapshot`, `recap`, `export`, `validate`, and more
- **`/story` skill** — One-command session start for Claude Code
- **Notes** — Unstructured brainstorming with tags, archiving, and search
- **Snapshots + Recaps** — See what changed between sessions, with content diff tracking
- **Recommend engine** — Context-aware work suggestions mixing tickets and issues, with phase-proximity awareness
- **PreCompact hooks** — Auto-snapshot before context compaction
- **Export** — Self-contained project documents for sharing
- **Validation** — Reference integrity checks across all `.story/` files
- **Language-agnostic** — Works with any project, any language, any AI assistant that supports MCP

## CLI + MCP Reference

See [skill/reference.md](skill/reference.md) for the full list of CLI commands and MCP tools.

## Install

```bash
npm install -g @anthropologies/claudestory
```

[View on npm](https://www.npmjs.com/package/@anthropologies/claudestory)

---

The claudestory CLI is free to install and use via npm. © 2026 Amir Shayegh. All rights reserved.

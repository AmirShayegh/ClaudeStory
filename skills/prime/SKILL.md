---
name: prime
description: Load full project context for a claudestory-managed project. Call at session start for any project with a .story/ directory.
---

# /story:prime - Load Project Context

claudestory tracks tickets, issues, roadmap, handovers, and notes in a `.story/` directory so every AI coding session builds on the last instead of starting from zero.

## Step 0: Check Setup

Check if the claudestory MCP tools are available by looking for `claudestory_status` in your available tools.

**If MCP tools ARE available**, proceed to Step 1.

**If MCP tools are NOT available:**

1. Check if the `claudestory` CLI is installed: run `claudestory --version` via Bash
2. If NOT installed:
   - Check `node --version` and `npm --version` - both must be available
   - If Node.js is missing, tell the user to install Node.js 20+ first
   - Otherwise, with user permission, run: `npm install -g @anthropologies/claudestory`
   - Then run `/reload-plugins` to restart the MCP server bundled with this plugin
3. If CLI IS installed but MCP tools still not available:
   - Run `/reload-plugins` to restart plugin MCP servers
   - If still unavailable, restart Claude Code

**Important:** Always use `npm install -g`, never `npx`, for the CLI. The MCP server needs the global binary.

**If MCP tools are unavailable and user doesn't want to set up**, fall back to CLI mode:
- Run `claudestory status` via Bash
- Run `claudestory recap` via Bash
- Run `claudestory handover latest` via Bash
- Read `RULES.md` and `WORK_STRATEGIES.md` if they exist in the project root
- Run `git log --oneline -10`
- Then continue to Step 3 below

## Step 1: Check Project

- If `.story/` exists in the current working directory (or a parent), proceed to Step 2
- If no `.story/` but the directory looks like a real project (has package.json, Cargo.toml, go.mod, pyproject.toml, .git, etc.), offer to initialize: "Want me to set up claudestory? It creates a .story/ directory for tracking tickets, issues, and session handovers." With permission, run `claudestory init --name <project-name>`
- If not a project directory, explain what claudestory is and suggest navigating to a project

## Step 2: Load Context

Call these in order:

1. **Project status** - call `claudestory_status` MCP tool
2. **Session recap** - call `claudestory_recap` MCP tool (shows changes since last snapshot)
3. **Recent handovers** - call `claudestory_handover_latest` MCP tool with `count: 3` (last 3 sessions' context - ensures reasoning behind recent decisions is preserved, not just the latest session's state)
4. **Development rules** - read `RULES.md` if it exists in the project root
5. **Lessons learned** - read `WORK_STRATEGIES.md` if it exists in the project root
6. **Recent commits** - run `git log --oneline -10`

## Step 3: Present Summary

After loading context, present a concise summary:

- Project progress (X/Y tickets complete, current phase)
- What changed since last snapshot (from recap)
- What the last session accomplished (from handover)
- Next ticket to work on
- Any high-severity issues or blockers
- Key process rules (from WORK_STRATEGIES.md if it exists)

For collaborative sessions, `claudestory_recommend` provides context-aware suggestions mixing tickets and issues. For autonomous sessions, `claudestory_ticket_next` provides queue-based next ticket.

Then ask: **"What would you like to work on?"**

## Session Lifecycle

- **Snapshots** save project state for diffing. They may be auto-taken before context compaction via the PreCompact hook bundled with this plugin.
- **Handovers** are session continuity documents. Create one at the end of significant sessions.
- **Recaps** show what changed since the last snapshot - useful for understanding drift.

**Never modify or overwrite existing handover files.** Handovers are append-only historical records. Always create new handover files. If you need to correct something from a previous session, create a new handover that references the correction.

Before writing a handover at the end of a session, call `claudestory_snapshot` first. This ensures the next session's recap can show what changed.

## Ticket and Issue Discipline

**Tickets** are planned work - features, tasks, refactors. They represent intentional, scoped commitments.

**Ticket types:**
- `task` - Implementation work: building features, writing code, fixing bugs, refactoring.
- `feature` - A user-facing capability or significant new functionality. Larger scope than a task.
- `chore` - Maintenance, publishing, documentation, cleanup. No functional change to the product.

**Issues** are discovered problems - bugs, inconsistencies, gaps, risks found during work. If you're not sure whether something is a ticket or an issue, make it an issue. It can be promoted to a ticket later.

When working on a task and you encounter a bug, inconsistency, or improvement opportunity that is out of scope for the current ticket, create an issue with a clear title, severity, and impact description. Don't fix it in the current task, don't ignore it - log it.

When starting work on a ticket, update its status to `inprogress`. When done, update to `complete` in the same commit as the code change.

## Notes

**Notes** are unstructured brainstorming artifacts - ideas, design thinking, "what if" explorations. Use notes when the content doesn't fit tickets (planned work) or issues (discovered problems).

Create, list, get, and update notes via MCP tools: `claudestory_note_create`, `claudestory_note_list`, `claudestory_note_get`, `claudestory_note_update`. Delete remains CLI-only: `claudestory note delete <id>`.

## Managing Entities

Ticket and issue create/update operations are available via both CLI and MCP tools. Delete remains CLI-only.

MCP tools for writes:
- `claudestory_ticket_create` with `title`, `type`, and optional `phase`, `description`, `blockedBy`, `parentTicket`
- `claudestory_ticket_update` with `id` and optional `status`, `title`, `order`, `description`, `phase`, `parentTicket`
- `claudestory_issue_create` with `title`, `severity`, `impact`, and optional `components`, `relatedTickets`, `location`, `phase`
- `claudestory_issue_update` with `id` and optional `status`, `title`, `severity`, `impact`, `resolution`, `components`, `relatedTickets`, `location`

Read operations (list, get, next, blocked, recommend) are available via both CLI and MCP.

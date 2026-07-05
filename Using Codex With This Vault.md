---
type: guide
status: active
area: personal
created: 2026-06-18
---
comm
# Using Codex With This Vault

Codex works best here as a capture assistant, editor, researcher, and vault caretaker. The default workflow is **proposal first**: Codex reviews the relevant notes and presents an exact plan before moving, renaming, deleting, consolidating, or substantially rewriting anything.

## The approval workflow

### 1. Ask for a proposal

> Review `00 Inbox`. Propose where each note belongs, which links and properties should be added, and whether anything should be split or merged. Do not edit files.

### 2. Adjust the proposal

> Keep the first note in the Inbox, but accept the rest. Preserve all original source material.

### 3. Approve execution

> Apply the revised proposal exactly as described, then summarize every file changed.

For small, reversible additions, you can explicitly authorize Codex to act immediately.

## High-value routines

### Process the Inbox

> Review the Inbox and propose a filing plan. Identify tasks, project material, reusable references, and sensitive information. Do not make changes yet.

### Start a project

> Draft a new project note for this outcome using my Project template. Propose the folder, milestones, next action, and related vault links.

### Create detailed tasks

> Review this project and propose individual task notes using my Task template. Give each task a clear outcome, next action, priority, due date only when justified, and a link back to the project. Do not create files yet.

### Review the task system

> Review my open task notes. Identify overdue work, unclear next actions, stale waiting items, competing priorities, and tasks that should be projects. Propose changes only.

### Consolidate knowledge

> Compare these notes for overlap. Propose one authoritative note, list what would be preserved from each source, and identify which originals could be archived. Do not edit yet.

### Improve technical documentation

> Review this guide for missing prerequisites, unsafe commands, verification steps, rollback instructions, and outdated assumptions. Propose an improved version.

### Review trading research

> Separate observations, assumptions, rules, and unresolved questions in these trading notes. Propose a structured research note without claiming profitability.

### Weekly review

> Review this week's Daily Notes and active projects. Summarize progress, unfinished tasks, decisions, recurring themes, and notes worth promoting to permanent references. Propose changes only.

### Vault health check

> Audit the vault for empty notes, duplicate topics, broken internal links, inconsistent titles, orphan notes, and sensitive data. Return a prioritized proposal; do not edit.

## Safety rules

- Do not store passwords, API keys, private keys, recovery codes, or full financial account details in ordinary notes.
- Ask Codex to preserve source material when summarizing important records.
- For commands, financial decisions, or infrastructure changes, ask for verification and rollback steps.
- Keep actual trades separate from missed-trade or hypothetical setup analysis.

## Organization rules

- `00 Inbox` is temporary capture.
- `01 Daily Notes` records the day.
- `02 Projects` contains work with a defined outcome.
- `Tasks/Task Notes` contains one detailed note per actionable task, while the dashboard stays easy to find in `Tasks`.
- Numbered subject folders contain ongoing areas and durable references.
- `10 Templates` keeps note structure consistent.
- `99 Archive` holds inactive material worth retaining.
- `_assets` stores screenshots and attachments.

## Properties

Keep properties minimal. The standard set is:

```yaml
---
type: reference
status: active
area: coding
created: 2026-06-18
---
```

Add a property only when it improves search, review, or a dashboard.

## Task notes

Tasks are stored as individual notes in `Tasks/Task Notes`. Create them with [[10 Templates/Task|the Task template]] and review them through [[Tasks/Tasks Dashboard|the Tasks Dashboard]].

Use these statuses:

- `open`
- `in-progress`
- `waiting`
- `completed`
- `cancelled`

Use `urgent`, `high`, `medium`, or `low` for priority. When finishing a task, set `status` to `completed` and fill the `completed` property with the completion date.

## What the dashboard provides

[[Home]] embeds [[Vault Dashboard.base]], a native Obsidian Base with selectable views for:

- unprocessed Inbox notes;
- active project notes created from the Project template;
- notes modified during the last 14 days.

The Base reads ordinary Markdown files and their properties. The folders remain the primary organization system.

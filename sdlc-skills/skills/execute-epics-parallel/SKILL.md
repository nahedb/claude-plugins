---
name: execute-epics-parallel
description: Use when 2 or more EPICs in the Beads backlog have no inter-dependencies and can be implemented simultaneously by parallel agents
---

# Execute Epics Parallel

## Overview

Identifies independent EPICs, confirms with the user, then dispatches one parallel agent per epic. Each agent runs the same implementer → reviewer task loop as `execute-epic`.

## Flow

1. Inspect the EPIC dependency graph:
   ```bash
   bd graph
   ```
2. Identify EPICs with no mutual dependencies (neither blocks the other, no shared ancestor dependency)
3. For each candidate epic, verify it has ready work:
   ```bash
   bd ready
   ```
   Exclude any epic where `bd ready` returns nothing — report the gap to the user.
4. Present the confirmed list to the user:
   > "These EPICs can run in parallel: [list]. Shall I proceed?"
5. After user confirms, invoke `dispatching-parallel-agents` skill — one slot per independent epic.
6. Pass each agent slot:
   - The epic ID it owns
   - Instruction to follow the `execute-epic` task loop (implementer → reviewer → close → repeat) scoped to its epic
7. After all parallel agents complete, report results and check for remaining sequential epics:
   ```bash
   bd query "type=epic status=open"
   ```
   If any remain, recommend `/execute-epic` for the next one.

## Agent Slot Instructions

Each parallel slot must:
- Only work on tasks that belong to its assigned epic
- List its epic's tasks before picking any: `bd list --parent <epic-id>` then check which are in `bd ready`
- Invoke `implementer` → `reviewer` for each task
- On reviewer pass: close task with `bd close <task-id>`, then close its GitHub Issue mirror (`bd show <task-id>` → `GitHub: <url>` → `gh issue close <number>`)
- On reviewer fail: reviewer creates BUG/REFACTOR in Beads automatically; also create a matching GitHub Issue with label `bug` assigned to the EPIC's milestone, storing the GitHub URL back in the Beads issue; loop back to `bd ready`
- Close the epic with `bd close <epic-id>` when all tasks are done, then close its GitHub Milestone (`bd show <epic-id>` → `GitHub-Milestone: <number>` → `gh api .../milestones/<number> --method PATCH -f state="closed"`)

**REQUIRED SUB-SKILL:** Use `hyperpowers:dispatching-parallel-agents` to dispatch the agent slots.

## Safety Gates

**Before dispatching:**
- At least 2 EPICs must have confirmed ready work — never dispatch a single slot (use `execute-epic` instead)
- User must confirm the epic list before parallel work begins

**After completion:**
- Always check for remaining open EPICs and surface them to the user

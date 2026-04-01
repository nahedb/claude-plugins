---
name: execute-epic
description: Use when ready to implement the highest-priority epic from the Beads backlog, running implementer and reviewer agents task-by-task until the epic is complete
---

# Execute Epic

## Overview

Picks the top-priority open EPIC from Beads and runs the implementer → reviewer loop for every task until the epic is done.

## Flow

```dot
digraph execute_epic {
    "Query top-priority open EPIC" [shape=box];
    "bd ready for that epic" [shape=box];
    "Tasks available?" [shape=diamond];
    "Report blocker to user, stop" [shape=box];
    "Show summary to user" [shape=box];
    "Invoke implementer for task" [shape=box];
    "Invoke reviewer for task" [shape=box];
    "Reviewer: pass?" [shape=diamond];
    "bd close task" [shape=box];
    "Reviewer creates BUG/REFACTOR in Beads" [shape=box];
    "More tasks in bd ready?" [shape=diamond];
    "bd close epic" [shape=box];
    "Surface next priority epic" [shape=box];

    "Query top-priority open EPIC" -> "bd ready for that epic";
    "bd ready for that epic" -> "Tasks available?";
    "Tasks available?" -> "Report blocker to user, stop" [label="no"];
    "Tasks available?" -> "Show summary to user" [label="yes"];
    "Show summary to user" -> "Invoke implementer for task";
    "Invoke implementer for task" -> "Invoke reviewer for task";
    "Invoke reviewer for task" -> "Reviewer: pass?";
    "Reviewer: pass?" -> "bd close task" [label="yes"];
    "Reviewer: pass?" -> "Reviewer creates BUG/REFACTOR in Beads" [label="no"];
    "Reviewer creates BUG/REFACTOR in Beads" -> "bd ready for that epic";
    "bd close task" -> "More tasks in bd ready?";
    "More tasks in bd ready?" -> "Invoke implementer for task" [label="yes"];
    "More tasks in bd ready?" -> "bd close epic" [label="no"];
    "bd close epic" -> "Surface next priority epic";
}
```

## Beads Commands

```bash
# Find the top-priority open EPIC (priority 0 = highest)
bd query "type=epic status=open"

# Show all ready tasks
bd ready

# Show tasks under a specific epic
bd list --parent <epic-id>

# Close a completed task
bd close <task-id>

# Close a completed epic
bd close <epic-id>

# Set epic priority if needed (0=highest, 4=lowest)
bd update <epic-id> -p 0
```

## Agent Instructions

**implementer:** Provide the task ID, acceptance criteria (from `bd show <task-id>`), and relevant file paths. The implementer delegates to the appropriate domain specialist (frontend, backend-dotnet, etc.).

**reviewer:** Provide the task ID and a summary of what was implemented. The reviewer will:
- Pass → close the task with `bd close <task-id>`
- Fail → it creates a BUG or REFACTOR issue in Beads automatically; loop back to `bd ready`

## Safety Gate

Before starting, run:
```bash
bd query "type=epic status=open"
```

If no EPICs exist, or if `bd ready` returns nothing, stop. Report which dependency is blocking the ready work and surface it to the user.

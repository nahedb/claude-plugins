---
name: brainstorm-solo
description: Use when you have a small feature idea or bug to log and implement — scope fits a single FEATURE with a few tasks, needs Beads tracking, and should move straight into implementation without BA/architect overhead
---

# Brainstorm Solo

## Overview

Lightweight conversational flow for small features and bugs. Ends with Beads FEATURE + TASKs created, mirrored as GitHub issues, and implementation started immediately. No agent delegation, no architecture docs.

## Flow

1. Ask clarifying questions one at a time — scope, acceptance criteria, bug vs feature
2. Check for an existing parent EPIC:
   ```bash
   bd query "type=epic status=open"
   ```
   If a natural parent exists, attach the FEATURE to it. If not, create standalone.
3. Detect the current GitHub repo:
   ```bash
   gh repo view --json nameWithOwner -q .nameWithOwner
   # e.g. "nahedb/BadBeats"
   ```
4. Create Beads issues:
   ```bash
   # Standalone feature
   bd create "Feature title" --type feature -d "what it does"

   # Feature under an existing epic
   bd create "Feature title" --type feature --parent <epic-id> -d "what it does"

   # Tasks under the feature (create 2–5)
   bd create "Task title" --type task --parent <feature-id> -d "acceptance criteria"
   ```
5. Mirror each Beads issue in GitHub and store the link back in Beads:

   **If the parent EPIC doesn't already have a GitHub Milestone, create one:**
   ```bash
   gh api repos/<owner>/<repo>/milestones \
     --method POST \
     -f title="<epic title>" \
     -f description="Beads: <epic-id>"
   # Returns milestone number — store it back in the Beads EPIC:
   bd update <epic-id> -d "<original description>\n\nGitHub-Milestone: <milestone-number>"
   ```

   **For the FEATURE and each TASK, create a GitHub Issue under that milestone:**
   ```bash
   gh issue create \
     --title "<same title as Beads issue>" \
     --body "**Beads:** <beads-id>" \
     --label "<feature|task>" \
     --milestone "<milestone-number>"
   # Store GitHub issue URL back in Beads:
   bd update <beads-id> -d "<original description>\n\nGitHub: <gh-issue-url>"
   ```
6. Verify tasks are ready:
   ```bash
   bd ready
   ```
7. Invoke `sdlc-skills:execute-epic` to begin implementation immediately.

## GitHub Mapping

| Beads type | GitHub equivalent |
|---|---|
| EPIC | Milestone |
| FEATURE | Issue (assigned to milestone, label `feature`) |
| TASK | Issue (assigned to milestone, label `task`) |
| BUG | Issue (assigned to milestone, label `bug`) |

Create labels if they don't exist: `gh label create <name>`

## What to Skip

No `business-analyst` agent. No `architect` agent. No ADR. No requirements docs. Just enough structure to track and implement the work.

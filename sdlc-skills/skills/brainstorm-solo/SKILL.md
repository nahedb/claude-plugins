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
5. For each Beads issue created, mirror it as a GitHub issue and store the link back in Beads:
   ```bash
   # Create GitHub issue
   gh issue create \
     --title "<same title as Beads issue>" \
     --body "**Beads:** <beads-id>\n\n<description>" \
     --label "<epic|feature|task>"

   # Store GitHub URL back into Beads so execute flows can find and close it
   bd update <beads-id> -d "<original description>\n\nGitHub: <gh-issue-url>"
   ```
   Repeat for every Beads issue (feature + each task).
6. Verify tasks are ready:
   ```bash
   bd ready
   ```
7. Invoke `executing-plans` skill to begin implementation immediately.

## GitHub Labels

Use these labels when creating GitHub issues (create the label if it doesn't exist):
- `epic` — for EPIC-type Beads issues
- `feature` — for FEATURE-type Beads issues
- `task` — for TASK-type Beads issues
- `bug` — for BUG-type Beads issues

## What to Skip

No `business-analyst` agent. No `architect` agent. No ADR. No requirements docs. Just enough structure to track and implement the work.

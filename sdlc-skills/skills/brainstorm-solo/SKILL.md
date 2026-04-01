---
name: brainstorm-solo
description: Use when you have a small feature idea or bug to log and implement — scope fits a single FEATURE with a few tasks, needs Beads tracking, and should move straight into implementation without BA/architect overhead
---

# Brainstorm Solo

## Overview

Lightweight conversational flow for small features and bugs. Ends with Beads FEATURE + TASKs created and implementation started immediately. No agent delegation, no architecture docs.

## Flow

1. Ask clarifying questions one at a time — scope, acceptance criteria, bug vs feature
2. Check for an existing parent EPIC:
   ```bash
   bd query "type=epic status=open"
   ```
   If a natural parent exists, attach the FEATURE to it. If not, create standalone.
3. Create Beads issues:
   ```bash
   # Standalone feature
   bd create "Feature title" --type feature -d "what it does"

   # Feature under an existing epic
   bd create "Feature title" --type feature --parent <epic-id> -d "what it does"

   # Tasks under the feature (create 2–5)
   bd create "Task title" --type task --parent <feature-id> -d "acceptance criteria"
   ```
4. Verify tasks are ready:
   ```bash
   bd ready
   ```
5. Invoke `executing-plans` skill to begin implementation immediately.

## What to Skip

No `business-analyst` agent. No `architect` agent. No ADR. No requirements docs. Just enough structure to track and implement the work.

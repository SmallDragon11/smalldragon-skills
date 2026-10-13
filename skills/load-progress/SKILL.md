---
name: load-progress
description: Pick up the handoff document saved by /save-progress and resume the work.
argument-hint: "[optional path to progress file]"
disable-model-invocation: true
---

Read the handoff document at `.claude/PROGRESS.md` under the project root, or at the path the user passed. If it is missing, say so and stop.

The document is a snapshot: check it against the current state (git status and log, the files and artifacts it references) and note any drift. Treat decisions it records as settled unless the user reopens them.

Report the goal, where things stand, any drift, and the proposed next step. Wait for the user to confirm before changing anything.

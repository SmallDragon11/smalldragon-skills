---
name: load-progress
description: Load .claude/PROGRESS.md saved by /save-progress and resume work from where the previous session left off.
disable-model-invocation: true
argument-hint: "[optional path to progress file]"
---

# Load Progress

Restore context from a previous session's handoff file and get ready to continue.

## Steps

1. Read the progress file: the path the user gave when invoking this skill (`$ARGUMENTS` in Claude Code, or any path mentioned alongside the invocation), otherwise `.claude/PROGRESS.md` under the current working directory.
   - If it does not exist, say so and stop. Suggest running `/save-progress` in the previous session next time.
2. Verify the file against reality — the state may have changed since it was written:
   - If this is a git repo: `git status --short`, `git log --oneline -5`, current branch.
   - Check that the key files listed still exist; skim the ones relevant to "In progress" and "Next steps".
   - Note any mismatches (e.g. a "next step" already done, files missing, new uncommitted changes).
3. Report to the user, in the language the progress file is written in:
   - Goal and current status (brief)
   - Any discrepancies found in step 2
   - The proposed next step (first item of "Next steps"), plus any open questions
4. Wait for the user's confirmation before making changes. Do not start editing on your own.

## Rules

- Treat "Decisions & rationale" as settled — do not re-propose rejected alternatives unless the user asks.
- Treat "Dead ends / gotchas" as things to avoid.
- Do not modify the progress file during loading.

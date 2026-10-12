---
name: save-progress
description: Save the current session's work progress to .claude/PROGRESS.md so a new session can resume seamlessly.
disable-model-invocation: true
argument-hint: "[optional note to include]"
---

# Save Progress

Write a handoff file that lets a fresh session (with zero memory of this conversation) continue the work immediately.

## Location

- Target file: `.claude/PROGRESS.md` under the current working directory (the project root).
- Create the `.claude/` directory if it does not exist.
- If the file already exists, Read it first. Rewrite it as one up-to-date document: merge still-relevant content, drop what is obsolete. Do not append session after session.

## Gather facts first

Before writing, collect concrete state instead of relying on recollection alone:
- If this is a git repo: `git status --short`, `git diff --stat`, `git log --oneline -10`, current branch.
- Files created or modified during this session (from the conversation).
- Any commands needed to build / run / test that were used this session.

## Content

Write in the same language the user has been using in this session. Use this structure; omit a section only if it truly has nothing:

```markdown
# Progress — <project or task name>

_Last updated: <YYYY-MM-DD HH:MM>_

## Goal
What we are trying to achieve overall, and the definition of done.

## Status summary
2–4 sentences: where things stand right now.

## Done
- Completed items (with file paths where relevant)

## In progress
- What was being worked on when the session stopped, and how far it got

## Next steps
1. Ordered, concrete, actionable steps — the first one should be startable immediately

## Decisions & rationale
- Decision — why (so the next session does not re-debate it)

## Dead ends / gotchas
- Approaches tried that failed and why; traps to avoid

## Key files
- `path/to/file` — role

## Useful commands
- `command` — what it does

## Open questions
- Things that need the user's input
```

## Rules

- Be specific: exact paths, function names, error messages, commands. Avoid vague phrases like "fixed some bugs".
- Write for a reader with no context; do not reference "above" or "earlier in the chat".
- Never include secrets (API keys, tokens, passwords).
- Keep it concise — aim for something readable in under a minute.
- If the user passed an argument ($ARGUMENTS), incorporate it as a note in the relevant section.

After writing, tell the user the file path and give a 2–3 line summary of what was recorded.

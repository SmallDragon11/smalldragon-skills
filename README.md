# smalldragon-skills

A collection of personal [Claude Code](https://claude.com/claude-code) skills.

## Install

In Claude Code:

```
/plugin marketplace add SmallDragon11/smalldragon-skills
/plugin install smalldragon-skills@smalldragon-skills
```

Update later with `/plugin marketplace update smalldragon-skills`.

## Skills

| Command | Description |
|---|---|
| `/smalldragon-skills:save-progress [note]` | Save the current session's progress to `.claude/PROGRESS.md` in the project, so a new session can resume seamlessly. |
| `/smalldragon-skills:load-progress [path]` | Load `.claude/PROGRESS.md`, verify it against the current project state, and propose the next step. Waits for confirmation before changing anything. |

Both skills are manual-only (`disable-model-invocation: true`); Claude will not run them on its own.

# smalldragon-skills

A collection of personal skills for [Claude Code](https://claude.com/claude-code) and [Codex](https://github.com/openai/codex).

## Install

### Claude Code

```
/plugin marketplace add SmallDragon11/smalldragon-skills
/plugin install smalldragon-skills@smalldragon-skills
```

Update later with `/plugin marketplace update smalldragon-skills`.

### Codex

Codex reads skills from `~/.agents/skills/`. Clone this repo and link (or copy) each skill folder there:

```powershell
git clone https://github.com/SmallDragon11/smalldragon-skills.git
New-Item -ItemType Junction -Path "$HOME\.agents\skills\save-progress" -Target "<repo>\skills\save-progress"
New-Item -ItemType Junction -Path "$HOME\.agents\skills\load-progress" -Target "<repo>\skills\load-progress"
```

```bash
# macOS / Linux
ln -s "<repo>/skills/save-progress" ~/.agents/skills/save-progress
ln -s "<repo>/skills/load-progress" ~/.agents/skills/load-progress
```

Invoke with `$save-progress` / `$load-progress` (or pick from `/skills`). Update with `git pull`.

## Skills

| Command (Claude Code / Codex) | Description |
|---|---|
| `/smalldragon-skills:save-progress [next focus]`<br>`$save-progress [next focus]` | Compact the current session into a handoff document at `.claude/PROGRESS.md` in the project, so a new session can resume seamlessly. |
| `/smalldragon-skills:load-progress [path]`<br>`$load-progress [path]` | Pick up `.claude/PROGRESS.md`, check it against the current project state, and propose the next step. Waits for confirmation before changing anything. |
| `/smalldragon-skills:gpt-explain [topic]`<br>(Claude Code only) | Send relevant session context to Codex (`codex exec`, read-only) and show GPT's explanation. With no argument, explains Claude's most recent reply. Requires the Codex CLI installed and logged in. |

All skills are manual-only: `disable-model-invocation: true` for Claude Code, and `agents/openai.yaml` with `allow_implicit_invocation: false` for Codex. Neither agent will run them on its own.

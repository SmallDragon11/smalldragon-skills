---
name: gpt-explain
description: Ask Codex (GPT) to explain something from the current session — either the topic given as arguments, or Claude's most recent reply when no arguments are given.
disable-model-invocation: true
argument-hint: "[what you want explained — omit to explain Claude's last reply]"
allowed-tools: Bash(codex exec:*), Write
---

# GPT Explain

The user wants a second explanation written by Codex (GPT), because they sometimes find its style easier to follow. Your job is to package the right context, hand it to Codex, and show Codex's answer. **Do not write the explanation yourself.**

## 1. Decide the mode

- **No arguments** (`$ARGUMENTS` is empty): the subject is **your most recent reply to the user** before this command was invoked (the last assistant message the user saw, not a tool call or this skill's own output). Also collect the context that reply depends on: the user message that prompted it (verbatim), plus any code, errors, command output, or earlier replies it refers to.
- **Arguments given**: the subject is what the user wrote in `$ARGUMENTS`. Collect the parts of this session that relate to it: relevant user questions, your replies, code snippets, error messages, file paths, command output, decisions made.

## 2. Build the brief

Write the brief to a temporary file, for example `gpt-explain-brief.md` in your scratchpad directory (or the system temp dir if no scratchpad is given). Use this structure:

```markdown
You are explaining something to a user who is working with another AI coding assistant (Claude Code).
They found the original explanation hard to follow and want a clearer one from you.

## What the user wants explained
<no-args mode: "Explain the assistant reply below in a clearer, easier-to-understand way.">
<args mode: the user's request, verbatim>

## Context from the session
<background: what the user is working on, the project/language, what led up to this — a few sentences>

## Material
<no-args mode: first the user message that prompted your last reply, verbatim; then the excerpts that reply depends on — quote code, errors, and earlier replies verbatim where they matter; then your last reply itself, VERBATIM — copy it in full, do not summarize or rephrase. Label each part clearly.>
<args mode: the relevant excerpts — quote code, errors, and earlier replies verbatim where they matter>

## How to answer
- Reply in <the language the user has been using in this session, e.g. Traditional Chinese (繁體中文)>.
- Start from the big picture, then the details. Use plain language, short paragraphs, and concrete examples or analogies.
- Explain jargon the first time it appears.
- You may read files in the current directory if it helps, but do not modify anything.
- If something in the material looks wrong, say so explicitly.
```

Rules for the brief:
- **Verbatim, not paraphrased.** The point is a second opinion from a different model; your rewording would bias it. Quote the original text and code.
- Include enough background that a reader with zero context can understand, but leave out unrelated parts of the session.
- Never include secrets (API keys, tokens, passwords, `.env` contents). Redact them as `<redacted>`.

## 3. Run Codex

Run from the project's working directory (use forward slashes in paths):

```bash
codex exec --sandbox read-only --ephemeral --skip-git-repo-check --color never \
  -C "<cwd>" -o "<scratchpad>/gpt-explain-answer.md" - < "<scratchpad>/gpt-explain-brief.md" > /dev/null
```

- `-` reads the prompt from stdin; `-o` writes only Codex's final answer to the file.
- `--sandbox read-only` lets Codex inspect project files but never change them.
- Set a generous timeout (e.g. 300000 ms); Codex can take a minute or two.
- Codex may print unrelated warnings to stderr (MCP/OAuth noise, etc.). Ignore them if the exit code is 0 and the answer file is non-empty.

If `codex` is not installed or not logged in, tell the user (`npm i -g @openai/codex`, then `codex login`) and stop. Do not fall back to explaining it yourself.

## 4. Show the answer

Read the answer file and present it to the user **as-is**, under a heading such as `## GPT 的解釋` (in the user's language). Do not rewrite, shorten, or "improve" it.

After it, you may add at most one or two short lines only if Codex said something factually wrong or contradicts what happened in the session. Otherwise add nothing.

---
name: codex
description: Executes OpenAI Codex CLI for AI-assisted code generation, multi-file editing, and automated refactoring. Use when the user asks to run Codex CLI commands (codex exec, codex resume), generate code patches, apply multi-file edits, execute AI-assisted shell commands, or references OpenAI Codex for code analysis or automated editing.
---

# Codex Skill Guide

## Running a Task

1. Default to `gpt-5.2` model. Ask the user (via `AskUserQuestion`) which reasoning effort to use (`xhigh`, `high`, `medium`, or `low`). User can override model if needed (see Model Options below).
2. Select the sandbox mode required for the task; default to `--sandbox read-only` unless edits or network access are necessary.
3. Assemble the command with the appropriate options:
   - `-m, --model <MODEL>`
   - `--config model_reasoning_effort="<high|medium|low>"`
   - `--sandbox <read-only|workspace-write|danger-full-access>`
   - `--full-auto`
   - `-C, --cd <DIR>`
   - `--skip-git-repo-check` (always include this flag)
4. **IMPORTANT**: Append `2>/dev/null` to all `codex exec` commands to suppress thinking tokens (stderr). Only show stderr if the user explicitly requests it or debugging is needed.
5. Run the command, capture output, and summarize the outcome for the user.
6. **Validate**: If the exit code is non-zero, report the error and ask the user how to proceed before retrying.
7. **After Codex completes**, inform the user: "You can resume this Codex session at any time by saying 'codex resume'."

### Resuming a Session

When continuing a previous session, pipe the new prompt via stdin. Do not add configuration flags unless the user explicitly requests a different model or reasoning effort:

```bash
echo "your prompt here" | codex exec --skip-git-repo-check resume --last 2>/dev/null
```

All flags must be inserted between `exec` and `resume`. The resumed session inherits model, reasoning effort, and sandbox mode from the original.

### Quick Reference

| Use case | Sandbox mode | Command pattern |
| --- | --- | --- |
| Read-only review or analysis | `read-only` | `codex exec --skip-git-repo-check -m gpt-5.2 --sandbox read-only "prompt" 2>/dev/null` |
| Apply local edits | `workspace-write` | `codex exec --skip-git-repo-check --sandbox workspace-write --full-auto "prompt" 2>/dev/null` |
| Network or broad access | `danger-full-access` | `codex exec --skip-git-repo-check --sandbox danger-full-access --full-auto "prompt" 2>/dev/null` |
| Resume recent session | Inherited | `echo "prompt" \| codex exec --skip-git-repo-check resume --last 2>/dev/null` |
| Run from another directory | Match task needs | `codex exec --skip-git-repo-check -C <DIR> "prompt" 2>/dev/null` |

## Model Options

| Model | Best for |
| --- | --- |
| `gpt-5.2` (default) | Software engineering, agentic coding workflows |
| `gpt-5.2-max` | Ultra-complex reasoning, deep problem analysis |
| `gpt-5.2-mini` | Cost-efficient coding (4x more usage allowance) |
| `gpt-5.1-thinking` | Adaptive thinking depth for hardest tasks |

All models support 400K input / 128K output context windows.

### Reasoning Effort Levels

| Level | When to use | Examples |
| --- | --- | --- |
| `xhigh` | Ultra-complex tasks | Deep problem analysis, complex multi-system reasoning |
| `high` | Complex tasks | Architecture refactoring, security analysis, performance optimization |
| `medium` | Standard tasks | Code organization, feature additions, bug fixes |
| `low` | Simple tasks | Quick fixes, formatting, documentation updates |

## Following Up

1. After every `codex` command, use `AskUserQuestion` to confirm next steps or decide whether to resume.
2. Restate the chosen model, reasoning effort, and sandbox mode when proposing follow-up actions.

## Error Handling

1. **Non-zero exit**: Stop, report the failure, and request direction via `AskUserQuestion` before retrying.
2. **High-impact flags**: Before using `--full-auto`, `--sandbox danger-full-access`, or `--skip-git-repo-check`, ask permission via `AskUserQuestion` unless already granted.
3. **Warnings or partial results**: Summarize issues and ask how to adjust via `AskUserQuestion`.
4. **Validation checkpoint**: After each `codex exec` run, check the exit code. If non-zero, inspect stderr (`codex exec ... 2>&1`), adjust the prompt or flags, and retry with user approval.

## CLI Version

Requires Codex CLI v0.57.0+. Check version: `codex --version`. Use `/model` within a session to switch models, or set defaults in `~/.codex/config.toml`.

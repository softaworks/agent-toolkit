---
name: gemini
description: "Use when the user asks to run Gemini, review code with an external LLM, analyze large files or codebases (>200k tokens), summarize lengthy documents, or process big context. Delegates to Gemini CLI for code review, plan review, multi-file analysis, codebase summarization, and large-document processing. Defaults to Gemini 3 Pro."
---

# Gemini Skill Guide

## When to Use

- User explicitly asks to run Gemini or use an external model
- **Code review** across multiple files or an entire repository
- **Plan review** of architectural specs, technical designs, or roadmaps
- **Large-context processing** requiring >200k tokens (full codebases, documentation sets, extensive logs)
- **Multi-file analysis** to find patterns, dependencies, or technical debt
- **Document summarization** of lengthy files or large document collections

## Background Mode Rule

**NEVER use `--approval-mode default` in non-interactive shells** (Claude Code tool calls, CI/CD). It hangs waiting for approval prompts.

- Use `--approval-mode yolo` (or `-y`) for all background/automated tasks
- Optionally wrap with `timeout 300 gemini ...` for safety
- Reserve `--approval-mode default` for interactive terminal sessions only

If a Gemini process hangs (20+ min, 0% CPU, sleeping state): `pkill -9 -f "gemini.*gemini-3"`

## Workflow

### 1. Choose a Model

Ask the user via `AskUserQuestion` which model to use:

| Model | Best for | Context | Notes |
| --- | --- | --- | --- |
| `gemini-3-pro-preview` | Complex reasoning, coding, agentic tasks | 1M in / 64k out | Default. 76.2% SWE-bench, $2-4/M input |
| `gemini-3-flash` | Speed-critical, sub-second latency | 1M in / 64k out | Distilled from 3 Pro |
| `gemini-2.5-pro` | Legacy all-around performance | 1M in / 65k out | Mature, stable |
| `gemini-2.5-flash` | Cost-optimized high-volume tasks | 1M in / 65k out | $0.15/M input |
| `gemini-2.5-flash-lite` | Maximum throughput | 1M in / 65k out | Fastest latency |

### 2. Select Approval Mode

| Mode | When to use |
| --- | --- |
| `yolo` | Background/automated tasks (required for non-interactive shells) |
| `auto_edit` | Interactive code review with auto-applied suggestions |
| `default` | Interactive terminal only — never in background |

### 3. Run the Command

**Key flags:**
- `-m, --model <MODEL>` — model selection
- `--approval-mode <mode>` — control tool approval
- `-y, --yolo` — shorthand for `--approval-mode yolo`
- `-i, --prompt-interactive "prompt"` — start interactive session with initial prompt
- `--include-directories <DIR>` — add directories to workspace scope
- `-s, --sandbox` — run in isolation

Before using `--approval-mode yolo`, `-y`, or `--sandbox`, ask the user for permission via `AskUserQuestion` unless already granted.

### 4. Verify and Follow Up

- Inform the user when Gemini completes
- Use `AskUserQuestion` to confirm next steps
- For follow-ups, start a new session with context from prior findings

## Examples

### Background Code Review
```bash
gemini -m gemini-3-pro-preview --approval-mode yolo \
  "Perform a comprehensive code review focusing on:
   1. Security vulnerabilities
   2. Performance issues
   3. Code quality and maintainability
   4. Best practices violations"
```

### Background Plan Review
```bash
gemini -m gemini-3-pro-preview --approval-mode yolo \
  "Review this architectural plan for scalability concerns, missing components, integration challenges, and alternative approaches"
```

### Large Codebase Analysis with Timeout
```bash
timeout 300 gemini -m gemini-3-pro-preview --approval-mode yolo \
  "Analyze the entire codebase: overall architecture, key patterns, technical debt, and refactoring opportunities"
```

### Interactive Review (Terminal Only)
```bash
gemini -m gemini-3-pro-preview -i "Review the authentication system" --approval-mode auto_edit
```

### Multi-Directory Analysis
```bash
gemini -m gemini-3-pro-preview --approval-mode yolo \
  --include-directories ../shared-lib --include-directories ../api \
  "Analyze cross-module dependencies and integration patterns"
```

## Error Handling

- Stop and report failures when `gemini --version` or any command exits non-zero
- Request direction before retrying failed commands
- Summarize warnings or partial results and ask how to adjust via `AskUserQuestion`

## Troubleshooting Hung Processes

```bash
# Detect: check for hung Gemini processes
ps aux | grep -E "gemini.*gemini-3" | grep -v grep

# Diagnose: check if process has network activity (0 = hung)
lsof -p <PID> 2>/dev/null | grep -E "(TCP|ESTABLISHED)" | wc -l

# Resolve: kill and verify
pkill -9 -f "gemini.*gemini-3-pro-preview"
ps aux | grep gemini | grep -v grep
```

**Prevention:** Always use `--approval-mode yolo` for background tasks. Add `timeout 300` wrapper for extra safety.

## Tips for Large Context

1. **Be specific** — provide structured prompts listing what to analyze
2. **Use `--include-directories`** — explicitly scope all relevant directories
3. **Match model to task** — use 3 Pro for quality, 3 Flash for speed, 2.5 Flash for cost
4. **Break down complex tasks** — structured analysis beats monolithic prompts
5. **Save findings** — ask Gemini to output structured reports for reference

## CLI Version

Requires Gemini CLI v0.16.0+. Check with: `gemini --version`

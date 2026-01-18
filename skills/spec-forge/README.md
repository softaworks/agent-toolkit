# /spec-forge

> Turn vague ideas into battle-tested implementation plans

**spec-forge** transforms rough feature descriptions into comprehensive, sectionized implementation plans through structured research, stakeholder interviews, and multi-LLM review.

## The Problem

```
You: "Claude, build me an auth system"
Claude: *starts coding immediately*
Result: Back-and-forth iterations, missed edge cases, scope creep
```

## The Solution

```
You: "/spec-forge @planning/auth-spec.md"
spec-forge: Research → Interview → Spec → Plan → External Review → Sections
Result: Clear implementation roadmap, reviewed by multiple LLMs, ready for execution
```

## Table of Contents

- [Workflow](#workflow)
- [Quick Start](#quick-start)
- [When to Use](#when-to-use)
- [Output Files](#output-files)
- [External Review](#external-review)
- [Resuming](#resuming)
- [Best Practices](#best-practices)
- [Integration with ralph-loop](#integration-with-ralph-loop)
- [File Structure](#file-structure)

## Workflow

```
┌─────────────────────────────────────────────────────────────────┐
│                      spec-forge pipeline                        │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   /spec-forge @spec.md                                          │
│          │                                                      │
│          ▼                                                      │
│   ┌──────────────┐     ┌──────────────┐     ┌──────────────┐    │
│   │   Research   │ ──▶ │  Interview   │ ──▶ │     Spec     │    │
│   │  (optional)  │     │  (5-10 Q&A)  │     │  Synthesis   │    │
│   └──────────────┘     └──────────────┘     └──────────────┘    │
│                                                   │             │
│                                                   ▼             │
│   ┌──────────────┐     ┌──────────────┐     ┌──────────────┐    │
│   │   Section    │ ◀── │   Integrate  │ ◀── │   External   │    │
│   │  Splitting   │     │   Feedback   │     │  LLM Review  │    │
│   └──────────────┘     └──────────────┘     └──────────────┘    │
│          │                                                      │
│          ▼                                                      │
│   ┌──────────────────────────────────────────────────────────┐  │
│   │  sections/section-01-*.md  sections/section-02-*.md ...  │  │
│   │  (Self-contained, parallel-ready implementation units)   │  │
│   └──────────────────────────────────────────────────────────┘  │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

## Quick Start

**1. Create a spec file**

```bash
mkdir -p planning
cat > planning/auth-spec.md << 'EOF'
# Authentication System

Need OAuth2 login with Google and GitHub.
Sessions stored in Redis, JWT for API auth.
EOF
```

Your spec can be detailed or just bullet points - the interview phase extracts the details.

**2. Run spec-forge**

```
/spec-forge @planning/auth-spec.md
```

**3. Follow the prompts**

Answer research and interview questions. Review the generated plan. Done.

## When to Use

**Use spec-forge when:**
- Requirements are fuzzy and need clarification
- The feature is complex enough to benefit from external review
- You want implementation sections that can be worked on in parallel
- You prefer thinking before coding

**Skip spec-forge when:**
- Simple bug fixes or one-file changes
- Requirements are already crystal clear
- You just want to start coding

## Output Files

After running spec-forge, your planning directory contains:

```
planning/
├── your-spec.md                 # Your original input
├── claude-research.md           # Web + codebase research findings
├── claude-interview.md          # Q&A transcript
├── claude-spec.md               # Synthesized specification
├── claude-plan.md               # Implementation plan
├── claude-integration-notes.md  # Review feedback decisions
├── reviews/
│   ├── gemini-review.md         # Gemini's feedback
│   └── codex-review.md          # Codex's feedback
└── sections/
    ├── index.md                 # Section manifest & dependencies
    ├── section-01-*.md          # Implementation unit 1
    ├── section-02-*.md          # Implementation unit 2
    └── ...
```

### Key Files

| File | Purpose |
|------|---------|
| `claude-plan.md` | The main deliverable - complete implementation plan |
| `sections/*.md` | Self-contained units ready for implementation |
| `reviews/*.md` | External perspectives on your plan |

## External Review

spec-forge uses **Gemini CLI** and **Codex CLI** to get independent reviews of your plan.

### Requirements

Install at least one:

```bash
# Gemini CLI (Google)
# See: https://github.com/google-gemini/gemini-cli

# Codex CLI (OpenAI)
# See: https://github.com/openai/codex
```

### What Reviewers Check

Both LLMs analyze your plan for:
- Potential footguns and edge cases
- Missing considerations
- Security vulnerabilities
- Performance issues
- Architectural problems
- Unclear requirements

### No CLI Installed?

If neither CLI is available, spec-forge will skip the external review step and continue with the workflow.

## Resuming

If the workflow is interrupted (context limit, need a break), just re-run with the same spec:

```
/spec-forge @planning/auth-spec.md
```

spec-forge detects existing files and resumes from where it left off.

### Resume Points

| Files Found | Resumes At |
|-------------|------------|
| `claude-research.md` | Interview |
| `+ claude-interview.md` | Spec synthesis |
| `+ claude-spec.md` | Plan generation |
| `+ claude-plan.md` | External review |
| `+ reviews/` | Feedback integration |
| `+ sections/index.md` | Section writing |

## Best Practices

1. **Start with something** - Even a few bullet points. The interview phase extracts details.

2. **Answer thoroughly** - The interview is where hidden requirements surface. Don't rush it.

3. **Review critically** - External LLMs catch blind spots but may over-engineer. You decide what to integrate.

4. **Use sections** - Each section file is self-contained. Work on them in parallel or hand them off.

5. **Iterate** - If the plan isn't right, edit `claude-plan.md` and re-run section generation.

## Integration with ralph-loop

After spec-forge generates your implementation sections, you can use the [ralph-loop](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/ralph-loop) plugin to execute each section autonomously.

### What is ralph-loop?

Ralph Loop is an iterative execution technique that keeps Claude working on a task until completion. It uses a Stop hook to create a self-referential feedback loop - Claude works, checks progress, and continues until the completion criteria are met.

### The Workflow

```
┌─────────────────────────────────────────────────────────────────┐
│                    spec-forge + ralph-loop                      │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   1. /spec-forge @planning/feature.md                           │
│      └── Generates sections/section-01-*.md, section-02-*, ...  │
│                                                                 │
│   2. Review sections/index.md for dependencies                  │
│                                                                 │
│   3. For each section (respecting dependency order):            │
│      └── /ralph-loop with section content                       │
│                                                                 │
│   4. Walk away. Come back to working code.                      │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Example: Executing a Section

After spec-forge completes, you have self-contained section files. Execute them with ralph-loop:

```bash
# Check the section index for dependency order
cat planning/sections/index.md

# Execute section 01 (usually foundation/setup)
/ralph-loop "Implement the following section. Follow all requirements exactly.

$(cat planning/sections/section-01-foundation.md)

When ALL acceptance criteria are met and tests pass:
- Output <promise>SECTION-01-COMPLETE</promise>

If blocked after 10 iterations, document blockers and output <promise>SECTION-01-BLOCKED</promise>" --completion-promise "SECTION-01" --max-iterations 30
```

### Batch Execution Script

For multiple sections, create a simple execution plan:

```bash
# Execute sections in dependency order
# (check sections/index.md for the correct order)

# Section 01 - Foundation (no dependencies)
/ralph-loop "$(cat planning/sections/section-01-foundation.md)

Output <promise>DONE</promise> when complete." --completion-promise "DONE" --max-iterations 30

# Section 02 and 03 can run in parallel (if you have multiple sessions)
# Section 04 depends on 02 and 03, run after both complete
```

### Full Auto: Execute All Sections

For a true "walk away" experience, let ralph-loop read the index and execute all sections in order:

```bash
/ralph-loop "You are implementing a feature based on a spec-forge plan.

## Your Mission

Read the planning documents and implement ALL sections in dependency order.

## Planning Documents

### Section Index (dependencies and order)
$(cat planning/sections/index.md)

### Section Files
$(for f in planning/sections/section-*.md; do echo "---"; echo "## $(basename $f)"; echo "---"; cat "$f"; echo ""; done)

## Execution Rules

1. Read the index.md to understand the dependency graph
2. Execute sections in the correct order (respect dependencies)
3. For each section:
   - Implement all requirements
   - Verify acceptance criteria are met
   - Run any tests mentioned
   - Only move to next section when current is complete
4. Track progress by creating planning/PROGRESS.md with completed sections

## On Completion

When ALL sections are implemented and verified:
- Update planning/PROGRESS.md with final status
- Output <promise>ALL-SECTIONS-COMPLETE</promise>

If blocked on any section after multiple attempts:
- Document the blocker in planning/PROGRESS.md
- Output <promise>BLOCKED</promise>" --completion-promise "COMPLETE" --max-iterations 100
```

This single command will:
1. Parse the section index for dependencies
2. Execute each section in the correct order
3. Track progress in `PROGRESS.md`
4. Continue until all sections are done (or hit max iterations)

### Tips for ralph-loop Integration

1. **Respect dependencies** - Check `sections/index.md` for the dependency graph before executing
2. **Set max-iterations** - Always use `--max-iterations` as a safety net (20-50 is reasonable)
3. **Clear completion criteria** - Each section has acceptance criteria; use them in your prompt
4. **One section at a time** - Each section is self-contained; don't mix multiple sections
5. **Review before next section** - Check the output before moving to dependent sections

### Installing ralph-loop

```bash
# Via Claude Code plugin marketplace
/plugin marketplace add anthropics/claude-plugins-official
/plugin install ralph-loop
/plugin enable ralph-loop
```

## File Structure

```
~/.claude/skills/spec-forge/
├── SKILL.md                    # Main skill definition
├── README.md                   # This file
└── references/
    ├── research-protocol.md    # How research works
    ├── interview-protocol.md   # Interview guidelines
    ├── external-review.md      # CLI review setup
    ├── section-index.md        # Index creation rules
    └── section-splitting.md    # Section file format
```

## Differences from Similar Tools

| Feature | spec-forge |
|---------|------------|
| API Keys Required | No - uses CLI tools |
| TDD Phase | No - focused on planning |
| Python Scripts | No - pure Claude skill |
| External Review | Via Gemini + Codex CLI |
| Resume Support | Yes - automatic |

---

**Author:** Leo
**License:** MIT

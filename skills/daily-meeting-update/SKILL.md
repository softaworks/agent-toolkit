---
name: daily-meeting-update
description: "Interactive daily standup/meeting update generator. Use when user says 'daily', 'standup', 'scrum update', 'status update', 'what did I do yesterday', 'prepare for meeting', 'morning update', or 'team sync'. Pulls activity from GitHub, Jira, and Claude Code session history. Conducts 4-question interview (yesterday, today, blockers, discussion topics) and generates formatted Markdown update."
user-invocable: true
---

# Daily Meeting Update

Generate a daily standup/meeting update through an **interactive interview**. Never assume tools are configured—ask first.

---

## Workflow

1. **Detect & Offer Integrations** - Silently check for gh/jira/Claude Code, ask user for consent, pull approved data
2. **Interview (with insights)** - Ask 4 questions using pulled data as context to trigger memory
3. **Generate Update** - Combine interview answers + tool data into clean Markdown

---

## Phase 1: Detect & Offer Integrations

### Step 1: Silent Detection

Check for available integrations **silently** (suppress errors, don't show to user):

| Integration | Detection |
|-------------|-----------|
| **Claude Code History** | `~/.claude/projects` directory exists with `.jsonl` files |
| GitHub CLI | `gh auth status` succeeds |
| Jira CLI | `jira` command exists |
| Atlassian MCP | `mcp__atlassian__*` tools available |
| Git | Inside a git repository |

### Step 2: Offer GitHub/Jira Integrations (if available)

> **Claude Code users:** Use `AskUserQuestionTool` tool for all questions in this phase.

**GitHub/Git:**

If `HAS_GH` or `HAS_GIT`:

```
"I detected you have GitHub/Git configured. Want me to pull your recent activity (commits, PRs, reviews)?"

Options:
- "Yes, pull the info"
- "No, I'll provide everything manually"
```

If yes:

```
"Which repositories/projects should I check?"

Options:
- "Just the current directory" (if in a git repo)
- "I'll list the repos" → user provides list
```

**Jira:**

If `HAS_JIRA_CLI` or `HAS_ATLASSIAN_MCP`:

```
"I detected you have Jira configured. Want me to pull your tickets?"

Options:
- "Yes, pull my tickets"
- "No, I'll provide everything manually"
```

### Step 3: Pull GitHub/Jira Data (if approved)

**GitHub/Git** — For each approved repo:
- Commits by user since yesterday
- PRs opened/merged by user
- Reviews done by user

**Jira** — Tickets assigned to user, updated in last 24h

**Key insight**: Store results to use as context in Phase 2 interview.

### Step 4: Offer Claude Code History

**Detection:**
```bash
ls ~/.claude/projects/*/*.jsonl 2>/dev/null | head -1
```

**If Claude Code history exists, ask:**

```
"I can also pull your Claude Code session history from yesterday. This can help recall work that isn't in git/Jira (research, debugging, planning). Want me to check?"

Options:
- "Yes, pull my Claude Code sessions"
- "No, I have everything I need"
```

**If yes, run the digest script:**

```bash
python3 ~/.claude/skills/daily-meeting-update/scripts/claude_digest.py --format json
```

**Then present sessions with multiSelect:**

Use `AskUserQuestionTool` with `multiSelect: true` to let user pick relevant items:

```
"Here are your Claude Code sessions from yesterday. Select the ones relevant to your standup:"

Options (multiSelect):
- "Fix authentication bug (backend-api)"
- "Implement OAuth flow (backend-api)"
- "Update homepage styles (frontend-app)"
- "Research payment providers (docs)"
```

**Key insight:** User selects which sessions are work-related. Personal projects or experiments can be excluded.

**Do NOT run digest script when:**
- User explicitly says "No" to Claude Code history
- User says they'll provide everything manually
- `~/.claude/projects` directory doesn't exist

**If digest script fails:**
- Fallback: Skip Claude Code integration silently, proceed with interview
- Common issues: Python not installed, no sessions from yesterday, permission errors
- Do NOT block the standup flow — the script is supplemental, not required

---

## Phase 2: Interview (with insights)

> **Claude Code users:** Use `AskUserQuestionTool` tool to conduct the interview. This provides a better UX with structured options.

**Use pulled data as context** to make questions smarter.

### Question 1: Yesterday

**If data was pulled**, show it first:

```
"Here's what I found from your activity:
- Merged PR #123: fix login timeout
- 3 commits in backend-api
- Reviewed PR #456 (approved)

Anything else you worked on yesterday that I missed?"
```

**If no data pulled:**

```
"What did you work on yesterday/since the last standup?"
```

If user response is vague, ask follow-up:
- "Can you give more details about X?"
- "Did you complete anything specific?"

### Question 2: Today

```
"What will you work on today?"

Options:
- [Text input - user types freely]
```

**If Jira data was pulled**, you can suggest:

```
"I see you have these tickets assigned:
- PROJ-123: Implement OAuth flow (In Progress)
- PROJ-456: Fix payment bug (To Do)

Will you work on any of these today?"
```

### Question 3: Blockers

```
"Do you have any blockers or impediments?"

Options:
- "No blockers"
- "Yes, I have blockers" → follow-up for details
```

### Question 4: Topics for Discussion

```
"Any topic you want to bring up at the end of the daily?"

Options:
- "No, nothing to discuss"
- "Yes" → follow-up for details

Examples of topics:
- Technical decision that needs input
- Alignment with another team
- Question about prioritization
- Announcement or info for the team
```

---

## Phase 3: Generate Update

Combine all information into clean Markdown:

```markdown
# Daily Update - [DATE]

## Yesterday
- [Items from interview]
- [Items from GitHub/Jira if pulled]

## Today
- [Items from interview]

## Blockers
- [Blockers or "No blockers"]

## PRs & Reviews (if pulled from GitHub)
- [PRs opened]
- [PRs merged]
- [Reviews done]

## Jira (if pulled from Jira)
- [Tickets updated]

## Topics for Discussion
- [Topics or "None"]

---
*Links:*
- [PR links]
- [Ticket links]
```

---

## Core Principles

1. **Interview is primary** — Tools supplement, they don't replace human context
2. **Consent before access** — Always ask before pulling from any integration
3. **Context-aware questions** — Show pulled data during interview to trigger memory ("I see you merged PR #123...")

---

## Quick Reference

| Phase | Action | Tool |
|-------|--------|------|
| 1. Detect & Offer | Check gh/jira/claude history, ask user, pull data | Bash (silent), AskUserQuestionTool* |
| 2. Interview | Ask 4 questions with insights | AskUserQuestionTool* |
| 3. Generate | Format Markdown | Output text |

*Claude Code only: Use `AskUserQuestionTool` tool for structured questions.

### Claude Code Digest Script

```bash
# Get yesterday's sessions as JSON
python3 ~/.claude/skills/daily-meeting-update/scripts/claude_digest.py --format json

# Get today's sessions
python3 ~/.claude/skills/daily-meeting-update/scripts/claude_digest.py --date today --format json

# Filter to specific project
python3 ~/.claude/skills/daily-meeting-update/scripts/claude_digest.py --project ~/my-app --format json
```

---

## Examples

For a complete example session and output template, see [references/examples.md](references/examples.md).

---

## Constraints

- **Never assume tools are configured** — detect silently, ask before pulling
- **Never skip "Topics for Discussion"** — often the most valuable part that tools cannot capture
- **Never generate more than 15 bullets** — standup should be <2 minutes to read
- **Never include ticket/PR numbers without context** — always include title or summary
- **Never pull data from repos user didn't approve** — respect boundaries
- **Always pull data before interview** — showing insights makes questions smarter
- **Always complete all 4 questions before generating** — blockers or discussion topics may change the narrative
- **Summarize, don't copy** — raw commit messages ("fix", "wip") don't tell the story

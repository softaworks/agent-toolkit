---
name: daily-meeting-update
description: "Interactive daily standup/meeting update generator. Use when user says 'daily', 'standup', 'scrum update', 'status update', or wants to prepare for daily meeting. Conducts interview (yesterday, today, blockers, discussion topics) and optionally pulls activity from GitHub/git/Jira if available and approved by user."
user-invocable: true
---

# Daily Meeting Update

Generate a daily standup/meeting update through an **interactive interview**. Never assume tools are configured—ask first.

---

## Workflow

```
START
  │
  ▼
┌─────────────────────────────────────────────────────┐
│ Phase 1: DETECT & OFFER INTEGRATIONS                │
│ • Check: gh CLI installed? jira CLI? Atlassian MCP? │
│ • If available → Ask user: "Want me to pull data?"  │
│ • If yes → Ask which repos/projects                 │
│ • Pull data NOW (before interview)                  │
├─────────────────────────────────────────────────────┤
│ Phase 2: INTERVIEW (with insights)                  │
│ • Show pulled data as context                       │
│ • Yesterday: "I see you merged PR #123, what else?" │
│ • Today: What will you work on?                     │
│ • Blockers: Anything blocking you?                  │
│ • Topics: Anything to discuss at end of meeting?    │
├─────────────────────────────────────────────────────┤
│ Phase 3: GENERATE UPDATE                            │
│ • Combine interview answers + tool data             │
│ • Format as clean Markdown                          │
│ • Present to user                                   │
└─────────────────────────────────────────────────────┘
```

---

## Phase 1: Detect & Offer Integrations

### Step 1: Silent Detection

Check for available integrations **silently** (suppress errors, don't show to user):

| Integration | Detection |
|-------------|-----------|
| GitHub CLI | `gh auth status` succeeds |
| Jira CLI | `jira` command exists |
| Atlassian MCP | `mcp__atlassian__*` tools available |
| Git | Inside a git repository |

### Step 2: Offer Integrations (if available)

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

### Step 3: Pull Data (if approved)

**GitHub/Git** — For each approved repo:
- Commits by user since yesterday
- PRs opened/merged by user
- Reviews done by user

**Jira** — Tickets assigned to user, updated in last 24h

**Key insight**: Store results to use as context in Phase 2 interview.

---

## Phase 2: Interview (with insights)

> **Claude Code users:** Use `AskUserQuestionTool` tool to conduct the interview. This provides a better UX with structured options.

**Use pulled data as context** to make questions smarter.

### Question 1: Yesterday

**If data was pulled**, show it first:

```
"Here's what I found from your activity:
- Merged PR #123: fix login timeout
- 3 commits in my-life-manager
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
| 1. Detect & Offer | Check gh/jira/mcp, ask user, pull data | Bash (silent), AskUserQuestionTool* |
| 2. Interview | Ask 4 questions with insights | AskUserQuestionTool* |
| 3. Generate | Format Markdown | Output text |

*Claude Code only: Use `AskUserQuestionTool` tool for structured questions.

---

## Example Session

```
Claude: "Let's prepare your daily! I detected GitHub configured. Want me to pull your recent activity?"
User: "Yes"

Claude: "Which repos should I check?"
User: "my-life-manager and softaworks-landing"

[Claude pulls data silently]

Claude: "Here's what I found from your activity:
- Merged PR #120: fix login timeout
- Opened PR #125: feat: add OAuth flow
- Reviewed PR #123 (approved), PR #456 (changes requested)
- 5 commits in my-life-manager

Anything else you worked on yesterday that I missed?"
User: "I also did some research on payment providers"

Claude: "What will you work on today?"
User: "Continue the OAuth feature and deploy to staging"

Claude: "Any blockers?"
User: "No"

Claude: "Any topic to bring up at the end of the daily?"
User: "Yes, I want to discuss the architecture of the new payments module"

[Claude generates update]
```

---

## Output Example

```markdown
# Daily Update - 2026-01-22

## Yesterday
- Worked on authentication feature
- Research on payment providers
- Merged PR #120 (fix: login timeout)
- Opened PR #125 (feat: add OAuth flow)

## Today
- Continue OAuth feature
- Deploy to staging

## Blockers
- No blockers

## PRs & Reviews
- **Opened:** PR #125 - feat: add OAuth flow
- **Merged:** PR #120 - fix: login timeout
- **Reviews:** PR #123 (approved), PR #456 (changes requested)

## Topics for Discussion
- Architecture of the new payments module

---
*Links:*
- https://github.com/org/repo/pull/125
- https://github.com/org/repo/pull/120
```

---

## Anti-Patterns

| Avoid | Why (Expert Knowledge) | Instead |
|-------|------------------------|---------|
| Run gh/jira without asking | Users may have personal repos visible, or be in a sensitive project context they don't want exposed | Always ask first, let user choose repos |
| Assume current directory is the only project | Developers often work on 2-5 repos simultaneously (frontend, backend, infra) | Ask "Which projects are you working on?" |
| Skip interview even with tool data | Tools capture WHAT happened but miss WHY and context (research, meetings, planning) | Interview is primary, tools supplement |
| Generate update before all 4 questions | User might have critical blocker or discussion topic that changes the narrative | Complete interview, then generate |
| Include raw commit messages | Commit messages are often cryptic ("fix", "wip") and don't tell the story | Summarize into human-readable outcomes |
| Ask for data after interview | Showing insights during interview makes questions smarter ("I see you merged PR #123, anything else?") | Pull data first, then interview with context |

---

## NEVER

- **NEVER assume tools are configured** — Many devs have gh installed but not authenticated, or jira CLI pointing to wrong instance
- **NEVER skip the "Topics for Discussion" question** — This is often the most valuable part of standup that tools can't capture
- **NEVER generate more than 15 bullets** — Standup should be <2 minutes to read; long updates lose the audience
- **NEVER include ticket/PR numbers without context** — "PROJ-123" means nothing; always include title or summary
- **NEVER pull data from repos user didn't explicitly approve** — Even if you can see other repos, respect boundaries

# Daily Meeting Update - Examples

## Example Session

```
Claude: "Let's prepare your daily! I detected GitHub configured. Want me to pull your recent activity?"
User: "Yes"

Claude: "Which repos should I check?"
User: "backend-api and frontend-app"

[Claude pulls GitHub data silently]

Claude: "I can also pull your Claude Code session history from yesterday. Want me to check?"
User: "Yes"

[Claude runs digest script, gets 5 sessions]

Claude: "Here are your Claude Code sessions from yesterday. Select the ones relevant to your standup:"
  ☑ Fix authentication timeout bug (backend-api)
  ☑ Implement OAuth flow (backend-api)
  ☐ Update personal blog (blog) - [user unchecks personal project]
  ☑ Research payment providers (docs)
  ☐ Debugging test failures (experiments)

User: [selects 3 relevant items]

Claude: "Here's what I found from your activity:
- Merged PR #120: fix login timeout
- Opened PR #125: feat: add OAuth flow
- Reviewed PR #123 (approved), PR #456 (changes requested)
- 5 commits in backend-api
- [From Claude Code] Fix authentication timeout bug
- [From Claude Code] Implement OAuth flow
- [From Claude Code] Research payment providers

Anything else you worked on yesterday that I missed?"
User: "No, that covers it"

Claude: "What will you work on today?"
User: "Continue the OAuth feature and deploy to staging"

Claude: "Any blockers?"
User: "No"

Claude: "Any topic to bring up at the end of the daily?"
User: "Yes, I want to discuss the architecture of the new payments module"

[Claude generates update]
```

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

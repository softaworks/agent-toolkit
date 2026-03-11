---
name: frontend-to-backend-requirements
description: Generate structured backend requirement documents that capture frontend data needs, UI states, and business rules as collaborative requests. Use when frontend needs to communicate API requirements to backend, user says 'backend requirements', 'what data do I need', 'API requirements', or is describing data needs for a UI. Outputs a requirements markdown file per feature.
---

# Backend Requirements Mode

You are a frontend developer generating a structured requirements document that captures what data you need from backend. You describe the **what**, not the **how**. Backend owns implementation details.

> **No Chat Output**: ALL responses go to `.claude/docs/ai/<feature-name>/backend-requirements.md`
> **No Implementation Details**: Don't specify endpoints, field names, or API structure—that's backend's call.

## What You Own vs. What Backend Owns

| Frontend Owns | Backend Owns |
|---------------|--------------|
| What data is needed | How data is structured |
| What actions exist | Endpoint design |
| UI states to handle | Field names, types |
| User-facing validation | API conventions |
| Display requirements | Performance/caching |

## Workflow

### Step 1: Describe the Feature

Before listing requirements:

1. **What is this?** — Screen, flow, component
2. **Who uses it?** — User type, permissions
3. **What's the goal?** — What does success look like?

### Step 2: List Data Needs

For each screen/component, describe:

**Data I need to display:**
- What information appears on screen?
- What's the relationship between pieces?
- What determines visibility/state?

**Actions user can perform:**
- What can the user do?
- What's the expected outcome?
- What feedback should they see?

**States I need to handle:**
- Loading, empty, error, success
- Edge cases (partial data, expired, etc.)

### Step 3: Surface Uncertainties and Invite Discussion

List what you're unsure about, then end with open questions that invite backend collaboration:

- Business rules you don't fully understand
- Edge cases you're not sure how to handle
- Places where you're guessing
- Questions like "Would it make sense to...?", "Should I expect...?", "Is there a simpler way to...?"

Frame uncertainties as invitations: "Push back if this complicates things unnecessarily" or "Open to suggestions on a better approach."

## Output Format

Create `.claude/docs/ai/<feature-name>/backend-requirements.md`:

```markdown
# Backend Requirements: <Feature Name>

## Context
[What we're building, who it's for, what problem it solves]

## Screens/Components

### <Screen/Component Name>
**Purpose**: What this screen does

**Data I need to display**:
- [Description of data piece, not field name]
- [Another piece]
- [Relationships between pieces]

**Actions**:
- [Action description] → [Expected outcome]
- [Another action] → [Expected outcome]

**States to handle**:
- **Empty**: [When/why this happens]
- **Loading**: [What's being fetched]
- **Error**: [What can go wrong, what user sees]
- **Special**: [Any edge cases]

**Business rules affecting UI**:
- [Rule that changes what's visible/enabled]
- [Permissions that affect actions]

### <Next Screen/Component>
...

## Uncertainties
- [ ] Not sure if [X] should show when [Y]
- [ ] Don't understand the business rule for [Z]
- [ ] Guessing that [A] means [B]

## Questions for Backend
- Would it make sense to combine [X] and [Y]?
- Should I expect [Z] to always be present?
- Is there existing data I can reuse for [W]?

## Discussion Log
[Backend responses, decisions made, changes to requirements]
```

## Examples

**Bad** — Dictating implementation:
> "I need a GET /api/contracts endpoint that returns an array with fields: id, title, status, created_at"

**Good** — Describing needs with context:
> "On the dashboard, there's a 'Recent Contracts' widget showing the 5 most recent contracts. Each item shows the contract title, its current status, and when it was created. User should be able to filter by status and clicks one to go to detail page."

**Bad** — Assuming structure:
> "The provider object should be nested inside the contract response"

**Good** — Describing the relationship:
> "For each contract, I need to show who the provider is (their name and maybe logo)"

## Rules

- **DESCRIBE, DON'T PRESCRIBE** — say what you need, not how to provide it. No endpoints, methods, or field names.
- **INCLUDE CONTEXT** — why you need it helps backend make better choices
- **SURFACE UNKNOWNS** — don't hide confusion, invite clarification
- **INVITE PUSHBACK** — explicitly ask for backend's input
- **UPDATE THE DOC** — when backend responds, add to Discussion Log, adjust requirements, mark resolved uncertainties, and note decisions made. The doc becomes the source of truth.

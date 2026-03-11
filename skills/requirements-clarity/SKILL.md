---
name: requirements-clarity
description: Transform vague requirements into actionable PRDs through structured clarification dialogue. Use when someone says "build me a feature", "I need a dashboard", "scope this project", "spec out", "what should I build", or before starting any complex feature (>2 days effort, cross-team, or unclear acceptance criteria). Scores clarity 0-100 and iterates until requirements are implementation-ready.
---

# Requirements Clarity Skill

Transform vague feature requests into concrete, implementation-ready PRDs through iterative clarification dialogue scored on a 100-point rubric.

## When to Activate

Activate when requirements lack specific acceptance criteria, technical constraints, or clear scope boundaries — e.g., "add login feature", "implement payment", "create dashboard", or any request missing the "how" and "what exactly."

Do NOT activate when the user provides specific file paths, code snippets, existing function references, or bug fixes with clear reproduction steps.

## Workflow

### Step 1: Assess Initial Clarity (Score 0-100)

Parse the requirement, generate a kebab-case feature name, and score against this rubric:

| Category | Points | Criteria |
|----------|--------|----------|
| Functional Clarity | /30 | Clear inputs/outputs (10), user interaction defined (10), success criteria stated (10) |
| Technical Specificity | /25 | Tech stack mentioned (8), integration points identified (8), constraints specified (9) |
| Implementation Completeness | /25 | Edge cases considered (8), error handling mentioned (9), data validation specified (8) |
| Business Context | /20 | Problem statement clear (7), target users identified (7), success metrics defined (6) |

Respond with the current score, what is clear, and what needs clarification.

### Step 2: Iterative Clarification

Ask 2-3 focused questions per round, prioritizing highest-impact gaps across four dimensions:

1. **Functional Scope** — core functionality, boundaries, out-of-scope, edge cases
2. **User Interaction** — inputs, outputs, success/failure scenarios
3. **Technical Constraints** — performance, compatibility, security, scalability
4. **Business Value** — problem solved, target users, success metrics, priority

After each response, update the score and summarize new information. Continue until score reaches 90+.

**Example Clarification Round:**

> User: "I need a payment integration"
>
> **Clarity Score: 15/100**
>
> I need to clarify a few things:
> 1. **Functional Scope**: Which payment provider(s) — Stripe, PayPal, or something else? One-time payments, subscriptions, or both?
> 2. **Technical Constraints**: What's your current stack? Any PCI compliance requirements already in place?
> 3. **Business Value**: What's the primary use case — e-commerce checkout, SaaS billing, or marketplace payouts?

### Step 3: Generate PRD

Once score reaches 90+, write the PRD to `./docs/prds/{feature_name}-v{version}-prd.md` using the `Write` tool. The PRD must include:

- **Requirements Description**: background/problem, feature overview with boundaries, detailed requirements with inputs/outputs, edge cases, and data validation
- **Design Decisions**: technical approach and rationale, key components, constraints (performance, security, scalability), risk assessment
- **Acceptance Criteria**: checklistable items (`- [ ]` format) covering functional, quality, and user acceptance
- **Execution Phases**: concrete tasks organized into preparation, core development, integration/testing, and deployment phases with time estimates

Close the PRD with document version (default `1.0`), creation timestamp, number of clarification rounds, and final quality score.

## Success Criteria

- Clarity score reaches 90+/100 before PRD generation
- All PRD sections contain substantive, specific content (no placeholders)
- Acceptance criteria are checklistable and measurable
- Execution phases have concrete tasks ready for development handoff

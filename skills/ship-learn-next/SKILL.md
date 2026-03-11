---
name: ship-learn-next
description: >
  Convert learning content into shippable action plans using the Ship-Learn-Next framework.
  Use when the user says "turn this into a plan", "make this actionable", "I watched/read X, now what?",
  or wants to extract implementation steps from a transcript, article, tutorial, or course notes.
allowed-tools: Read, Write
---

# Ship-Learn-Next Action Planner

Transform passive learning content into actionable **Ship-Learn-Next cycles** — turning advice and lessons into concrete, shippable iterations.

**Core principle**: 100 reps beats 100 hours of study. Learning = doing better, not knowing more.

## Workflow

### 1. Read and extract actionable lessons

Read the user-provided file (transcript, article, notes) and identify:

- **Actionable principles** that can be practiced
- **Concrete techniques** or workflows mentioned
- **Examples/case studies** to replicate

Skip theory-only content — focus on what can be turned into action.

### 2. Define the quest

Ask the user to frame a 4-8 week goal:

1. "Based on this content, what do you want to achieve?"
2. "What would success look like? Be specific."
3. "What's something concrete you could build/create/ship?"

Good quest: "Ship 10 cold outreach messages and get 2 responses"
Bad quest: "Learn about sales" (too vague — push for specifics)

### 3. Design Rep 1

Break the quest into the **smallest shippable version**:

- Completable in 1-7 days
- Produces a real artifact (code, content, product)
- Small enough to start today, big enough to learn something

Ask: "What's the smallest version you could ship THIS WEEK?"

### 4. Create the rep plan

Structure each rep using this template:

```markdown
## Rep 1: [Specific Goal]

**Ship Goal**: [Concrete deliverable]
**Timeline**: [This week / By date]
**Success Criteria**:
- [ ] [Measurable outcome 1]
- [ ] [Measurable outcome 2]

**What You'll Practice** (from the source content):
- [Skill/concept from source material]

**Action Steps**:
1. [Concrete step]
2. [Concrete step]
3. Ship it (publish/deploy/share/demonstrate)

**After Shipping — Reflection**:
- What actually happened?
- What worked? What didn't?
- Rate this rep: _/10
- What's one thing to try differently next time?
```

### 5. Map future reps (2-5)

Suggest a progression where each rep adds ONE new element. Reference specific lessons from the source content. Keep all reps shippable, not theoretical.

```markdown
## Rep 2: [Next level]
**Builds on**: Rep 1 learnings
**New challenge**: [One new element]
**Ship goal**: [Concrete deliverable]
```

### 6. Save and present

Save the complete plan as `Ship-Learn-Next Plan - [Brief Quest Title].md`.

Then:
1. Confirm the file was saved
2. Highlight Rep 1 and its deadline
3. Ask: "When will you ship Rep 1?" and "What might stop you?"

## Content-type guidelines

| Content Type | Focus On | Skip |
|---|---|---|
| YouTube transcripts | Concrete techniques, case studies to replicate | Stories, filler |
| Articles/tutorials | Workflows, "do this" sections | Pure theory |
| Course notes | Smallest project, modules needed for rep 1 | Everything else |

## Conversation style

- **Direct but supportive**: "Ship it, then we'll improve it"
- **Question-driven**: Make them think rather than prescribing
- **Specific**: "By Friday, ship one landing page" not "Learn web development"
- **Action-oriented**: Always end with "what's next?"

## Constraints

- Never create a study plan — create a SHIP plan
- Never accept vague goals — push "learn X" toward "ship Y by Z date"
- Never overwhelm with the full journey — focus on rep 1 first
- Pick minimal resources for the current rep only

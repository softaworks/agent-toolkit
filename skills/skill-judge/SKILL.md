---
name: skill-judge
description: Evaluate Agent Skill SKILL.md files against official specifications and best practices. Use when reviewing, auditing, scoring, or improving skill quality. Validates YAML frontmatter structure, checks description trigger terms, evaluates knowledge delta, scores progressive disclosure and file organization. Provides 8-dimension scoring (120 points) and actionable improvement suggestions for skill packages.
---

# Skill Judge

Evaluate Agent Skills against official specifications and patterns derived from 17+ official examples. Produces an 8-dimension score (120 points) with actionable improvement guidance.

---

## Core Principle

> **Good Skill = Expert-only Knowledge − What Claude Already Knows**

A Skill's value is its **knowledge delta**. Categorize every section:

| Type | Definition | Treatment |
|------|------------|-----------|
| **Expert** | Claude genuinely doesn't know this | Must keep — the Skill's value |
| **Activation** | Claude knows but may not think of | Keep if brief — serves as reminder |
| **Redundant** | Claude definitely knows this | Delete — wastes tokens |

Target ratio: >70% Expert, <20% Activation, <10% Redundant.

---

## Evaluation Dimensions (120 points total)

### D1: Knowledge Delta (20 points) — THE CORE DIMENSION

| Score | Criteria |
|-------|----------|
| 0-5 | Explains basics Claude knows (what is X, standard tutorials) |
| 6-10 | Mixed: some expert knowledge diluted by obvious content |
| 11-15 | Mostly expert knowledge with minimal redundancy |
| 16-20 | Pure knowledge delta — every paragraph earns its tokens |

**Red flags** (instant ≤5): "What is [basic concept]" sections, standard library tutorials, generic best practices.
**Green flags**: Decision trees for non-obvious choices, expert-only trade-offs, edge cases from real-world experience, "NEVER do X because [non-obvious reason]".

---

### D2: Mindset + Appropriate Procedures (15 points)

| Score | Criteria |
|-------|----------|
| 0-3 | Only generic procedures Claude already knows |
| 4-7 | Has domain procedures but lacks thinking frameworks |
| 8-11 | Good balance: thinking patterns + domain-specific workflows |
| 12-15 | Expert-level: shapes thinking AND provides procedures Claude wouldn't know |

**Valuable**: Thinking patterns ("Before X, ask yourself..."), domain-specific sequences Claude hasn't been trained on, non-obvious ordering, critical easy-to-miss steps.
**Redundant**: Generic file operations, standard programming patterns, common well-documented library usage.

---

### D3: Anti-Pattern Quality (15 points)

| Score | Criteria |
|-------|----------|
| 0-3 | No anti-patterns mentioned |
| 4-7 | Generic warnings ("avoid errors", "be careful") |
| 8-11 | Specific NEVER list with some reasoning |
| 12-15 | Expert-grade anti-patterns with WHY — things only experience teaches |

**The test**: Would an expert say "I learned this the hard way"? Or "this is obvious to everyone"?

---

### D4: Specification Compliance — Especially Description (15 points)

| Score | Criteria |
|-------|----------|
| 0-5 | Missing frontmatter or invalid format |
| 6-10 | Has frontmatter but description is vague or incomplete |
| 11-13 | Valid frontmatter, description has WHAT but weak on WHEN |
| 14-15 | Perfect: comprehensive description with WHAT, WHEN, and trigger keywords |

**Why description is critical**: The Agent sees ONLY skill descriptions when deciding which to activate. A Skill with perfect content but poor description is useless — it never gets loaded.

**Description must answer**:
1. **WHAT**: What does this Skill do? (specific capabilities)
2. **WHEN**: In what situations should it be used? (trigger scenarios)
3. **KEYWORDS**: What terms should trigger this Skill? (searchable terms)

**Checklist**:
- [ ] Lists specific capabilities (not just "helps with X")
- [ ] Includes explicit trigger scenarios ("Use when...", "When user asks for...")
- [ ] Contains searchable keywords (file extensions, domain terms, action verbs)
- [ ] Specific enough that Agent knows EXACTLY when to use it

---

### D5: Progressive Disclosure (15 points)

Three loading layers:
- **Layer 1**: Metadata (always in memory) — name + description only (~100 tokens)
- **Layer 2**: SKILL.md body (loaded after triggering) — ideal < 500 lines
- **Layer 3**: Resources (loaded on demand) — scripts/, references/, assets/

| Score | Criteria |
|-------|----------|
| 0-5 | Everything dumped in SKILL.md (>500 lines, no structure) |
| 6-10 | Has references but unclear when to load them |
| 11-13 | Good layering with MANDATORY triggers present |
| 14-15 | Perfect: decision trees + explicit triggers + "Do NOT Load" guidance |

**Good loading trigger** (embedded in workflow):
```markdown
**MANDATORY - READ ENTIRE FILE**: Before proceeding, you MUST read
[`docx-js.md`](docx-js.md) completely. **Do NOT load** `ooxml.md` for this task.
```

**For simple Skills** (no references, <100 lines): Score based on conciseness and self-containment.

---

### D6: Freedom Calibration (15 points)

Match constraint level to task fragility:

| Task Type | Freedom Level | Why |
|-----------|---------------|-----|
| Creative/Design | High (principles, not steps) | Multiple valid approaches |
| Code review | Medium (priorities + judgment) | Principles exist but judgment required |
| File format operations | Low (exact scripts) | One wrong byte corrupts file |

| Score | Criteria |
|-------|----------|
| 0-5 | Severely mismatched (rigid for creative, vague for fragile) |
| 6-10 | Partially appropriate |
| 11-13 | Good calibration for most scenarios |
| 14-15 | Perfect freedom calibration throughout |

---

### D7: Pattern Recognition (10 points)

Five official patterns identified across 17 Skills:

| Pattern | ~Lines | When to Use |
|---------|--------|-------------|
| **Mindset** | ~50 | Creative tasks requiring taste |
| **Navigation** | ~30 | Multiple distinct scenarios |
| **Philosophy** | ~150 | Art/creation requiring originality |
| **Process** | ~200 | Complex multi-step projects |
| **Tool** | ~300 | Precise operations on specific formats |

| Score | Criteria |
|-------|----------|
| 0-3 | No recognizable pattern, chaotic structure |
| 4-6 | Partially follows a pattern with significant deviations |
| 7-8 | Clear pattern with minor deviations |
| 9-10 | Masterful application of appropriate pattern |

---

### D8: Practical Usability (15 points)

| Score | Criteria |
|-------|----------|
| 0-5 | Confusing, incomplete, contradictory, or untested guidance |
| 6-10 | Usable but with noticeable gaps |
| 11-13 | Clear guidance for common cases |
| 14-15 | Comprehensive coverage including edge cases and error handling |

**Check for**: Decision trees for multi-path scenarios, working code examples, error handling/fallbacks, edge case coverage, immediate actionability.

---

## NEVER Do When Evaluating

- **NEVER** give high scores just because it "looks professional" or is well-formatted
- **NEVER** ignore token waste — every redundant paragraph should result in deduction
- **NEVER** let length impress you — a 43-line Skill can outperform a 500-line Skill
- **NEVER** skip mentally testing decision trees — do they lead to correct choices?
- **NEVER** forgive explaining basics with "but it provides helpful context"
- **NEVER** overlook missing anti-patterns — no NEVER list is a significant gap
- **NEVER** assume all procedures are valuable — distinguish domain-specific from generic
- **NEVER** undervalue the description field — poor description = skill never gets used
- **NEVER** put "when to use" info only in the body — Agent only sees description before loading

---

## Evaluation Protocol

### Step 1: Knowledge Delta Scan

Read SKILL.md completely. Mark each section as **[E] Expert**, **[A] Activation**, or **[R] Redundant**. Calculate the E:A:R ratio.

### Step 2: Structure Analysis

Check frontmatter validity, count lines, list reference files and sizes, identify pattern, check loading triggers.

### Step 3: Score Each Dimension

For each of the 8 dimensions: find specific evidence, assign score with justification, note improvements if below max.

### Step 4: Calculate Total & Grade

| Grade | Percentage | Meaning |
|-------|------------|---------|
| A | 90%+ (108+) | Excellent — production-ready expert Skill |
| B | 80-89% (96-107) | Good — minor improvements needed |
| C | 70-79% (84-95) | Adequate — clear improvement path |
| D | 60-69% (72-83) | Below Average — significant issues |
| F | <60% (<72) | Poor — needs fundamental redesign |

### Step 5: Generate Report

```markdown
# Skill Evaluation Report: [Skill Name]

## Summary
- **Total Score**: X/120 (X%)
- **Grade**: [A/B/C/D/F]
- **Pattern**: [Mindset/Navigation/Philosophy/Process/Tool]
- **Knowledge Ratio**: E:A:R = X:Y:Z
- **Verdict**: [One sentence assessment]

## Dimension Scores

| Dimension | Score | Max | Notes |
|-----------|-------|-----|-------|
| D1: Knowledge Delta | X | 20 | |
| D2: Mindset vs Mechanics | X | 15 | |
| D3: Anti-Pattern Quality | X | 15 | |
| D4: Specification Compliance | X | 15 | |
| D5: Progressive Disclosure | X | 15 | |
| D6: Freedom Calibration | X | 15 | |
| D7: Pattern Recognition | X | 10 | |
| D8: Practical Usability | X | 15 | |

## Critical Issues
[Must-fix problems that significantly impact effectiveness]

## Top 3 Improvements
1. [Highest impact improvement with specific guidance]
2. [Second priority improvement]
3. [Third priority improvement]
```

---

## References

**MANDATORY** — When encountering a recurring failure pattern during evaluation, read [`references/failure-patterns.md`](references/failure-patterns.md) for detailed diagnosis and fix guidance covering 9 common anti-patterns (The Tutorial, The Dump, The Orphan References, The Checkbox Procedure, The Vague Warning, The Invisible Skill, The Wrong Location, The Over-Engineered, The Freedom Mismatch).

**MANDATORY** — For quick validation during Step 2 (Structure Analysis), read [`references/checklist.md`](references/checklist.md) for a comprehensive per-dimension checkbox reference.

**Do NOT load** reference files for simple evaluations where the skill clearly follows good patterns and scores above 80%.

---

## The Meta-Question

> **"Would an expert in this domain say: 'Yes, this captures knowledge that took me years to learn'?"**

If yes — the Skill has genuine value. If no — it's compressing what Claude already knows.

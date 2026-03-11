---
name: qa-test-planner
description: Generate test plans, manual test cases, regression suites, and bug reports. Use when the user asks for QA documentation, testing strategies, test scenarios, acceptance criteria, test coverage plans, or needs to validate UI implementations against Figma designs. Trigger phrases include "write test cases", "create a test plan", "build regression suite", "QA this feature", "quality assurance", and "compare with Figma design".
---

# QA Test Planner

Generate structured QA deliverables: test plans, manual test cases, regression suites, Figma design validation checklists, and bug reports.

## Workflow

1. **Analyze** the feature or requirement:
   - Identify what test types are needed (functional, UI, integration, regression)
   - Determine scope boundaries (what is in/out of testing)
   - Assess risk areas and priorities
2. **Generate** structured deliverables using the templates below
3. **Validate** output completeness:
   - Every test step has an expected result
   - Preconditions and test data are documented
   - Priority is assigned to each test case
   - If validation fails, revise before delivering

## Scripts

| Script | Purpose | Example |
|--------|---------|---------|
| `./scripts/generate_test_cases.sh` | Create test cases interactively | Run, then follow prompts for feature name, scope, and priority |
| `./scripts/create_bug_report.sh` | Generate structured bug reports | Run, then provide reproduction steps, environment, and severity |

## Quick Reference

| Request | Output |
|---------|--------|
| "Create test plan for {feature}" | Complete test plan with scope, strategy, risks, entry/exit criteria |
| "Generate {N} test cases for {feature}" | Numbered test cases with steps and expected results |
| "Build smoke test suite" | P0 critical-path tests (15-30 min execution) |
| "Compare with Figma at {URL}" | Component-by-component visual validation checklist |
| "Document bug: {description}" | Structured bug report with reproduction steps |

## Anti-Patterns

| Avoid | Instead |
|-------|---------|
| Vague test steps | Specific actions + expected results |
| Missing preconditions | Document all setup requirements |
| No test data | Provide sample data or generation instructions |
| Generic bug titles | Specific: "[Feature] issue when [action]" |
| Skipping edge cases | Include boundary values, nulls, empty states |

## Verification Checklist

After generating any deliverable, verify:

- [ ] **Test plans**: Scope defined (in/out), entry/exit criteria specified, risks identified with mitigations
- [ ] **Test cases**: Each step has expected result, preconditions documented, test data available, priority assigned
- [ ] **Bug reports**: Reproducible steps, environment documented, evidence attached, severity/priority set
- [ ] **Figma validation**: Design specs extracted, implementation compared per-component, discrepancies logged as bugs

---

## References

- [Test Case Templates](references/test_case_templates.md) - Standard formats for all test types
- [Bug Report Templates](references/bug_report_templates.md) - Documentation templates
- [Regression Testing Guide](references/regression_testing.md) - Suite building and execution
- [Figma Validation Guide](references/figma_validation.md) - Design-implementation validation

---

<details>
<summary><strong>Template: Test Case</strong></summary>

```markdown
## TC-001: [Test Case Title]

**Priority:** High | Medium | Low
**Type:** Functional | UI | Integration | Regression
**Status:** Not Run | Pass | Fail | Blocked

### Preconditions
- [Setup requirement 1]
- [Test data needed]

### Test Steps
1. [Action to perform]
   **Expected:** [What should happen]
2. [Action to perform]
   **Expected:** [What should happen]

### Test Data
- Input: [Test data values]
- User: [Test account details]

### Post-conditions
- [System state after test]
- [Cleanup required]
```

</details>

<details>
<summary><strong>Template: Test Plan</strong></summary>

```markdown
# Test Plan: [Feature/Release Name]

## Executive Summary
- Feature being tested, objectives, key risks, timeline

## Test Scope
**In Scope:** Features, test types, platforms, user flows
**Out of Scope:** Features excluded, known limitations

## Test Strategy
- Test types: Manual, exploratory, regression, integration, UAT
- Approach: Black box, positive/negative, boundary value analysis

## Test Environment
- OS, browsers, devices, test data, backend environments

## Entry Criteria
- [ ] Requirements documented
- [ ] Test environment ready
- [ ] Build deployed

## Exit Criteria
- [ ] All high-priority test cases executed
- [ ] 90%+ pass rate
- [ ] No open critical bugs

## Risk Assessment
| Risk | Probability | Impact | Mitigation |
|------|-------------|--------|------------|
| [Risk 1] | H/M/L | H/M/L | [Mitigation] |
```

</details>

<details>
<summary><strong>Template: Bug Report</strong></summary>

```markdown
# BUG-[ID]: [Clear, specific title]

**Severity:** Critical | High | Medium | Low
**Priority:** P0 | P1 | P2 | P3

## Environment
- **OS:** [Windows 11, macOS 14, etc.]
- **Browser:** [Chrome 120, Firefox 121, etc.]
- **Build:** [Version/commit]

## Steps to Reproduce
1. [Specific step]
2. [Specific step]

## Expected Behavior
[What should happen]

## Actual Behavior
[What actually happens]

## Impact
- **Frequency:** [Always, Sometimes, Rarely]
- **Workaround:** [If one exists]
- **Figma design:** [Link if UI bug]
```

</details>

<details>
<summary><strong>Figma MCP Validation Process</strong></summary>

**Prerequisites:** Figma MCP server configured, access to design files.

**Step 1 — Extract design specs:**
```
"Get the button specifications from Figma file [URL]"
→ Returns: dimensions, colors, typography, spacing, border radius, states
```

**Step 2 — Compare implementation against specs:**
```
TC: Primary Button Visual Validation
1. Inspect primary button in browser dev tools
2. Compare: dimensions (120x40px), border-radius (8px), background (#0066FF), font (16px Medium #FFFFFF)
3. Document discrepancies
```

**Step 3 — File bug if mismatch:**
```
BUG: Primary button color doesn't match design
Expected (Figma): #0066FF | Actual: #0052CC
```

**What to validate per component:** Colors (exact hex), spacing (padding/margin px), typography (font/size/weight), layout (width/height), interactive states (hover/active/focus), responsive breakpoints.

</details>

<details>
<summary><strong>Regression Suite Building</strong></summary>

| Suite Type | Duration | When to Run |
|------------|----------|-------------|
| Smoke | 15-30 min | Daily / every deploy |
| Targeted | 30-60 min | Per feature change |
| Full | 2-4 hours | Weekly / pre-release |

**Build process:**
1. Identify critical paths (revenue, security, high-usage features)
2. Assign priorities: P0 (always run), P1 (weekly+), P2 (releases only)
3. Execute in order: Smoke → P0 → P1 → P2 → Exploratory
4. If any P0 fails → stop execution, fix build, restart

**Release gate:** All P0 pass, 90%+ P1 pass, no critical bugs open. If P1 failures have workarounds and fix plan exists → conditional pass.

</details>

---

<details>
<summary><strong>Example: Login Flow Test Case</strong></summary>

```markdown
## TC-LOGIN-001: Valid User Login

**Priority:** P0 (Critical)  |  **Type:** Functional

### Preconditions
- User account exists (test@example.com / Test123!)
- User is not logged in, browser cookies cleared

### Test Steps
1. Navigate to https://app.example.com/login
   **Expected:** Login page displays with email and password fields
2. Enter email: test@example.com
   **Expected:** Email field accepts input
3. Enter password: Test123!
   **Expected:** Password field shows masked characters
4. Click "Login" button
   **Expected:** Loading indicator → redirect to /dashboard → "Welcome back, Test User" shown

### Post-conditions
- User session created, auth token stored

### Related Edge Cases
- TC-LOGIN-002: Invalid password → error message shown
- TC-LOGIN-003: Non-existent email → "account not found"
- TC-LOGIN-004: SQL injection attempt → input sanitized
```

</details>

<details>
<summary><strong>Example: Figma Responsive Validation</strong></summary>

```markdown
## TC-UI-045: Mobile Navigation Menu

**Priority:** P1 (High)  |  **Type:** UI/Responsive

### Preconditions
- Viewport width: 375px–428px (mobile range)

### Test Steps
1. Open homepage on mobile device
   **Expected:** Hamburger menu icon visible (top-right)
2. Tap hamburger icon
   **Expected:** Menu slides in from right, overlay appears, close (X) button visible
3. Compare against Figma mobile design [link]
   **Expected:** Menu width 280px, slide animation 300ms ease-out, overlay opacity 0.5 #000000, font 16px/24px

### Breakpoints to Test
375px (iPhone SE), 390px (iPhone 14), 428px (iPhone Pro Max), 360px (Galaxy S21)
```

</details>

---
name: reducing-entropy
description: "Use when the user asks to reduce code, clean up codebase, remove dead code, shrink footprint, or minimize code size. Manual-only — only activate when explicitly requested. Removes dead code, consolidates duplicate functions, eliminates unused dependencies, and simplifies over-engineered abstractions. Measures success by final line count, not effort. Bias toward deletion."
---

# Reducing Entropy

More code begets more code. Entropy accumulates. This skill biases toward the smallest possible codebase.

**Core question:** "What does the codebase look like *after*?"

## Before You Begin

**Load at least one mindset from `references/`**

1. List the files in the reference directory
2. Read frontmatter descriptions to pick which applies
3. Load at least one
4. State which you loaded and its core principle

**Do not proceed until you've done this.**

## The Goal

The goal is **less total code in the final codebase** - not less code to write right now.

- Writing 50 lines that delete 200 lines = net win
- Keeping 14 functions to avoid writing 2 = net loss
- "No churn" is not a goal. Less code is the goal.

**Measure the end state, not the effort.**

## Three Questions

### 1. What's the smallest codebase that solves this?

Not "what's the smallest change" - what's the smallest *result*.

- Could this be 2 functions instead of 14?
- Could this be 0 functions (delete the feature)?
- What would we delete if we did this?

### 2. Does the proposed change result in less total code?

Count lines before and after. If after > before, reject it.

- "Better organized" but more code = more entropy
- "More flexible" but more code = more entropy
- "Cleaner separation" but more code = more entropy

### 3. What can we delete?

Every change is an opportunity to delete. Ask:

- What does this make obsolete?
- What was only needed because of what we're replacing?
- What's the maximum we could remove?

## Red Flags

- **"Keep what exists"** - Status quo bias. The question is total code, not churn.
- **"This adds flexibility"** - Flexibility for what? YAGNI.
- **"Better separation of concerns"** - More files/functions = more code. Separation isn't free.
- **"Type safety"** - Worth how many lines? Sometimes runtime checks in less code wins.
- **"Easier to understand"** - 14 things are not easier than 2 things.

## Reduction Workflow

1. **Measure** — count lines before starting:
   ```bash
   find . -name '*.ts' -o -name '*.js' | xargs wc -l | tail -1
   ```
2. **Identify** dead code targets:
   - Unused exports (no importers)
   - Unreachable branches or feature-flagged dead paths
   - Orphaned files with no references
   - Duplicate functions doing the same thing with slight variations
3. **Delete or consolidate** — apply changes
4. **Verify** — run tests to confirm nothing breaks:
   ```bash
   npm test  # or the project's equivalent
   ```
5. **Measure again** — confirm the line count dropped. If it didn't, reconsider.

## Concrete Example

A module has 14 helper functions across 3 files (280 lines) for API response formatting:

- **Before:** `formatUser()`, `formatPost()`, `formatComment()`, ... 14 functions, 280 lines
- **After:** 1 generic `formatEntity(schema)` + a schema map, 45 lines
- **Net result:** -235 lines. The 45 lines of new code deleted 280 lines of old code.

The question is not "is 45 lines a lot to write?" — it is "is the codebase smaller after?"

## When This Doesn't Apply

- The codebase is already minimal for what it does
- You're in a framework with strong conventions (don't fight it)
- Regulatory/compliance requirements mandate certain structures

## Reference Mindsets

See `references/` for philosophical grounding.

To add new mindsets, see `adding-reference-mindsets.md`.

---

**Bias toward deletion. Measure the end state.**

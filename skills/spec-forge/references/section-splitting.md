# Section File Writing

Write individual section files from the plan. This step assumes `sections/index.md` already exists.

## Input Files

- `<planning_dir>/claude-plan.md` - implementation details
- `<planning_dir>/sections/index.md` - section definitions and dependencies

## Output

```
<planning_dir>/sections/
├── index.md (already exists)
├── section-01-<name>.md
├── section-02-<name>.md
└── ...
```

## Writing Loop

For each section in the SECTION_MANIFEST:

```
┌─────────────────────────────────────────────────────┐
│  ITERATION LOOP                                     │
│                                                     │
│  1. Parse index.md to get section list              │
│  2. Check which sections already exist              │
│  3. For each missing section:                       │
│     a. Read claude-plan.md for relevant content     │
│     b. Write section-NN-name.md                     │
│     c. Mark TODO complete                           │
│  4. Continue until all sections written             │
│                                                     │
└─────────────────────────────────────────────────────┘
```

### Check Progress

Parse the SECTION_MANIFEST from index.md:

```markdown
<!-- SECTION_MANIFEST
section-01-foundation
section-02-config
section-03-api
END_MANIFEST -->
```

Then check which `section-*.md` files exist in the sections directory.

### Write Section Files

For each missing section:

1. Read `claude-plan.md` and `index.md`
2. Write `section-NN-<name>.md` with:
   - Full background context
   - Requirements for this section
   - Implementation details
   - Acceptance criteria

**CRITICAL: Each section file must be completely self-contained.**

The implementer reading a section file should NOT need to reference `claude-plan.md` or any other document. They should be able to:
1. Read the single section file
2. Create a TODO list
3. Start implementing immediately

Include all necessary background, requirements, and implementation details within each section.

### Section File Template

```markdown
# Section NN: {Section Name}

## Background

{Why this section exists, what problem it solves}

## Requirements

{What must be true when this section is complete}

## Dependencies

- Requires: {list of prior sections that must be complete}
- Blocks: {list of sections that depend on this one}

## Implementation Details

{Detailed implementation guidance}

### {Subsection 1}

{Details}

### {Subsection 2}

{Details}

## Acceptance Criteria

- [ ] {Criterion 1}
- [ ] {Criterion 2}
- [ ] {Criterion 3}

## Files to Create/Modify

- `path/to/file1.ts` - {description}
- `path/to/file2.ts` - {description}
```

### Mark Progress

After writing each section file, mark its corresponding TODO as completed via TodoWrite.

### Batching

You can write multiple sections before updating TODOs if that's more efficient. Just make sure to mark them all complete when done.

## Completion

All sections are complete when every section in the SECTION_MANIFEST has a corresponding `section-NN-name.md` file.

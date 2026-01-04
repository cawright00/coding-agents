# Create Plan Workflow

This workflow takes a validated spec document and generates a comprehensive, step-by-step implementation plan with bite-sized tasks.

## Objective

Transform a SPEC document into an actionable implementation plan that an engineer with zero codebase context could follow successfully.

---

## Step 1: Identify the Spec Document

**Do you have a spec document you'd like me to create a plan for?**

If you just completed a brainstorm session, I'll use the spec we just created.

Otherwise, please provide either:
- The spec filename (e.g., `SPEC-user-authentication-2026-01-03.md`)
- Or say "list specs" and I'll show available specs in your plans directory

---

## Step 2: Determine the Plans Directory

I will locate the plans directory using the same logic as the brainstorm workflow:

```bash
pwd
```

From this, I will identify:
1. The path to the `git` folder
2. The first folder after `git` (project name)
3. The plans directory: `<git-folder>/claude/<project-name>/plans/`

Then list available specs if needed:

```bash
ls -la <git-folder>/claude/<project-name>/plans/SPEC-*.md
```

---

## Step 3: Read and Analyze the Spec

I will read the spec document thoroughly:

```bash
cat <path-to-spec-file>
```

I will pay special attention to:
- **Acceptance Criteria** - These are rigid requirements agreed upon by the team
- **Testing Strategy** - Required approach for verification
- **Technology Decisions** - Approved libraries and frameworks (no deviations without permission)
- **Non-Goals** - What is explicitly out of scope

**Critical:** If anything in the spec is unclear or seems incomplete, I will ask for clarification before proceeding. I will not make assumptions or deviate from the spec without explicit permission.

---

## Step 4: Gather Codebase Context

Before writing the plan, I will understand the existing codebase:

```bash
ls -la
```

```bash
# Check for existing patterns, test setup, etc.
find . -name "*.test.*" -o -name "*.spec.*" | head -20
```

```bash
# Look at package.json or similar for dependencies
cat package.json 2>/dev/null || cat Cargo.toml 2>/dev/null || cat requirements.txt 2>/dev/null || echo "No manifest found"
```

This helps me write tasks that reference the correct files, patterns, and conventions.

---

## Step 5: Generate the Implementation Plan

I will create a plan document following this structure:

```markdown
# [Idea Title] Implementation Plan

**Spec:** [Link/reference to SPEC document]
**Created:** YYYY-MM-DD
**Goal:** [One sentence describing what this builds]

**Architecture:** [2-3 sentences about the approach]

**Tech Stack:** [Key technologies/libraries from spec's Technology Decisions]

---

## Prerequisites

Before starting, ensure:
- [ ] [Any setup steps, access requirements, etc.]
- [ ] [Dependencies to install]
- [ ] [Documentation to review]

---

## Tasks

### Task 1: [Short descriptive title]

**What:** [Clear description of what to do]

**Why:** [How this fits into the bigger picture]

**Files to touch:**
- `path/to/file1.ts` - [what changes]
- `path/to/file2.ts` - [what changes]

**Implementation:**
1. [Step-by-step instructions]
2. [Specific enough that someone unfamiliar could follow]
3. [Include code snippets where helpful]

**Tests:**
- [ ] [Specific test case 1]
- [ ] [Specific test case 2]
- [ ] [Edge case to cover]

**Verification:**
```bash
npm test -- --grep "relevant tests"
```

**Commit checkpoint:**
```bash
git add .
git commit -m "feat(scope): description of task 1"
```

---

### Task 2: [Short descriptive title]

[Same structure as Task 1]

---

[Continue for all tasks...]

---

## Final Verification

After all tasks are complete:

- [ ] All tests pass: `npm test`
- [ ] [Acceptance Criteria 1 from spec]
- [ ] [Acceptance Criteria 2 from spec]
- [ ] [Acceptance Criteria 3 from spec]
- [ ] No regressions in existing functionality
- [ ] Code reviewed / PR created

---

## Rollback Plan

If issues arise:
- [How to revert changes]
- [What to watch for]

---

## References

- Spec document: `SPEC-<idea-title>-YYYY-MM-DD.md`
- [Relevant documentation links]
- [Related code areas]
```

---

## Task Granularity Guidelines

Each task should be:
- **Bite-sized**: Completable in roughly 15-30 minutes
- **Self-contained**: Can be committed independently
- **Testable**: Has clear verification criteria
- **Ordered**: Dependencies flow naturally (earlier tasks don't depend on later ones)

**Every task must include tests unless:**
- It's purely configuration (and even then, consider smoke tests)
- It's documentation-only

**Testing approach:**
- Think carefully about what tests prove the task works
- Include both happy path and edge cases
- Run tests after each task to catch regressions early
- Reference the Testing Strategy from the spec

---

## Step 6: Review the Plan with User

Before saving, I will present the plan in sections and ask:
- Does the task breakdown make sense?
- Is the granularity appropriate?
- Are there any missing tasks or edge cases?
- Do the test cases align with the spec's Acceptance Criteria?

---

## Step 7: Save the Plan Document

Save to the same directory as the spec:

**Filename format:** `PLAN-<idea-title>-YYYY-MM-DD.md`

Using the same `<idea-title>` as the spec for easy association.

```bash
# Create/write the plan file
cat > "<git-folder>/claude/<project-name>/plans/PLAN-<idea-title>-YYYY-MM-DD.md" << 'EOF'
[plan content]
EOF
```

---

## Step 8: Commit the Plan

Commit to the claude repo:

```bash
git -C <git-folder>/claude add <project-name>/plans/PLAN-<idea-title>-YYYY-MM-DD.md
git -C <git-folder>/claude commit -m "docs: Add implementation plan for <idea-title>"
```

---

## Key Principles

- **Zero context assumption** - Write for an engineer who knows nothing about this codebase
- **Bite-sized tasks** - Small enough for frequent commits and easy checkpoints
- **Tests for everything** - Every code task needs corresponding test cases
- **Spec is sacred** - Acceptance Criteria and Testing Strategy are rigid requirements; ask before deviating
- **No silent decisions** - If something isn't in the spec, ask rather than assume
- **DRY / YAGNI** - Don't add tasks for things not in the spec
- **Clear verification** - Every task has explicit "how do I know it works" criteria

---

## After Planning

Once the plan is saved, I will ask:

**"Ready to begin implementation?"**

If yes, we can start working through the tasks systematically by calling the Execute Plan Workflow.

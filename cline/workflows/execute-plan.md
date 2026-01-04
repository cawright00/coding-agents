# Execute Plan Workflow

This workflow executes an implementation plan task by task, with checkpoints and verification at each step.

## Objective

Systematically work through a PLAN document, implementing each task, running tests, and committing progress—while stopping immediately if anything is unclear or broken.

---

## Step 1: Identify the Plan Document

**Do you have a plan document you'd like me to execute?**

If you just completed a Create Plan session, I'll use the plan we just created.

Otherwise, please provide either:
- The plan filename (e.g., `PLAN-user-authentication-2026-01-03.md`)
- Or say "list plans" and I'll show available plans in your plans directory

---

## Step 2: Locate the Plans Directory

I will find the plans directory:

```bash
pwd
```

From this, I will identify:
1. The path to the `git` folder
2. The first folder after `git` (project name)
3. The plans directory: `<git-folder>/claude/<project-name>/plans/`

Then list available plans if needed:

```bash
ls -la <git-folder>/claude/<project-name>/plans/PLAN-*.md
```

---

## Step 3: Create a Branch for This Plan

Extract the branch name from the plan filename (portion after `PLAN-` without `.md`):

**Example:** `PLAN-user-authentication-2026-01-03.md` → branch `user-authentication-2026-01-03`

```bash
# Ensure we're on main/master and up to date
git fetch origin
git checkout main || git checkout master
git pull

# Create and switch to the new branch
git checkout -b <branch-name>
```

Verify the branch was created:

```bash
git branch --show-current
```

---

## Step 4: Read and Critically Review the Plan

Read the entire plan:

```bash
cat <path-to-plan-file>
```

Before executing, I will critically review the plan for:

**Blockers (STOP and ask before proceeding):**
- Missing prerequisites or dependencies
- Unclear or ambiguous instructions
- Tasks that reference files or code that doesn't exist
- Contradictions within the plan
- Tasks that seem to violate the spec's Acceptance Criteria
- Missing test cases for code tasks

**Concerns (raise but may proceed with approval):**
- Task ordering that might cause issues
- Potential edge cases not covered
- Implementation approaches that might have better alternatives

**If I have blockers or concerns, I will present them now:**

> ⚠️ **Before we begin, I have some questions/concerns about this plan:**
> 
> 1. [Question or concern]
> 2. [Question or concern]
> 
> Should we address these before starting, or proceed as-is?

**If the plan looks solid, I will confirm:**

> ✅ **Plan reviewed. Ready to execute.**
> 
> - Total tasks: [N]
> - Estimated scope: [brief summary]
> - Starting with Task 1: [task title]
> 
> Shall I begin?

---

## Step 5: Execute Tasks One by One

For each task, I will:

### 5a. Announce the Task

> 🔨 **Starting Task [N]: [Task Title]**
> 
> **What:** [Brief description]
> **Files:** [Files to touch]

### 5b. Implement the Task

Follow the plan's instructions exactly. If the plan includes code snippets, use them. If it references specific files, modify those files.

### 5c. Run Tests

Execute the **exact verification command specified in the plan** for this task. The plan may specify:
- A specific test file or test suite
- A grep pattern for relevant tests
- Integration tests, e2e tests, or other test types
- Multiple verification steps

Follow whatever the plan says—do not simplify or substitute with a generic test command.

**If tests fail:** Examine the failure output and attempt to fix the code.

**Retry loop (up to 3 attempts):**

1. **Attempt 1:** Analyze the test failure, identify the root cause, fix the code, re-run tests
2. **Attempt 2:** If still failing, re-examine—perhaps I misunderstood the failure. Try a different fix approach, re-run tests
3. **Attempt 3:** Final attempt with fresh analysis of what's actually being tested vs. what the code does

**After each failed attempt, I will note:**

> 🔄 **Test fix attempt [1/2/3]**
> 
> **Failure:** [What's failing]
> **Analysis:** [Why I think it's failing]  
> **Fix:** [What I'm changing]

**If tests still fail after 3 attempts: STOP and ask for help.**

> ❌ **Task [N] verification failed after 3 attempts.**
> 
> **Test output:**
> ```
> [Latest test output]
> ```
> 
> **What I tried:**
> 1. [Attempt 1 summary]
> 2. [Attempt 2 summary]
> 3. [Attempt 3 summary]
> 
> **My analysis:** [What I think is going wrong]
> 
> **Options:**
> 1. You can provide guidance on the fix
> 2. We can skip this task and note it as blocked
> 3. We can revisit the plan—perhaps the test expectation is wrong
> 
> How would you like to proceed?

### 5d. Commit the Task

If tests pass, commit using the plan's commit message:

```bash
git add .
git commit -m "[commit message from plan]"
```

### 5e. Report Completion

> ✅ **Task [N] complete: [Task Title]**
> 
> **Changes made:**
> - [File 1]: [what changed]
> - [File 2]: [what changed]
> 
> **Tests:** All passing
> **Commit:** `[commit hash]` - [commit message]
> 
> **Progress:** [N] of [Total] tasks complete
> 
> Proceeding to Task [N+1]: [Next task title]...

---

## STOP Conditions

**I will STOP executing immediately and ask for help if:**

- ❌ Hit a blocker mid-task (missing dependency, file not found)
- ❌ Test fails after 3 fix attempts
- ❌ Instruction is unclear or ambiguous
- ❌ Plan has critical gaps I didn't catch in review
- ❌ I would need to deviate from the plan to proceed
- ❌ Something feels wrong or risky

**When stopped, I will clearly state:**

> 🛑 **Execution paused at Task [N]**
> 
> **Issue:** [Clear description of what went wrong]
> 
> **What I tried:** [If applicable]
> 
> **What I need:** [Specific question or guidance needed]

**I will NOT:**
- Guess at solutions when instructions are unclear
- Skip failing tests to proceed
- Make changes not specified in the plan
- Assume I know what the plan meant

---

## Step 6: Final Verification

After all tasks are complete, run the final verification from the plan:

```bash
# Full test suite
npm test

# Any other verification commands from the plan
```

Check off the Acceptance Criteria:

> 📋 **Final Verification Checklist:**
> 
> - [ ] All tests pass
> - [ ] [Acceptance Criteria 1]
> - [ ] [Acceptance Criteria 2]
> - [ ] [Acceptance Criteria 3]
> - [ ] No regressions

---

## Step 7: Push the Branch and Summary

Once all tasks are complete and verified, push the branch automatically:

```bash
git push -u origin <branch-name>
```

Then present the summary:

> 🎉 **Plan execution complete!**
> 
> **Branch:** `<branch-name>` (pushed to origin)
> **Commits:** [N] commits
> **Tasks completed:** [N] of [N]
> 
> **Summary of changes:**
> - [High-level summary point 1]
> - [High-level summary point 2]
> 
> **Would you like me to run the PR review workflow (`/pr-review.md`) to review the changes before creating a PR?**

---

## Key Principles

- **Plan is the source of truth** - Follow it exactly; don't improvise
- **Stop early, stop often** - Any confusion = pause and ask
- **Verify everything** - Tests must pass before committing
- **Transparent progress** - Report after every task
- **No silent failures** - If something breaks, surface it immediately
- **Small commits** - One commit per task for easy rollback
- **Ask, don't guess** - Ambiguity is a blocker, not a challenge to overcome creatively

---

## Recovery

If execution was stopped and you're returning to continue:

```bash
# Check current state
git status
git log --oneline -5

# See where we left off
cat <path-to-plan-file>
```

I will identify which tasks are complete (by checking commits) and resume from the next incomplete task.

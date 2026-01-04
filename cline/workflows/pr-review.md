# PR Review Workflow

This workflow performs a comprehensive code review of your current branch against the main branch, analyzing changes for quality, consistency, and potential issues.

## Objective

Review all changes in the current branch compared to `origin/main`, providing structured feedback categorized by severity to help identify issues before merging.

---

## Step 1: Fetch Latest Main and Gather Context

First, I will fetch the latest changes from origin and identify what we're reviewing.

```bash
git fetch origin
```

Then I will gather information about the current branch:

```bash
git branch --show-current
```

And get a summary of what's being reviewed:

```bash
git log origin/main..HEAD --oneline
```

---

## Step 2: Generate the Diff

I will run the following command to get the complete diff of changes:

```bash
git diff origin/main..HEAD
```

I will also get a list of changed files for reference:

```bash
git diff --name-status origin/main..HEAD
```

---

## Step 3: Analyze the Changes

I will now perform a comprehensive code review of the diff, acting as a Senior Code Reviewer with expertise in software architecture, design patterns, and best practices.

### Review Criteria

For each change, I will evaluate:

**Code Quality**
- Readability and maintainability
- Appropriate naming conventions
- Code organization and structure
- Duplication and DRY principles
- Proper error handling

**Architecture & Design**
- Consistency with existing patterns
- Appropriate abstractions
- Separation of concerns
- SOLID principles adherence

**Security**
- Input validation
- Authentication/authorization checks
- Sensitive data handling
- Injection vulnerabilities

**Performance**
- Algorithm efficiency
- Resource management
- Potential bottlenecks
- Memory considerations

**Testing**
- Test coverage for new code
- Edge cases considered
- Test quality and clarity

**Documentation**
- Code comments where needed
- API documentation updates
- README changes if applicable

---

## Step 4: Present Review Findings

I will present my findings organized by severity:

### 🔴 Critical Issues (Must Fix Before Merge)
Issues that could cause bugs, security vulnerabilities, or data loss. These block the PR.

### 🟠 Important Issues (Should Fix)
Significant problems that affect maintainability, performance, or code quality. Fix before proceeding.

### 🟡 Minor Issues (Nice to Have)
Style inconsistencies, minor improvements, or suggestions that would improve the code but aren't blocking.

### ✅ Strengths
Positive observations about the implementation - good patterns, clean code, or clever solutions worth highlighting.

---

## Step 5: Summary and Recommendation

I will provide:

1. **Overall Assessment**: A brief summary of the PR quality
2. **Recommendation**: One of the following:
   - ✅ **Ready to Merge** - No critical issues, minor issues are acceptable
   - ⚠️ **Needs Changes** - Important issues should be addressed first
   - ❌ **Significant Rework Needed** - Critical issues or fundamental problems exist

3. **Key Action Items**: Prioritized list of what needs attention

---

## Notes

- If I find unclear or ambiguous changes, I will ask for clarification
- If I disagree with my assessment, push back with technical reasoning
- For complex changes, I may suggest breaking into smaller PRs
- Consider requesting a second review for critical components

---

## After Review

Once issues are addressed, you can re-run this workflow to verify fixes:
```
/pr-review.md
```

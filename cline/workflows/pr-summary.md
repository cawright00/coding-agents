# PR Summary Generator Workflow

This workflow generates a professional PR title and summary by analyzing the changes in your current branch compared to main.

## Objective

Create a well-formatted, ready-to-use PR title and description that clearly communicates what changed and why.

---

## Step 1: Fetch Latest and Gather Context

First, I will fetch the latest changes from origin:

```bash
git fetch origin
```

Get the current branch name:

```bash
git branch --show-current
```

Get the commit history for context on what was done:

```bash
git --no-pager log origin/main..HEAD --format="%s%n%b" --reverse
```

---

## Step 2: Analyze the Changes

Get the complete diff:

```bash
git diff origin/main..HEAD
```

Get a summary of changed files:

```bash
git diff --stat origin/main..HEAD
```

List files by change type:

```bash
git diff --name-status origin/main..HEAD
```

---

## Step 3: Generate PR Title and Summary

Based on my analysis of the changes, I will generate a PR summary in the following format:

---

## PR Output

### Title

I will generate a concise, descriptive title following conventional commit style where appropriate:
- `feat: Add user authentication flow`
- `fix: Resolve race condition in data sync`
- `refactor: Simplify payment processing logic`
- `docs: Update API documentation for v2 endpoints`

### Summary

```markdown
## Description

[A 2-3 sentence high-level description of what this PR accomplishes and why]

## Changes

### [Category 1 - e.g., "New Features" or "Core Changes"]
- Brief description of change 1
- Brief description of change 2

### [Category 2 - e.g., "Bug Fixes" or "Refactoring"]
- Brief description of change 1
- Brief description of change 2

### [Category 3 - e.g., "Documentation" or "Tests"]
- Brief description of change 1

## Files Changed

| Type | File | Summary |
|------|------|---------|
| ✨ Added | `path/to/new/file.ts` | Brief purpose |
| 📝 Modified | `path/to/changed/file.ts` | What changed |
| 🗑️ Deleted | `path/to/removed/file.ts` | Why removed |

## Testing

[Description of how changes were tested, or "N/A" if not applicable]

## Notes

[Any additional context, breaking changes, migration steps, or deployment considerations - omit section if none]
```

---

## Formatting Guidelines

When generating the summary, I will:

- **Be concise**: Each bullet point should be one line
- **Focus on "what" and "why"**: Not implementation details
- **Group logically**: Related changes together under clear headings
- **Highlight impact**: Call out breaking changes, new dependencies, or configuration changes
- **Use clear language**: Avoid jargon; write for reviewers who may not have full context

---

## After Generation

The output will be formatted markdown that you can:
1. Copy directly into your PR description
2. Adjust or expand as needed
3. Use the title for your PR title

To regenerate after making more changes:
```
/pr-summary.md
```

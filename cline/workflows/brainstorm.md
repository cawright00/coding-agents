# Brainstorming Workflow

This workflow helps turn ideas into fully formed designs and specs through natural collaborative dialogue.

## Objective

Explore and refine an idea through structured conversation, then produce a validated design specification saved to the project's plans directory.

---

## Step 1: Ask for the Initial Idea

I will start by asking:

**What idea or feature would you like to brainstorm?**

Give me a brief description of what you're thinking about building, changing, or implementing. It can be rough - we'll refine it together.

---

## Step 2: Determine Output Location

While waiting for your response, I will determine where to save the final design document.

First, find the current working directory and locate the git folder structure:

```bash
pwd
```

I need to identify:
1. The path to the `git` folder (e.g., `/home/user1/git`)
2. The first folder after `git` in the current project path (e.g., `projectA`)

Then I will ensure the output directory exists:

```bash
# Example: mkdir -p /home/user1/git/claude/projectA/plans
# Actual path will be determined from pwd output
```

Two files will be saved:
- **Markdown:** `<git-folder>/claude/<project-name>/plans/SPEC-<idea-title>-YYYY-MM-DD.md`
- **Jira format:** `<git-folder>/claude/<project-name>/plans/JIRA-<idea-title>-YYYY-MM-DD.md`

---

## Step 3: Understand the Project Context

Before diving into questions, I will check out the current project state:

```bash
ls -la
```

```bash
cat README.md 2>/dev/null || echo "No README found"
```

```bash
git log --oneline -10 2>/dev/null || echo "Not a git repo or no commits"
```

I'll also look at relevant files, docs, or recent changes to understand the existing codebase.

---

## Step 4: Refine the Idea Through Questions

I will ask questions **one at a time** to understand:

- **Purpose**: What problem does this solve? Who benefits?
- **Constraints**: Technical limitations, timeline, dependencies?
- **Success criteria**: How will we know it works?
- **Scope**: What's in and what's explicitly out?
- **Technical considerations**: Libraries, frameworks, tools, patterns?

**Guidelines for my questions:**
- Prefer multiple choice when possible (easier to answer)
- Only one question per message
- If a topic needs more exploration, break it into multiple questions
- Stay focused on understanding, not implementation details yet

**Critical: Technology Decisions**
- **Never** make a library, framework, or tool selection without explicit user approval
- Always present technology options with trade-offs before proceeding
- If multiple options exist (e.g., test frameworks, ORMs, state management), present them as choices
- Include: what each option is good at, drawbacks, and my recommendation with reasoning
- Even if one option seems obvious, confirm it rather than assume

---

## Step 5: Explore Approaches

Once I understand the idea, I will:

1. Propose **2-3 different approaches** with trade-offs
2. Lead with my recommended option and explain why
3. Present options conversationally
4. Wait for your feedback before proceeding

Example format:
```
I see three ways we could approach this:

**Option A: [Name]** ⭐ Recommended
[Brief description]
- Pro: ...
- Con: ...

**Option B: [Name]**
[Brief description]
- Pro: ...
- Con: ...

**Option C: [Name]**
[Brief description]
- Pro: ...
- Con: ...

I'd recommend Option A because [reasoning]. What do you think?
```

---

## Step 6: Present the Design in Sections

Once we've agreed on an approach, I will present the design in **small sections (200-300 words each)**.

After each section, I will ask: **"Does this look right so far?"**

Sections will cover:
- **Overview**: High-level summary and goals
- **Architecture**: How components fit together
- **Technology Decisions**: Libraries, frameworks, and tools with rationale
- **Components**: Individual pieces and their responsibilities  
- **Data Flow**: How information moves through the system
- **Error Handling**: What can go wrong and how we handle it
- **Testing Strategy**: How we'll verify it works (including test framework choice)

I'll be ready to go back and clarify if something doesn't make sense.

---

## Step 7: Generate the Design Documents

Once the design is validated, I will create a title for the idea (short, descriptive, dash-separated).

**Naming the idea-title:**
- Use kebab-case (lowercase with dashes)
- Keep it short but descriptive
- **If a Jira ticket is mentioned, include it as the first part of the idea-title**
- Jira tickets look like `DS24-123` (project key + number)

**Examples:**
- User mentions "DS24-2113" → `DS24-2113-fix-error-dialog`
- User mentions "DS24-847 for the new auth flow" → `DS24-847-new-auth-flow`
- No Jira ticket mentioned → `user-authentication-flow`

I will save **two files** with identical content but different formatting:

### File 1: Markdown Format (SPEC)

**Filename:** `SPEC-<idea-title>-YYYY-MM-DD.md`

```markdown
# [Idea Title]

**Created:** YYYY-MM-DD
**Status:** Draft

## Overview

[High-level description of what we're building and why]

## Goals

- [Goal 1]
- [Goal 2]

## Non-Goals (Out of Scope)

- [Explicitly excluded item 1]
- [Explicitly excluded item 2]

## Technology Decisions

| Decision | Choice | Alternatives Considered | Rationale |
|----------|--------|------------------------|-----------|
| [e.g., Test Framework] | [e.g., Vitest] | Jest, Mocha | [Why this choice] |
| [e.g., HTTP Client] | [e.g., fetch] | Axios, ky | [Why this choice] |

## Architecture

[Architecture description and diagrams if applicable]

## Components

### [Component 1]
[Description and responsibilities]

### [Component 2]
[Description and responsibilities]

## Data Flow

[How information moves through the system]

## Error Handling

[What can go wrong and how we handle it]

## Testing Strategy

[How we'll verify the implementation works]

## Open Questions

[Any unresolved items to address during implementation]

## References

[Links to related docs, issues, or resources]
```

### File 2: Jira Format (JIRA)

**Filename:** `JIRA-<idea-title>-YYYY-MM-DD.md`

Same content as the SPEC file, but formatted using Jira wiki markup:

```
h1. [Idea Title]

*Created:* YYYY-MM-DD
*Status:* Draft

h2. Overview

[High-level description of what we're building and why]

h2. Goals

* [Goal 1]
* [Goal 2]

h2. Non-Goals (Out of Scope)

* [Explicitly excluded item 1]
* [Explicitly excluded item 2]

h2. Technology Decisions

||Decision||Choice||Alternatives Considered||Rationale||
|[e.g., Test Framework]|[e.g., Vitest]|Jest, Mocha|[Why this choice]|
|[e.g., HTTP Client]|[e.g., fetch]|Axios, ky|[Why this choice]|

h2. Architecture

[Architecture description and diagrams if applicable]

h2. Components

h3. [Component 1]
[Description and responsibilities]

h3. [Component 2]
[Description and responsibilities]

h2. Data Flow

[How information moves through the system]

h2. Error Handling

[What can go wrong and how we handle it]

h2. Testing Strategy

[How we'll verify the implementation works]

h2. Open Questions

[Any unresolved items to address during implementation]

h2. References

[Links to related docs, issues, or resources]
```

**Jira formatting reference:**
- `h1.` / `h2.` / `h3.` for headings
- `*text*` for bold
- `_text_` for italic
- `* item` for bullet lists
- `# item` for numbered lists
- `||header||header||` for table headers
- `|cell|cell|` for table rows
- `{code}...{code}` for code blocks
- `[link text|URL]` for links

---

## Step 8: Commit the Design Documents

After saving both documents, I will commit them to the claude directory's repo (not the current project's repo):

```bash
git -C <git-folder>/claude add <project-name>/plans/SPEC-<idea-title>-YYYY-MM-DD.md
git -C <git-folder>/claude add <project-name>/plans/JIRA-<idea-title>-YYYY-MM-DD.md
git -C <git-folder>/claude commit -m "docs: Add spec for <idea-title>"
```

---

## Key Principles

- **One question at a time** - Don't overwhelm with multiple questions
- **Multiple choice preferred** - Easier to answer than open-ended when possible
- **No silent technology decisions** - Always present library/framework options with trade-offs and get explicit approval
- **YAGNI ruthlessly** - Remove unnecessary features from all designs
- **Explore alternatives** - Always propose 2-3 approaches before settling
- **Incremental validation** - Present design in sections, validate each
- **Be flexible** - Go back and clarify if something doesn't make sense

---

## After Brainstorming

Once the spec is complete, I will ask:

**"Ready to set up for implementation?"**

If yes, we can proceed to create a detailed implementation plan.

---
allowed-tools: Read, Glob, Grep, Task, TodoWrite, Write, Bash(ls:*), Bash(tree:*), Bash(mkdir:*), AskUserQuestion
description: Create detailed implementation plans through interactive process
---

# Create Implementation Plan

You are tasked with creating detailed implementation plans through an interactive, iterative process. You should be skeptical, thorough, and work collaboratively with the user to produce high-quality technical specifications.

## Context

- Git branch: !`git branch --show-current`

Convert slashes to hyphens for the directory name (e.g., `feature/auth` → `feature-auth`). This determines the plan location: `.tasks/{branch-name}/plan.md`

## Initial Response

When this command is invoked, respond with:
```
I'll help you create a detailed implementation plan. Let me start by understanding what we're building.

Please provide:
1. The task description or requirements
2. Any relevant context, constraints, or specific requirements
3. Links to related research or previous implementations

I'll analyze this information and work with you to create a comprehensive plan.
```

Then wait for the user's input.

## Research Guidelines

Use `codebase-researcher` whenever you need to understand the codebase. Research findings are automatically saved to `.tasks/{branch}/findings/`.

**Core principle: One research = One focused question**
Each codebase-researcher call should answer ONE specific question. If you have multiple questions, spawn multiple researchers in parallel.

**When to research:**
- Before making design decisions
- When exploring unfamiliar areas of the codebase
- When you need to find existing patterns to follow
- When uncertain about implementation approach

**Good practices:**
```
# Good: Single focused question per researcher
"How is session managed in the login flow?"
"Analyze the structure and usage patterns of ImagePickerBottomSheet component"
"Find GraphQL mutation error handling patterns"

# Bad: Multiple questions bundled together (split these into parallel calls)
"1. Analyze login flow 2. How sessions work 3. Error handling patterns"

# Bad: Too broad
"Analyze the entire authentication system"
"Explain the app architecture"
```

**Important notes:**
- codebase-researcher has a limited context window—never bundle multiple questions
- Each research topic should have its own researcher and its own output file
- Multiple small research documents are better than one large document
- Research documents contain **facts about existing code**, not design proposals

## Process Steps

### Step 1: Context Gathering & Initial Analysis

1. **Read all mentioned files immediately and FULLY**
2. **Use codebase-researcher** to explore related areas of the codebase
3. **Present informed understanding and focused questions**: Based on research, ask only questions that require human judgment

### Step 2: Plan Structure Development

Once aligned on approach:
```
Here's my proposed plan structure:

## Overview
[1-2 sentence summary]

## Implementation Phases:
- [ ] 1. [Phase name] - [what it accomplishes]
- [ ] 2. [Phase name] - [what it accomplishes]
- [ ] 3. [Phase name] - [what it accomplishes]

Does this phasing make sense?
```

### Step 3: Detailed Plan Writing

**Before writing the plan:**
1. Read all documents in `.tasks/{branch}/findings/`
2. These research findings should inform your plan and be referenced where relevant

Write the plan to `.tasks/{branch-name}/plan.md` (where slashes in branch name are converted to hyphens):

```markdown
# [Feature/Task Name] Implementation Plan

## Overview
[Brief description of what we're implementing and why]

## Current State Analysis
[What exists now, what's missing, key constraints discovered]

## Desired End State
[Specification of the desired end state and how to verify it]

## What We're NOT Doing
[Explicitly list out-of-scope items]

## Implementation Approach
[High-level strategy and reasoning]

## - [ ] Phase 1: [Descriptive Name]

### Overview
[What this phase accomplishes]

### Changes Required:

#### 1. [Component/File Group]
**File**: `path/to/file.ext`
**Changes**: [Summary of changes]

### Success Criteria:

#### Automated Verification:
- [ ] Tests pass: `npm test`
- [ ] Type checking passes: `npm run typecheck`
- [ ] Linting passes: `npm run lint`

#### Manual Verification:
- [ ] Feature works as expected in UI
- [ ] Performance is acceptable
- [ ] No regressions in related features

---

## - [ ] Phase 2: [Descriptive Name]
[Similar structure...]

## Testing Strategy

### Unit Tests:
- [What to test]
- [Key edge cases]

### Integration Tests:
- [End-to-end scenarios]

### Manual Testing Steps:
1. [Specific verification step]
2. [Another verification step]

## Performance Considerations
[Any performance implications or optimizations needed]

## Migration Notes
[If applicable, how to handle existing data/systems]

## References

### Research Findings
- [./findings/topic-name.md](./findings/topic-name.md) - Brief description
- [./findings/another-topic.md](./findings/another-topic.md) - Brief description

### Other Sources
- [List of additional files read during planning]
- [Documentation consulted]
- [External resources referenced]
- [Related issues/PRs]
```

### Step 4: Review and Iterate

1. Save the plan and present location to user
2. Iterate based on feedback
3. Continue refining until satisfied

## Important Guidelines

1. **Be Skeptical**: Question vague requirements, identify issues early
2. **Be Interactive**: Get buy-in at each major step
3. **Be Thorough**: Include specific file paths and measurable success criteria
4. **Be Practical**: Focus on incremental, testable changes
5. **Track Progress**: Use TodoWrite throughout planning
6. **No Open Questions**: Resolve all questions before finalizing plan
7. **Ignore Other Plans**: Do NOT read or reference other existing plan files in `.tasks/`
8. **Cite Sources**: At the end of the plan document, include a "References" section listing all sources consulted during planning (e.g., files read, documentation, external resources, related issues/PRs)
9. **Minimize Code in Plans**: Plans are blueprints, not implementations. Only include code blocks when:
   - Defining API contracts or interfaces that must be exact
   - Logic is too tricky to explain clearly in words
   - A brief example significantly clarifies the intended approach
   For everything else, describe changes in plain language (e.g., "Add a validation function that checks X and Y" rather than writing the function).
10. **Research Before Planning**: Use codebase-researcher to understand the codebase before making design decisions. Research findings in `.tasks/{branch}/findings/` must be referenced in the plan's References section.

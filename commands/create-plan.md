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

### Step 2: Invoke Planner

Once research is complete, invoke the `planner` subagent to handle plan creation:

1. **Gather context for planner**:
   - Branch name (converted to directory format)
   - Summary of user requirements
   - Path to findings directory: `.tasks/{branch-name}/findings/`
   - Any additional considerations or emphasis points

2. **Call planner** (foreground, not background):
   ```
   Task tool with subagent_type: planner

   Prompt should include:
   - Branch name: {branch-name}
   - Task requirements: {summary of what user wants to implement}
   - Findings directory: .tasks/{branch-name}/findings/
   - Additional notes: {any constraints, preferences, or emphasis from the conversation}
   ```

3. **Let planner handle**:
   - Reading research findings
   - Proposing plan structure to user
   - Writing detailed plan
   - Iterating based on user feedback

The planner will interact directly with the user via `AskUserQuestion` and save the final plan to `.tasks/{branch-name}/plan.md`.

## Important Guidelines

1. **Be Skeptical**: Question vague requirements, identify issues early during context gathering
2. **Track Progress**: Use TodoWrite throughout planning
3. **Ignore Other Plans**: Do NOT read or reference other existing plan files in `.tasks/`
4. **Research Before Planning**: Use codebase-researcher to understand the codebase before invoking planner
5. **No Plan Mode Tools**: Do NOT use `EnterPlanMode` or `ExitPlanMode` tools. This command handles planning independently.

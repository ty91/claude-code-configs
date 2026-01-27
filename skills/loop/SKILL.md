---
name: loop
description: File-based task planning and execution workflow. Supports new/plan/run modes.
argument-hint: "[new | update [NNN] | run <NNN>]"
disable-model-invocation: true
allowed-tools:
  - Read
  - Write
  - Edit
  - Glob
  - Grep
  - Bash
  - Task
  - WebSearch
  - WebFetch
  - AskUserQuestion
---

# Loop: File-Based Task Workflow

You are operating in **Loop mode**, a file-based task planning and execution system.

## Core Philosophy

> **Context Window = RAM, Filesystem = Disk**
>
> Your context window is expensive and volatile. The filesystem is cheap and persistent.
> Write important information to disk immediately. Re-read files when you need them.

### Key Rules

1. **2-Action Rule**: After 2 search/read operations, immediately record findings in `findings.md`
2. **3-Strike Error Protocol**: After 3 failed attempts at the same problem, escalate to user
3. **Read Before Decide**: Re-read `plan.md` before any major decision
4. **Phase-by-Phase**: Complete one phase fully before moving to the next
5. **Manual Verification for User**: Run automated tests yourself; leave manual verification to the user

## Mode Detection

Parse `$ARGUMENTS` to determine the mode:

- `new` → Start a new task
- `update [NNN]` → Update task files (NNN optional)
- `run <NNN>` → Execute a plan

Arguments: $ARGUMENTS

---

## Mode: `new`

Start a new task with interactive planning.

### Step 1: Initial Setup

1. **Determine next task number**
   ```bash
   ls .loop/ 2>/dev/null | sort -n | tail -1
   # Next number = highest + 1, or 001 if none exist
   ```

2. **Greet and gather requirements**
   ```
   I'll help you create a detailed implementation plan. Let me start by understanding what we're building.

   Please provide:
   1. The task description or requirements
   2. Any relevant context, constraints, or specific requirements
   3. Links to related files, research, or previous implementations

   I'll analyze this information and work with you to create a comprehensive plan.
   ```

3. **Wait for user input** before proceeding.

### Step 2: Context Gathering & Initial Analysis

1. **Read all mentioned files immediately and FULLY**
   - Use Read tool WITHOUT limit/offset parameters
   - Read these files yourself in the main context

2. **Spawn research tasks in parallel** using Task tool:
   - Find all related files (patterns, imports, dependencies)
   - Understand current implementation
   - Find similar features to model after

3. **Record findings immediately** (2-Action Rule)
   - After reading files, write key discoveries to findings.md
   - Don't wait - your context window is not reliable

4. **Present informed understanding and ask focused questions**
   - Share what you learned from the codebase
   - Ask only questions that require human judgment
   - Don't ask questions you can answer by reading code

### Step 3: Research & Discovery

1. **Decompose the research question** into specific areas:
   - Which components are affected?
   - What patterns does this codebase use?
   - Are there similar implementations to reference?

2. **Spawn parallel sub-tasks** for comprehensive research:
   ```
   Task 1: Explore [area A] - find files matching [pattern]
   Task 2: Explore [area B] - understand [component]
   Task 3: Search web for [external topic] if needed
   ```

3. **Wait for ALL sub-tasks to complete** before proceeding

4. **Synthesize findings** and present design options with pros/cons

### Step 4: Plan Structure Development

Once aligned on approach, propose structure:

```
Here's my proposed plan structure:

## Overview
[1-2 sentence summary]

## Implementation Phases:
- [ ] Phase 1: [Phase name] - [what it accomplishes]
- [ ] Phase 2: [Phase name] - [what it accomplishes]
- [ ] Phase 3: [Phase name] - [what it accomplishes]

Does this phasing make sense? Should any phases be split, combined, or reordered?
```

**Wait for user confirmation** before writing the detailed plan.

### Step 5: Create Task Directory and Files

1. **Create directory**: `.loop/NNN/`

2. **Create plan.md** from template with:
   - Goal statement
   - Current State Analysis
   - Desired End State
   - What We're NOT Doing (scope boundaries)
   - Implementation Approach
   - Phased plan with checkboxes
   - Success Criteria (Automated vs Manual separated)
   - Testing Strategy
   - References (all files read, docs consulted)

3. **Create findings.md** from template with research results

4. **Create progress.md** from template

### Step 6: Review and Iterate

1. **Present plan summary** to user
2. **Ask for feedback**: "What would you like to change or clarify?"
3. **Iterate** until user approves
4. **Mark plan as ready** when user confirms

### Planning Guidelines

- **Be Skeptical**: Question vague requirements, identify risks early
- **Be Interactive**: Get buy-in at each major step, don't assume
- **Be Thorough**: Include specific file paths and measurable success criteria
- **Be Practical**: Focus on incremental, testable changes
- **No Open Questions**: Resolve all questions before finalizing plan
- **Cite Sources**: Include References section with all files/docs consulted
- **Minimize Code in Plans**: Only include code when:
  - Defining API contracts or interfaces that must be exact
  - Logic is too tricky to explain clearly in words
  - A brief example significantly clarifies the approach

  For everything else, describe changes in plain language.

---

## Mode: `update [NNN]`

Update task files. Behavior depends on context:

### Scenario Detection

1. **Session starts with `/loop update NNN`** → Interactive update mode
2. **Mid-conversation `/loop update`** → Context-based update mode

---

### Scenario A: Interactive Update (`/loop update NNN`)

Use when session starts with this command.

#### Step 1: Load Current State

Read all three files:
- `.loop/<NNN>/plan.md`
- `.loop/<NNN>/findings.md`
- `.loop/<NNN>/progress.md`

#### Step 2: Summarize Current State

Present to user:
```
## Task <NNN> Summary

**Goal**: [goal statement]

**Current Phase**: Phase [N] - [name]

**Progress**:
- [x] Phase 1: [name] - complete
- [ ] Phase 2: [name] - in progress (3/5 items done)
- [ ] Phase 3: [name] - pending

**Key Findings**:
- [Important discovery 1]
- [Important discovery 2]

**Blockers/Open Questions**:
- [Any unresolved issues]

What changes would you like to make to this plan?
```

#### Step 3: Gather Feedback

- Ask what changes are needed
- Clarify any ambiguities
- If user wants to add research, spawn Task agents

#### Step 4: Update Files

- Modify plan.md with new/changed phases
- Add new research to findings.md
- Note plan changes in progress.md with timestamp

#### Step 5: Confirm Changes

```
I've updated the plan with these changes:
- [Change 1]
- [Change 2]

Would you like to make any other adjustments?
```

---

### Scenario B: Context-Based Update (`/loop update`)

Use when called mid-conversation without a task number.

#### Step 1: Determine Target Task

Infer the task number from session start:

1. **Session started with `/loop run NNN` or `/loop update NNN`** → use that NNN
2. **Session started with `/loop new`** → use the task number created at the beginning
3. **Otherwise** → error: "Cannot determine task number. Session must start with /loop new, /loop run <NNN>, or /loop update <NNN>"

#### Step 2: Analyze Conversation

Review the conversation to identify:
- New discoveries or research findings
- Changes to the plan or approach
- Progress updates or completed items
- Errors encountered and resolutions
- Decisions made

#### Step 3: Update Appropriate Files

Based on conversation content, update relevant files:

| Content Type | Target File | Section |
|:-------------|:------------|:--------|
| New discoveries | findings.md | Research Findings |
| Plan changes | plan.md | Phases / Approach |
| Progress updates | progress.md | Session Log |
| Errors encountered | progress.md | Error Log |
| Decisions made | findings.md | Technical Decisions |

#### Step 4: Present Summary

```
I've updated task <NNN> files based on our conversation:

**findings.md**:
- Added: [discovery 1]
- Added: [discovery 2]

**plan.md**:
- Modified: [change description]

**progress.md**:
- Logged: [action taken]

Would you like me to adjust anything?
```

---

## Mode: `run <NNN>`

Execute an approved task plan.

### Step 1: Load Context

1. **Read all files completely**:
   - `.loop/<NNN>/plan.md` - the source of truth
   - `.loop/<NNN>/findings.md` - context and discoveries
   - `.loop/<NNN>/progress.md` - where you left off

2. **Read all files mentioned in the plan** before starting implementation

### Step 2: Identify Current Phase

1. Find the first phase with unchecked items (`- [ ]`)
2. If all phases complete:
   ```
   All phases complete! Here's a summary of what was accomplished:
   - [Summary of changes]
   - [Files modified]
   - [Tests passing]

   The task is ready for final review.
   ```

### Step 3: Execute Current Phase

For each step in the phase:

1. **Read Before Decide**: Re-read plan.md before major decisions

2. **Implement the change**:
   - Follow the plan's intent
   - Adapt if reality differs (see "When Things Don't Match" below)

3. **Record discoveries** in findings.md (2-Action Rule)

4. **Update progress.md** with actions taken:
   ```markdown
   #### Phase [N]: [Name]
   - **Status**: in_progress
   - **Started**: [timestamp]

   **Actions Taken**:
   - Created `path/to/file.ts` with [description]
   - Modified `path/to/other.ts` to [change]
   ```

5. **Mark checkbox** when step is complete using Edit tool

### Step 4: Verify Phase Completion

**Automated Verification** (run these yourself):
```bash
pnpm test          # or: npm test, uv run pytest
pnpm tsc --noEmit  # or: npx tsc --noEmit
pnpm lint          # or: npm run lint
```

**Manual Verification** (ask user):
```
Phase [N] automated checks passed.

Please verify the following manually:
- [ ] [Manual verification item 1]
- [ ] [Manual verification item 2]

Let me know when you've verified these, or if you find any issues.
```

**Wait for user confirmation** before proceeding to next phase.

### Step 5: Handle Errors

1. **Log error** in progress.md with timestamp:
   ```markdown
   | 2026-01-25 10:35 | TypeError: undefined is not a function | 1 | Investigating... |
   ```

2. **Attempt fix** (up to 3 times per error)

3. **After 3 strikes**, escalate:
   ```
   I've attempted to fix this error 3 times without success:

   **Error**: [error message]
   **Attempts**:
   1. [What I tried] - [Result]
   2. [What I tried] - [Result]
   3. [What I tried] - [Result]

   I need your guidance on how to proceed.
   ```

### Step 6: Transition to Next Phase

1. **Update progress.md** with phase completion
2. **Mark phase checkbox** as complete: `- [x] Phase N`
3. **Re-read plan.md** for next phase details
4. **Continue with Step 3**

### When Things Don't Match

If the plan doesn't match reality:

```
Issue in Phase [N]:

**Expected** (from plan): [what the plan says]
**Found** (in codebase): [actual situation]

**Why this matters**: [explanation of the impact]

Options:
1. [Option A] - [pros/cons]
2. [Option B] - [pros/cons]

How should I proceed?
```

**Wait for user decision** before continuing.

### Resuming Work

If the plan has existing checkmarks:
- **Trust completed work** - don't re-verify unless something seems off
- **Pick up from first unchecked item**
- **Re-read progress.md** to understand what was done

### Execution Guidelines

- **One phase at a time**: Don't jump ahead
- **Verify before proceeding**: Each phase must pass verification
- **Document everything**: Progress and errors go in their respective files
- **Respect checkboxes**: They are the source of truth for progress
- **Adapt thoughtfully**: Follow plan's intent, but report discrepancies

---

## File Locations

All task files are stored in `.loop/<NNN>/`:

```
.loop/
├── 001/
│   ├── plan.md        # Task plan with phases and checkboxes
│   ├── findings.md    # Research results and discoveries
│   └── progress.md    # Session log and error tracking
├── 002/
│   └── ...
```

## Templates

When creating new task files, read the templates from this skill's `templates/` directory:
- `templates/plan.md`
- `templates/findings.md`
- `templates/progress.md`

Customize them for the specific task.

---

## Error Handling

| Situation | Action |
|:----------|:-------|
| Task number not found | List available tasks in `.loop/`, ask user to choose |
| Invalid arguments | Show usage: `/loop new`, `/loop update [NNN]`, `/loop run <NNN>` |
| `/loop update` without NNN, session not started with loop command | Error: session must start with /loop new, /loop run, or /loop update |
| Verification failure | Log error, attempt fix, escalate after 3 strikes |
| Plan-reality mismatch | Report with "Issue in Phase [N]" format, wait for user |
| Need clarification | Ask specific questions, don't guess |

## Usage Examples

```
/loop new              # Start planning a new task
/loop update 001       # Update task 001 files interactively
/loop update           # Update current task from conversation context
/loop run 001          # Execute task 001
```

---
allowed-tools: Read, Glob, Grep, Bash(ls:*), Bash(tree:*), Bash(find:*), Bash(cat:*), WebSearch, WebFetch
description: Research codebase and external resources for implementation planning
---

## Context

- Project structure: !`tree -L 2 -I 'node_modules|.git|dist|build|coverage|__pycache__|.venv|venv'`
- Git status: !`git status --short`
- Recent commits: !`git log --oneline -5`

## Your task

Conduct pre-implementation research for the user's requested spec:

1. **Read PRD First**
   - Start by reading `docs/PRD.md` to understand the overall product requirements and context

2. **Explore Codebase**
   - Identify relevant code files and directory structure
   - Analyze existing coding conventions and patterns
   - Review similar existing implementations

3. **Gather External Knowledge** (if needed)
   - Search library/framework documentation
   - Research best practices

4. **Check Existing File**
   - If `docs/RESEARCH.md` already exists, ask the user whether to overwrite or append

5. **Document Findings**
   - Write research results to `docs/RESEARCH.md`

## Output Format

Include the following sections in `docs/RESEARCH.md`:
- Related Files: File paths and their roles
- Conventions: Discovered coding conventions/patterns
- External Resources: Reference documents/links (if any)
- Notes: Implementation considerations

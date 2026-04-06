---
name: next-task
description: Execute the next task/phase of the PLAN.md — plan, implement, test, commit, and update PLAN.md.
---

## Next Task Workflow

You are driving the project forward one task (or phase) at a time. Follow these steps in order.

### Step 1: Determine what to work on

Read PLAN.md at the project root. Identify the first incomplete task under "Next Up". If the user
provides arguments via `$ARGUMENTS`, treat that as guidance on what to work on instead. If the "Next
Up" section is empty, or doesn't exist, re-organize the PLAN.md as such. If the top item is large
(likely to consume more than 120k tokens for an LLM), please break it into smaller sub-tasks first,
then proceed as per these instructions but on a unit of work that will fit.

If the next step is ambiguous or there are open questions listed in PLAN.md that block this task,
ask the user to resolve them before proceeding.

### Step 2: Detect phase

Check whether the current task already has a detailed implementation plan — look for an
`### Implementation Plan` subsection under the task. This distinguishes two phases:

- **No implementation plan yet** → go to Step 3 (Planning).
- **Implementation plan exists** → go to Step 4 (Execution).

### Step 3: Planning

**Do NOT enter plan mode.** Explore the codebase thoroughly in normal mode — read the relevant
source files, tests, and any related code. Produce a detailed, concrete implementation plan that:

- Lists every file to create or modify
- Includes key code snippets or signatures where helpful
- Identifies test cases to add, and — when possible — prioritizes writing failing tests first,
  giving concrete goals to achieve in fixing the tests (TDD as appropriate)
- Calls out risks or open questions discovered during exploration

Write the plan into PLAN.md as an `### Implementation Plan` subsection under the current task.
Do not embed execution instructions in PLAN.md — only the plan content itself.

After writing the plan, tell the user:

> Plan written. Run `/clear` then `/next-task` to execute with a fresh context.

Then stop and yield. Do NOT begin implementation and do NOT ask "would you like to proceed?".

### Step 4: Execution

The current task has a plan. Summarize it to the user in 2-3 sentences, then follow the execution
workflow:

@../execution-workflow.md

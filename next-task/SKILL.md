---
name: next-task
description: Execute the next task/phase of the PLAN.md — plan, implement, test, commit, and update PLAN.md.
disable-model-invocation: true
---

## Next Task Workflow

You are driving the project forward one task (or phase) at a time. Follow these steps in order.

**Context survival note:** Entering plan mode clears your conversation context. To preserve
continuity, the plan you write in step 2 MUST include the full post-plan workflow (steps 3-7
below) as an explicit section at the end of the plan document. This way, when the plan is loaded
after exiting plan mode, the remaining instructions come with it.

### Step 1: Determine what to work on

Read `PLAN.md` at the project root. Identify the first incomplete task under "Next Up" (or, if
all "Next Up" items are done, look at "Future Work"). Summarize the task to the user in 2-3
sentences and confirm it's what they want to work on. If the user provides arguments via
`$ARGUMENTS`, treat that as guidance on what to work on instead. If the "Next Up" and "Future Work"
sections are empty, or don't exist, re-organize the PLAN.md as such. If the top item is large
(likely to consume more than 120k tokens for an LLM), feel free to sequence it into smaller
sub-tasks first, then proceed as per these instructions.

If the next step is ambiguous or there are open questions listed in PLAN.md that block this task,
ask the user to resolve them before proceeding.

### Step 2: Plan

Enter plan mode. Explore the codebase thoroughly to understand the current state — read the
relevant source files, tests, and any related code. Produce a detailed, concrete implementation
plan that:

- Lists every file to create or modify
- Includes key code snippets or signatures where helpful
- Identifies test cases to add
- Calls out risks or open questions discovered during exploration

**Critical:** At the end of your plan document, include a section titled
`## Post-Plan Execution Steps` containing the following verbatim instructions so they survive
the context transition:

```markdown
## Post-Plan Execution Steps

After the user approves this plan, execute these steps in order:

### Implement
Execute the plan above. Work methodically — use task lists to track progress. Prefer editing
existing files over creating new ones. Follow all project conventions from CLAUDE.md.
When in a cargo workspace, check compilation by running `chk` (never `cargo check -p`).

### Verify
1. Run `chk` to ensure formatting and linting pass.
2. Run `cargo test` (and any other relevant test commands).
3. If tests fail, fix them before proceeding.
4. If test coverage for the new work is insufficient, add tests.

### Commit
Stage and commit the changes with a semantic commit message. Do not add any attributions to
anyone, including yourself. Follow the project's commit message style (see `git log --oneline`).
If there are pre-existing modified files, and they don't look harmful or strange, go ahead and
commit them, too.

### Update PLAN.md
Read `PLAN.md`. Move the completed task from "Next Up" to the file COMPLETED.md with a summary of
what was done (matching the style of existing completed tasks if any exist). Update any open
questions that were resolved. If new future work items were discovered, add them. Do not ever add or
commit PLAN.md or COMPLETED.md to git.

### Yield
Tell the user what was accomplished and what the next task would be. Then stop and yield back
to the user — let them review before starting the next task. The user can invoke `/next-task`
again to continue.
```

Write the plan and exit plan mode for user review. Do NOT ask "would you like to proceed?" — just
present the plan via ExitPlanMode and wait for the user's response.

### Steps 3-7 (post-plan)

These steps are carried forward inside the plan document itself (see the `Post-Plan Execution
Steps` section above). After exiting plan mode and receiving user approval, follow those
instructions from the plan.

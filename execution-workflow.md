## Execution Workflow

These are the standard steps for executing a plan that has already been written into PLAN.md.

### Implement

Execute the plan from PLAN.md. Work methodically — use task lists to track progress. Prefer editing
existing files over creating new ones. Follow all project conventions from CLAUDE.md.
When in a cargo workspace, check compilation by running `chk` (never `cargo check ...` directly).

### Verify

1. Run `chk` to ensure formatting and linting pass.
2. Run `cargo nextest run` (and any other relevant test commands).
3. If tests fail, fix them before proceeding.
4. If test coverage for the new work is insufficient, add tests.

### Commit

Stage and commit the changes with a semantic commit message. Do not add any attributions to
anyone, including yourself. Follow the project's commit message style (see `git log --oneline`).
If there are pre-existing modified files, and they don't look harmful or strange, go ahead and
commit them, too.

### Update PLAN.md

Read `PLAN.md`. **Remove** the completed task entirely from the "Next Up" section — do not leave it
in place with a [DONE] tag, strikethrough, or any other marker. The task and its related subsections
should no longer appear in PLAN.md at all. PLAN.md should not have any sort of "Done" section. Then
append a brief summary of the completed work to `COMPLETED.md`.

If upcoming PLAN.md items need modifications due to a change during this plan's implementation then
update those. If new future work items were discovered, add them. Leftover compiler warnings count
as future work items unless they would naturally be handled by existing future work. Do not ever add
or commit PLAN.md or COMPLETED.md to git.

### Yield

Tell the user what was accomplished and what the next task would be. Then stop and yield back to
the user — let them review before starting the next task. The user can invoke `/next-task` again
to continue.

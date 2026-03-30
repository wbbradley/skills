# Claude Code Skills

Custom skills for [Claude Code](https://docs.anthropic.com/en/docs/claude-code).

## Install

```bash
git clone git@github.com:wbbradley/skills.git ~/.claude/skills
```

## Usage

1. Add tasks with `/todo` (front of queue) or `/later` (back of queue)
2. See what's queued with `/list-tasks`
3. Execute the next task with `/next-task`
4. Repeat

`/find-bugs` can audit the codebase and feed findings into the same queue. `todo` and `later` share a common workflow defined in `add-task-workflow.md`.

## Skills

### Task management

- **todo** — Research and add a new task to the front of the PLAN.md queue
- **later** — Research and add a new task to the back of the PLAN.md queue
- **next-task** — Execute the next task from PLAN.md — plan, implement, test, commit, and update PLAN.md
- **list-tasks** — List remaining tasks from PLAN.md as a concise numbered list
- **find-bugs** — Audit the codebase for bugs, inconsistencies, and other issues, then add findings to PLAN.md

### Release & profiling

- **publish** — Prepare a Rust crate for publishing to crates.io — update README, changelog, bump version, tag, and guide through publish
- **profile-rust** — Profile a Rust binary with samply on macOS, extract CPU hotspots, and measure memory usage

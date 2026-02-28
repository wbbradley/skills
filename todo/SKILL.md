---
name: todo
description: Research and add a new phase to the front of the PLAN.md queue.
---

## Todo Workflow

You are adding a new phase to the project's `PLAN.md`. The user has described what they want via
`$ARGUMENTS`. Your job is to turn that into a well-defined, actionable phase that could be handed
off to a strong junior SWE.

### Step 1: Delegate to a research sub-agent

Launch a sub-agent to do the exploration work. Before launching, read `PLAN.md` yourself so you can
pass its contents (or a sufficient summary) as context to the sub-agent along with the user's
`$ARGUMENTS`.

The sub-agent should:

1. **Understand the request.** Parse `$ARGUMENTS` for intent. If the request is ambiguous or
   underspecified, ask the user clarifying questions before proceeding — don't guess.

2. **Explore the codebase and gather requirements.** Read relevant source files, tests, configs,
   docs, or anything else needed to understand what this phase would touch. Use web search if the
   request involves external APIs, libraries, or concepts that need research.

3. **Check for overlap with existing phases.** Review the full `PLAN.md` for duplication or
   partial overlap. If the new work is already covered by an existing phase, tell the user and
   suggest whether to merge, extend, or skip. If the new phase would benefit from changes to
   other existing phases, note those suggestions — but do **not** modify other phases without
   explicit user approval.

4. **Think big picture, then zoom in.** Consider how this feature fits into the broader project:
   - Does it interact with or depend on other planned or completed work?
   - Are there inconsistencies, redundancies, or conflicts with the existing feature set?
   - What use-case scenarios does it enable? Are there edge cases worth calling out?
   - What risks or open questions should be flagged?

5. **Draft the new phase.** Write a phase entry that is concrete and actionable. It should make
   clear what files, modules, or systems are involved, what the expected behavior is, and what
   would need to be true for the phase to be considered complete. Write it so that a competent
   developer with access to the codebase — but no other context — could pick it up and execute.

The sub-agent should return the drafted phase text and any suggestions about existing phases.

### Step 2: Add the phase to PLAN.md

Read the current `PLAN.md`. Insert the new phase at the **top** of the "Next Up" section (first in
the queue). If `PLAN.md` doesn't exist yet, create it with "Next Up", "Completed", and
"Future Work" sections, and place the new phase under "Next Up".

If the sub-agent suggested modifications to other existing phases, present those suggestions to the
user and only apply them with approval.

### Step 3: Retrospective

Tell the user:
- What phase was added and a brief summary of its scope.
- How it fits into the larger plan — dependencies, synergies, or tensions with other phases.
- Any open questions or risks that surfaced during research.

Then stop and yield to the user.

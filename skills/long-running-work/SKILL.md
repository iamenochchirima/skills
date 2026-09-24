---
name: long-running-work
description: Plan and execute substantial multi-phase software work using persistent state, feature slices, bounded verification, and recovery across long autonomous runs. Do not use for small, self-contained changes.
---

# Long-Running Work

Use a single local plan as durable working memory for substantial software tasks.

The core unit of progress is a **slice**: the smallest meaningful, independently verifiable unit of user-visible or system-visible behavior that leaves the repository in a coherent state.

A slice is:

- a top-level checklist item;
- a meaningful unit of behavior;
- independently verifiable;
- a recovery boundary;
- a committable unit.

A slice may contain internal implementation checkboxes. Those are execution aids, not independent delivery or verification units.

Prefer:

> [ ] User can schedule a campaign.

over:

> [ ] Add campaign table.
>
> [ ] Add scheduler service.
>
> [ ] Add API route.

The table, service, and route normally belong inside the same behavioral slice.

---

# When to use

Use this skill when work involves:

- multiple dependent behaviors or requirements;
- enough complexity to span several focused execution periods;
- significant implementation or operational risk;
- a need to retain progress across context compaction or interruption;
- autonomous execution where losing state or expanding scope would be costly.

Do not use it for a narrow change that can be implemented and verified directly.

Before planning:

1. Read the user's request.
2. Inspect the relevant repository and conventions.
3. Understand the existing behavior.
4. Preserve the user's stated scope.
5. Do not add work merely because it seems useful.

---

# Working state

Keep the active plan at:

`.long-running-work/active/<task-slug>.md`

Move completed plans to:

`.long-running-work/completed/<task-slug>.md`

When possible, exclude `.long-running-work/` locally through `.git/info/exclude`.

Do not modify project-owned files such as `.gitignore`, `AGENTS.md`, or documentation merely to store agent state.

Use one active plan per task.

The plan is operational state, not a diary. Keep only information needed to execute, recover, verify, and finish the task.

---

# Plan with feature slices

Break the requested outcome into meaningful vertical slices.

Good slices:

- User can create and cancel a booking.
- Scheduled campaigns are picked up and executed.
- Agent tool calls survive worker restart without duplicate side effects.
- Admin can disable a tenant.

Poor slices:

- Add database model.
- Add types.
- Create service.
- Add API route.
- Write tests.

Those are normally implementation steps inside a slice.

A slice should be:

- describable as one meaningful outcome;
- independently verifiable;
- a coherent diff a reviewer could understand in one pass;
- capable of leaving the repository in a valid state.

Ask:

> Could I describe what this slice delivers in one sentence?

If not, it may be too broad.

Ask:

> Is this only an implementation detail needed for another behavior?

If yes, it probably belongs inside that slice.

Foundation slices are allowed when necessary infrastructure cannot naturally expose user-visible behavior. They must still have a concrete outcome, acceptance criteria, and verification.

---

# Checklist semantics

Top-level slice checkboxes are the units of delivery.

A slice becomes [x] only when:

- its implementation checklist is complete;
- its acceptance criteria are satisfied;
- its planned verification passes;
- concrete regressions introduced by the slice are resolved;
- evidence and a checkpoint are recorded.

Internal implementation checkboxes track progress within the slice.

Tick them as work proceeds so mid-slice recovery remains possible.

Internal items do not independently require:

- acceptance criteria;
- test suites;
- evidence;
- commits.

Do not turn internal implementation items into miniature slices.

---

# Plan format

Use this structure:

`markdown
# <Task title>

## Goal and scope

<Requested outcome and important boundaries.>

### Out of scope

- <Explicit non-goal>

## Constraints

- <Important repository, compatibility, or architectural constraint>

## Definition of done

- [ ] <Observable end-to-end outcome>
- [ ] <Observable end-to-end outcome>

## Baseline

Existing behavior:
<What currently works or fails.>

Relevant validation:
- `<command>`

Git state:
<branch, commit, relevant pre-existing changes>

---

## Slices

### [ ] Slice 1 — <observable behavior>

**Risk:** low | normal | high; **Test budget:** <number>; **Depends on:** none | <slice>

#### Outcome

<One sentence describing what this slice delivers.>

#### Acceptance criteria

- [ ] <observable behavior>
- [ ] <observable behavior>

#### Invariants

- [ ] <important property that must remain true>

Use `None` when no important invariant exists beyond the acceptance criteria.

#### Implementation

- [ ] <implementation requirement>
- [ ] <implementation requirement>
- [ ] <implementation requirement>

#### Required verification

- [ ] <verification mapped to acceptance criterion or invariant>
- [ ] `<relevant command>` passes

Checkpoint: pending | Evidence: pending

---

### [ ] Slice 2 — <observable behavior>

...

---

## Deferred work

- None.

## Current position

Slice: <number and name>; Current focus: <brief description>; Last checkpoint: <checkpoint or none>

## Decisions and findings

- <Only information that materially affects later work>

## Blockers

None.

## Completion audit

- [ ] Original request and definition of done satisfied.
- [ ] Every required slice complete with evidence.
- [ ] Appropriate repository-level regression validation passes.
- [ ] No known regression introduced by this work remains.
- [ ] Remaining non-blocking findings are deferred.
`

---

# Verification and debugging

Optimize for **sufficient evidence of correctness, not exhaustive evidence of correctness**.

The central rule is:

> **Explore deeply when concrete evidence shows something is broken. Do not explore broadly merely because something might be broken.**

## Acceptance criteria and invariants

Acceptance criteria define the behavior the slice must deliver.

Invariants define important properties that must remain true while delivering it.

Choose important invariants during planning.

Every planned new test should map to:

1. an acceptance criterion; or
2. a named invariant.

Do not add tests merely because:

- another edge case can be imagined;
- another branch exists;
- another helper was added;
- another theoretical failure is possible;
- broader coverage would be nice.

A passing slice does not need more tests simply because more tests could be written.

If execution reveals a previously unknown correctness requirement, classify it under the scope rules before expanding verification.

## Test budgets

Each slice gets a test budget during planning.

Default guidance:

- **Low risk:** up to 2 focused new tests.
- **Normal risk:** up to 4 focused new tests.
- **High risk:** choose an explicit budget based on the named invariants.

The budget limits planned new test cases. It does not cap required commands or manual checks, or investigation of a concrete failure.

Do not consume the full budget merely because it exists.

Do not increase it because more hypothetical edge cases can be imagined.

A test beyond the planned budget is justified when concrete evidence discovered during implementation shows that additional verification is needed for an existing acceptance criterion, named invariant, or observed regression.

Record the reason briefly under Decisions and findings.

## Debug concrete failures deeply

A real failure permits deep investigation.

When a concrete bug, failing test, or regression exists:

- reproduce it;
- inspect relevant code and state;
- add temporary diagnostics when useful;
- test hypotheses;
- try alternative fixes;
- use focused experiments;
- add a regression test when appropriate;
- continue until the concrete failure is understood and resolved or a genuine external blocker exists.

Do not stop merely because several attempts were required.

Keep the investigation anchored to the observed failure.

A difficult bug does not authorize unrelated edge-case discovery, broad refactoring, or general hardening.

## Verification scope

Use the narrowest useful verification first.

During a slice:

1. run the planned behavioral verification;
2. run tests directly related to the changed area;
3. broaden regression testing only when the changed surface justifies it.

Do not run the entire repository suite after every slice unless:

- the suite is cheap; or
- the slice changes shared infrastructure with broad regression risk.

Run broader repository-level validation during the completion audit.

Required verification is chosen during planning and should not casually expand during execution.

Adding new required verification follows the same classification rule as adding new work.

---

# Scope control and convergence

The plan must converge.

Required work should generally shrink as execution progresses.

Discovering something that could be improved does not make it required.

New work enters the active task only when necessary to:

1. satisfy an existing acceptance criterion;
2. preserve a named invariant;
3. fix a concrete regression introduced by the current work;
4. resolve a concrete blocker to the requested outcome; or
5. satisfy a requirement clearly present in the original request but accidentally omitted from the plan.

Otherwise, add it to **Deferred work**.

Typical deferred work includes:

- speculative edge cases;
- unrelated refactors;
- broader cleanup;
- optional abstractions;
- optional performance improvements;
- exhaustive test permutations;
- speculative hardening;
- nice-to-have improvements.

Classify new work **before implementing it**.

Ask:

> Would the requested behavior be incorrect, broken, or violate an existing acceptance criterion or named invariant without this change?

If yes, it is required.

If no, defer it.

If required work belongs naturally to the current behavior, add it to the current slice.

Create a new slice only when the discovered work represents a separate meaningful behavior.

## Completed slices stay completed

Do not repeatedly review, test, refactor, or harden a completed slice.

Revisit it only when concrete evidence shows:

- it is incorrect;
- it caused a regression;
- an assumption was false; or
- later integration legitimately requires a change.

Do not reopen completed slices merely to look for additional problems.

## Hardening boundary

Do not enter a hardening phase automatically.

If hardening, security review, stress testing, resilience work, performance work, or similar work was explicitly part of the original request, represent it as planned slices from the beginning.

Otherwise, hardening requires a new explicit user request.

Deferred work never authorizes further implementation by itself.

Finishing the requested implementation does not imply permission to harden it.

---

# Execute

For each slice:

1. Re-read the goal, current position, and current slice.
2. Inspect the relevant existing implementation.
3. Work through the internal implementation checklist, ticking items as completed.
4. Implement the smallest coherent solution satisfying the slice.
5. Run the planned verification.
6. Investigate concrete failures as deeply as necessary while staying anchored to them.
7. Resolve regressions introduced by the slice.
8. Run appropriate affected-area regression checks.
9. Record concise evidence.
10. Inspect the complete slice diff.
11. Record a checkpoint.
12. Mark the top-level slice [x].
13. Update Current position.
14. Continue to the next actionable slice.

Update Current position at slice boundaries and before risky or interruptible work, not after every edit.

---

# Checkpoints

Every completed slice must have a checkpoint.

A checkpoint is either:

- a commit hash for the completed slice when commits are authorized; or
- a verified working-tree state when commits are not being created.

A slice must be committable, but committing is not required unless authorized.

When commits are authorized, prefer one coherent commit per completed slice containing:

- the implementation;
- tests belonging to the behavior;
- necessary supporting changes.

Internal implementation checkboxes are not commit boundaries.

Before recording a checkpoint:

1. inspect the working tree and slice diff;
2. preserve unrelated user-owned changes;
3. confirm required verification passes;
4. confirm the repository is coherent;
5. commit the slice if authorized;
6. record the commit hash, or for a working-tree checkpoint record the current branch and HEAD, task-owned changed files, and verification evidence.

Do not rewrite unrelated history or user-owned work.

---

# Recovery

The active plan is the source of truth after context compaction, interruption, restart, or uncertainty.

Before resuming:

1. Read the active plan.
2. Re-read the goal and definition of done.
3. Find the first unfinished top-level slice.
4. Read its internal checklist and current focus.
5. Inspect Git status and recent relevant commits.
6. Verify the repository matches the recorded checkpoint.
7. Correct stale plan state if necessary.
8. Resume from the unfinished internal item.

Example:

`markdown
### [x] Slice 1 — User can schedule a campaign

Checkpoint: a83d91c | Evidence: scheduling behavior tests pass

### [x] Slice 2 — Scheduled campaign executes

Checkpoint: d51a220 | Evidence: worker integration tests pass

### [ ] Slice 3 — Failed execution retries safely

#### Implementation

- [x] Persist execution attempt identity.
- [x] Propagate idempotency key.
- [ ] Handle worker restart after provider success.

#### Required verification

- [ ] Retry after simulated crash does not duplicate action.
`

Resume from the unfinished work in Slice 3.

Do not repeat completed slices because conversational context was lost.

---

# Completion audit

After all required slices are complete, perform one bounded audit against the original request.

Confirm:

- the original request and definition of done are satisfied;
- every required slice has evidence and a checkpoint;
- appropriate repository-level regression validation passes;
- no known regression introduced by this work remains;
- remaining non-blocking findings are deferred.

The audit catches omissions and regressions.

It does not authorize:

- searching indefinitely for new edge cases;
- speculative tests;
- unrelated cleanup;
- redesigning completed work;
- optional hardening;
- reopening slices without evidence.

If the audit exposes a concrete correctness problem, fix it.

If it reveals only another possible improvement, defer it.

After the audit passes:

1. record the final checkpoint;
2. move the plan from `active/` to `completed/`;
3. report the completed behavior, verification performed, and meaningful deferred work.

---

# Blockers

Stop autonomous execution only when progress genuinely requires something the agent cannot resolve itself, such as:

- a user decision;
- missing credentials;
- unavailable external access;
- destructive approval;
- materially ambiguous product behavior;
- an external dependency that cannot be worked around safely.

Do not treat a difficult bug as a blocker merely because several fixes failed.

Continue investigating concrete failures while useful evidence and reasonable avenues remain.

Ask the user when progress actually depends on information, permission, or a decision the agent cannot obtain independently.

Record genuine blockers as:

`text
Blocker:
<exact problem>

Impact:
<what cannot continue>

Needed:
<smallest user decision or action required>
`

Do not declare the task complete while a required blocker remains.

---

# Planning and approval

After writing the plan:

- if the user requested planning or review before implementation, present the slices, risks, test budgets, and verification and wait for approval;
- if the user explicitly authorized autonomous implementation, proceed;
- if planning reveals a material ambiguity, destructive decision, or architectural choice not implied by the request, ask before proceeding.

Do not require approval for decisions already covered by the user's request or established repository conventions.

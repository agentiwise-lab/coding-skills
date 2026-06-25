---
name: implement
description: Builds the work for one Linear issue or a whole plan, one unit at a time, test-first, as tracer-bullet vertical slices. Use when the user says "implement this issue", "build the plan", "ship this unit", or hands a self-contained Linear issue or a docs/plans/<slug>.md. Triggers on implement, build, ship a unit. Not for planning (use /plan), not for reviewing existing code (use /review-code), not for debugging (use /diagnose).
---

# implement

Build the work for one issue or a whole plan, one unit at a time, test-first.

## Inputs
- A self-contained Linear issue, OR
- `docs/plans/<slug>.md`: implement all units in dependency order, or the first unblocked unit.

## Process
For each unit, in dependency order:
1. Identify the unit's seams (the 2-3 handoff points). Criterion: each seam has a named contract before any code.
2. Run `/tdd` to build the unit through those seams (Red-Green-Refactor). Criterion: the unit is a tracer-bullet vertical slice, demoable on its own.
3. Typecheck and run the single touched test file after each green. Criterion: both pass.
4. Run the full suite once after the unit is complete. Criterion: suite is green.
5. Commit the unit to the current branch (after confirmation). Criterion: one commit per unit.
6. Mark the issue done. Criterion: issue status updated only after step 4 passed.

## Hard rules
- Backend is Red-Green-Refactor via `/tdd`: one failing test, watch it fail RED, minimum code to pass, repeat, refactor once at the end. Why: minimum-to-pass keeps modules deep and stops speculative code.
- Frontend implements directly, no TDD. Why: component props are the contract; UI behavior is verified by running it, not unit tests.
- Fakes, not mocks. Why: a `Fake*` implementing the contract survives refactors; mocks patch internals and break.
- Tests live in a root `tests/` folder, scoped strictly to the touched function. Why: do not backfill tests for untouched code.
- No hardcoded values. Why: a hardcoded path or key is invisible coupling that breaks on the next environment.
- Never claim done without fresh verification evidence: run it, paste the output. Why: "should work" is not evidence.
- Never commit or push without confirmation. Why: the user owns when work lands.
- Any destructive change is subject to `/destructive-change-gate`. Why: reset, rebase, force-push can drop work; the gate makes the blast radius explicit first.

## Handoff
Run `/review-code`, then `/review-pr`.

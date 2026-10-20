# Plan document templates

Use these section structures exactly so output is consistent across runs. Fill
every section; never leave a placeholder or a "TBD".

---

## `overview.md` template

```markdown
# <Project / Feature> — Project Overview

## Problem
What's wrong or missing today, and who it affects. 1–2 short paragraphs.

## Goals
Bulleted, concrete outcomes this work must achieve.

## Non-goals
Bulleted, explicit out-of-scope items — what this work will NOT do. This is as
important as the goals; it prevents scope creep.

## Users & primary flow
Who uses it and the main end-to-end flow, step by step.

## Constraints
Technical and product constraints: existing stack, platforms, performance, data,
compatibility, deadlines, anything that bounds the solution.

## Success criteria
How you'll know it's done and good — observable, checkable statements.

## Decisions
A list of each question asked during the interview and the answer chosen:

- **<Question topic>** → <chosen answer>.
- **<Question topic>** → <chosen answer>.
```

---

## `phase-NN-<slug>.md` template

```markdown
# Phase NN — <Title>

**Depends on:** <earlier phase numbers, or "nothing"> · **Enables:** <what later
work this unblocks>

## Goal
One paragraph: what this phase delivers and why. Each phase should be an
independently shippable, verifiable increment.

## Files touched
Bulleted list of files to create/edit, each with a one-line note on the change.
Use real paths from the codebase.

## Steps
Ordered, concrete implementation steps. Resolve every decision here — no "TBD",
no "decide later", no unstated either/or.

## Build gate
The commands that must pass before this phase is considered done (build, type
check, lint, tests — whatever the project uses).

## Test plan
How to verify this phase works: the specific scenarios/tests, manual or
automated.

## Commit
A suggested commit message (conventional-commits style) for this phase.

## Rollback
How to back this phase out if it regresses — what reverting touches, and whether
it's safe to leave partially applied.
```

---

## Ordering check (apply before finalizing)

For every phase, confirm both:

- Its **Depends on** lists only **lower-numbered** phases (or "nothing").
- Nothing in its **Steps / Files touched** needs something a **later** phase
  introduces.

If a phase needs something from a later phase, the phases are mis-ordered —
reorder and renumber so dependencies always point backward. This is the property
that lets someone implement the plan top-to-bottom without ever getting stuck.

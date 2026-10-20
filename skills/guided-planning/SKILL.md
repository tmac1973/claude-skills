---
name: guided-planning
description: Turn a raw idea into a written project overview and a dependency-ordered, phased implementation plan through an interactive, one-question-at-a-time interview, writing markdown plan files to a plan/ folder (planning only — no code). Use this whenever the user wants to scope, plan, spec, or design a new project or feature, asks for an implementation plan / roadmap / design doc, says things like "help me plan X", "turn this idea into a plan", "let's spec this out", "guided planning", or starts describing something they want to build before any code exists. Prefer this over jumping straight to code whenever the work is multi-step or under-specified.
---

# Guided Planning

Take the user from a one-line idea to two artifacts, written to disk as markdown:

1. **`overview.md`** — a crisp project definition.
2. **`phase-NN-*.md`** — a dependency-ordered, phased implementation plan.

You do this by **interviewing the user one question at a time** until every
decision is made, grounding yourself in their actual codebase, and verifying the
plan before you finalize it. **Planning only — never write or edit code in this
skill.** The deliverable is the plan.

Run it in two stages with a review gate between them:

```
Stage 1: Overview interview → write overview.md → REVIEW CHECKPOINT
Stage 2: Planning interview → verify → APPROVAL CHECKPOINT → write phase files
```

If the user only wants the overview, stop after Stage 1. If they already have an
overview and want the plan, start at Stage 2 (read their overview first).

## Core rules (these are what make the output good)

- **One question at a time.** Use the `AskUserQuestion` tool for every decision,
  asking exactly **one** question per call — do not batch several into one call,
  and do not ask in prose. Give 2–4 concrete options, each with a short
  description; put your recommended option first and append " (Recommended)" to
  its label. `AskUserQuestion` already adds an "Other" choice so the user can
  always type their own answer — you don't need to add one. One question at a
  time keeps the user focused and produces sharper decisions than a wall of
  questions.
- **Be exhaustive, then stop.** Keep asking until you can write a *complete*
  artifact with nothing left vague. Tell the user up front they can answer
  "proceed" (via the Other option) to any question to cut the interview short.
- **No deferred decisions.** Never leave a "TBD", "we'll decide later", or an
  unresolved either/or in the written output. If something is unresolved, that
  is a question to ask — not a note to write down.
- **Ground in reality.** For an existing project, read the codebase before and
  during questioning (Read, Grep, Glob; spawn an Explore subagent for broad
  sweeps) so your questions and the plan use real file paths, real dependencies,
  and existing patterns. For a brand-new project there's nothing to read — skip
  it.
- **Dependency-ordered phases.** Order phases so each one depends ONLY on
  earlier phases, never a later one. This is the single most important property
  of the plan — a reader must be able to implement phases in order without ever
  needing something that hasn't been built yet.
- **Record the decisions.** End `overview.md` with a Decisions section listing
  each question you asked and the answer the user chose, so the rationale is
  traceable.

## Stage 1 — Project overview

1. **Frame it.** Briefly tell the user you'll ask a series of one-at-a-time
   multiple-choice questions to define the project, and that they can type their
   own answer or say "proceed" anytime.
2. **Ground yourself** in the working directory if it's an existing project.
3. **Interview** with `AskUserQuestion`, one question at a time, until you've
   resolved: the problem, goals, non-goals, the primary user flow, key
   constraints, and success criteria. Each answer should sharpen the next
   question.
4. **Write `overview.md`** to the output folder (see Output layout) using the
   overview template in `references/templates.md`. Include the **Decisions**
   appendix.
5. **Review checkpoint.** Tell the user the overview is written and ask them to
   review it. They can: approve and continue; tell you what to change (revise
   the file and re-present); or edit the file themselves and have you re-read
   it. Loop until they approve. Do not start the plan until the overview is
   approved.

## Stage 2 — Phased implementation plan

1. **Read the approved `overview.md`** and re-ground in the codebase so the plan
   fits the real code.
2. **Interview** with `AskUserQuestion`, one question at a time, to resolve every
   *implementation* decision the overview doesn't already settle (architecture,
   libraries, data model, sequencing, etc.). Do not defer — if it's unresolved,
   ask.
3. **Draft the phases.** Break the work into phases ordered strictly by
   dependency. Write each to `phase-NN-<slug>.md` using the phase template in
   `references/templates.md`. Resolve every decision in the text — no "TBD".
4. **Verify independently.** Spawn a fresh subagent (Task/Agent tool) that reads
   ONLY the overview + the drafted phase files and reports two things:
   (a) any phase that depends on a later phase (an ordering violation), and
   (b) any deferred/unresolved decision. Fix everything it finds and re-verify
   until it comes back clean. A fresh pair of eyes catches ordering mistakes
   your own draft will not. (If subagents aren't available, do this review
   yourself in a separate, deliberate pass.)
5. **Approval checkpoint.** Present the ordered phase list with each phase's
   dependencies as a short map, and ask the user to approve or request changes.
   Apply any changes, re-verify, and re-present. Finalize on approval.

## Output layout

Write everything into a per-feature folder so runs never clobber each other:

```
plan/<feature-slug>/
├── overview.md
├── phase-01-<slug>.md
├── phase-02-<slug>.md
└── …
```

Derive `<feature-slug>` from the project/feature name (lowercase, kebab-case).
Confirm the location with the user early if it matters; default to
`plan/<feature-slug>/` relative to the working directory. Write **only** inside
that folder — never touch code, and never write outside the plan folder.

## Templates

The exact section structure for both documents lives in
`references/templates.md`. Read it before writing `overview.md` (Stage 1) and
before writing the first phase file (Stage 2), and follow it exactly so output
is consistent across runs.

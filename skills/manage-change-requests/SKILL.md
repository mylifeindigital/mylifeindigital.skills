---
name: manage-change-requests
description: Manage repository-local Markdown change requests, including bootstrapping change-requests/, capturing ideas, performing reconnaissance, planning, updating, reviewing, prioritizing, and closing work. Use when the user mentions change requests, CRs, a local backlog, planned implementation work, PRD-like notes, or Markdown-based implementation tracking. Defer to more specific repository-local instructions when they exist.
---

# Manage Change Requests

Manage implementation work as durable Markdown inside the repository. Keep the
workflow useful without requiring an external issue tracker.

## Establish The Project Contract

Before reading or changing a request:

1. Find the repository root and read its `AGENTS.md` instruction chain.
2. Look for `change-requests/WORKFLOW.md`, then `change-requests/SKILL.md`.
3. Read `change-requests/templates/change-request.md` when it exists.
4. Read `change-requests/index.md` before opening detail files.
5. Treat project-local instructions and templates as authoritative when they
   differ from this skill.

Do not assume that every project uses the default statuses, priorities, table
columns, or detail headings. Report a material conflict between local files
rather than silently choosing one.

## Bootstrap A Project

Bootstrap only when the user explicitly asks to introduce the mechanism or asks
for a change request in a repository that does not have one.

1. Create `change-requests/templates/`.
2. Copy `assets/index.md` to `change-requests/index.md`.
3. Copy `assets/change-request.md` to
   `change-requests/templates/change-request.md`.
4. Add concise project-specific rules to `AGENTS.md` when the repository needs
   deviations from the defaults.

Do not overwrite an existing backlog, index, workflow, or template while
bootstrapping.

## Default Contract

Use these defaults only when the project does not define its own contract:

- Store the dashboard at `change-requests/index.md`.
- Store one detail file per promoted request.
- Use stable IDs in the form `CR-001`, `CR-002`, and so on.
- Name detail files `CR-xxx-short-kebab-title.md`.
- Keep one implementation outcome per request.
- Use `Proposed`, `Planned`, `In Progress`, `Blocked`, `Done`, or `Dropped`.
- Use `High`, `Medium`, or `Low` priority.
- Preserve useful history; add decisions and implementation notes instead of
  rewriting the record to look predetermined.

## Move Ideas Through Three Gates

### Gate 1: Capture The Note

Add an index row containing one sentence that says what should change and why
it matters now. Do not create a detail file unless the user asks for one or the
work is being actively considered.

Most ideas should be allowed to remain lightweight notes.

### Gate 2: Perform Reconnaissance

Before drafting an implementation plan, inspect the repository to find the
load-bearing fact that determines the shape of the work. Write `Context` from
current evidence, cite relevant paths, and record the date in `Reviewed`.

Create explicit `Open Questions`. A question must identify a decision that can
change the implementation, not merely restate missing research. If no open
question exists, verify that the request is genuinely trivial rather than
under-examined.

### Gate 3: Record Decisions

Resolve open questions before implementation. For each resolution, add a dated
entry to `Decisions` that records both the choice and the evidence or constraint
that settled it.

Phase work by verifiability. Give each phase a check that can fail; avoid phases
that merely name a broad activity.

## Create Or Promote A Request

1. Inspect the index and existing detail filenames.
2. Allocate the next unused numeric ID; never reuse an old ID.
3. For a Gate 1 note, add only the index row.
4. For a promoted request, copy the project template, or the bundled template
   when no project template exists.
5. Fill `Context` from repository evidence rather than memory.
6. Add the detail link to the index and keep dashboard metadata consistent.
7. Keep the first version concise enough to remain reviewable.

## Update A Request

1. Update the detail file first.
2. Update matching dashboard metadata immediately afterward.
3. Add dated decisions and implementation notes without deleting useful
   history.
4. Re-check `Context` when repository changes may have overtaken it, then update
   `Reviewed`.
5. Use `Blocked` when the request names an unresolved dependency or required
   decision, and record the blocker and next action.

Do not infer implementation status from intention alone.

## Review A Backlog

For a focused review, read only the relevant requests. For a full grooming pass,
read every non-template `CR-*.md` file.

Check for:

- index/detail status mismatches
- stale context or missing review dates
- unresolved dependencies hidden behind `Proposed`
- missing or weak acceptance criteria
- broad requests containing multiple outcomes
- duplicate or overlapping requests
- unchecked open questions on work marked `In Progress` or `Done`
- completed implementation without a recorded outcome

Do not create, merge, split, close, or delete requests during a review unless
the user authorized those changes.

## Implement And Complete Work

Do not begin implementation while an open question remains unresolved. Resolve
questions from repository evidence when possible. Ask the user only when a
choice would materially change the outcome and cannot be established locally.

Mark a request `Done` only when:

- its acceptance criteria are satisfied,
- its open questions are resolved and captured in `Decisions`,
- relevant implementation notes are recorded,
- `Outcome` states what actually changed,
- and the index reflects the final status.

Use `Dropped` only for intentionally abandoned work and record the reason in
`Outcome`.

## Report Changes

Summarize:

- requests or dashboard rows created or changed,
- important reconnaissance findings and decisions,
- unresolved blockers or questions,
- and validation performed.

Do not replace the repository workflow with GitHub Issues, project-management
software, or another external system unless the user explicitly requests that
migration.

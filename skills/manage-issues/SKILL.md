---
name: manage-issues
description: Manage planned work as GitHub issues with the gh CLI, including capturing ideas, reconnaissance, decisions, labels, project-board status, and closing issues with an outcome. Use when the user mentions issues, change requests, CRs, the backlog, the board, planned implementation work, or PRD-like notes. Defer to more specific repository-local instructions when they exist.
---

# Manage Issues

Track implementation work as GitHub issues, created, updated, and closed with
`gh`. Issues live on GitHub rather than on a branch, so they need no pull
request to change.

## Establish The Project Contract

Before reading or changing an issue:

1. Find the repository root and read its `AGENTS.md` instruction chain. Look
   for an "Issues" section naming labels, a board, and a backlog-order issue.
2. Confirm the repository with `gh repo view --json nameWithOwner`.
3. List labels with `gh label list` and open issues with
   `gh issue list --state open --limit 100`.
4. Treat project-local instructions as authoritative when they differ from
   this skill, and report material conflicts instead of silently picking one.

## Default Contract

Use these defaults only when the project does not define its own contract:

- Labels: `idea` (worth exploring, may be dropped), `change` (accepted work),
  `decision` (a question to settle; the issue records the answer and why).
  Give every issue exactly one of them.
- Board: one user-level GitHub Project collects issues from every repository.
  For `mylifeindigital` repositories it is
  <https://github.com/users/mylifeindigital/projects/4> (owner
  `mylifeindigital`, number `4`), with a `Status` field of `Todo`,
  `In Progress`, `Blocked`, and `Done`. Auto-add rules put new issues on it.
- Keep one implementation outcome per issue.
- Preserve history: add dated comments for decisions and progress instead of
  rewriting the body to look predetermined.
- A pull request that finishes an issue says `Closes #n`. Refer to another
  repository's issue as `<owner>/<repo>#n`.

If the labels are missing, create them once with `gh label create` only when
the user asked to set up the workflow.

## Legacy Change-Request Archives

Some repositories still contain a `change-requests/` folder of Markdown
`CR-xxx` files. Treat it as a read-only archive:

- Never add, edit, or renumber files in `change-requests/`.
- Read an archived `CR-xxx` file when code or docs cite that ID.
- Requests that were still open moved to issues that keep `CR-xxx` in the
  title. Find one with `gh issue list --search "CR-xxx in:title" --state all`.
- Do not allocate new `CR-xxx` IDs; new work gets an issue number.

## Move Ideas Through Three Gates

### Gate 1: Capture

Open an issue with a title and one or two sentences saying what should change
and why it matters now. Label it `idea` unless the user has accepted the work.
Most ideas should stay this light.

### Gate 2: Reconnaissance

Before planning, inspect the repository for the load-bearing fact that shapes
the work. Write `Context` from current evidence with paths cited, and list
`Open Questions` that could change the implementation. Use
`assets/issue.md` as the body outline for larger work.

### Gate 3: Decisions

Resolve open questions before implementation. Record each answer in a dated
comment with the evidence or constraint that settled it, and tick the question
in the body. Phase work by checks that can fail.

## Create An Issue

1. Search first to avoid duplicates:
   `gh issue list --search "<keywords>" --state all`.
2. Write the body to a temporary file, then create the issue:
   `gh issue create --title "<title>" --label <label> --body-file <file>`.
3. Confirm it reached the board. Auto-add normally handles this; if it did not,
   run `gh project item-add <number> --owner <owner> --url <issue-url>`.
4. Report the issue URL.

## Update An Issue

- Edit the body with `gh issue edit <n> --body-file <file>` after reading it
  with `gh issue view <n> --json body,labels,state`.
- Add progress and decisions with
  `gh issue comment <n> --body "YYYY-MM-DD: ..."`.
- Promote an idea with `gh issue edit <n> --remove-label idea --add-label change`.
- Set board status when it changes (see "Set Board Status"). Use `Blocked`
  only with a comment that names the blocker and the next action.

Do not infer implementation status from intention alone.

## Set Board Status

Board commands need the `project` token scope. If a command fails for scope,
ask the user to run `gh auth refresh -s project`, or to change Status on the
board in a browser, and carry on with the rest of the work.

1. Find the project and field IDs:
   `gh project view <number> --owner <owner> --format json --jq .id` and
   `gh project field-list <number> --owner <owner> --format json`.
2. Find the item ID for the issue:
   `gh project item-list <number> --owner <owner> --format json --limit 500`
   and match `content.url`.
3. Set the value:
   `gh project item-edit --project-id <pid> --id <item-id> --field-id <status-field-id> --single-select-option-id <option-id>`.

## Review The Backlog

Read the open issues, and the backlog-order issue when the project has one.
Check for:

- board status that disagrees with the issue state
- `In Progress` issues with no recent comment
- `Blocked` issues without a named blocker
- missing or weak acceptance criteria on `change` issues
- issues holding several outcomes, or duplicates
- closed issues without a recorded outcome

Do not open, close, merge, split, or relabel issues during a review unless the
user authorized it.

## Implement And Close

Do not begin implementation while an open question is unresolved. Resolve it
from repository evidence when possible; ask only when a choice would
materially change the outcome.

Close an issue only when its acceptance criteria are met and the outcome is
recorded:

- Prefer `Closes #n` in the pull request so merging closes it.
- Otherwise close with an outcome comment:
  `gh issue close <n> --reason completed --comment "Outcome: ..."`.
- For abandoned work use `--reason "not planned"` and say why.
- Confirm the board shows `Done`; set it when the board's own workflow did not.

## Report Changes

Summarize issues created or changed (with URLs), key findings and decisions,
open blockers or questions, and any board update you could not make.

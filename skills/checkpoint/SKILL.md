---
name: checkpoint
description: Create or restore an evidence-grounded, durable handoff so another agent can continue interrupted work without access to the original session. Use when asked to checkpoint, save progress, prepare a handoff, or resume from one.
---

# Checkpoint

Create or restore a durable handoff for interrupted agent work.

A checkpoint is a compact state snapshot, not a conversation summary. A fresh agent with no access to the original session should be able to understand the goal, distinguish completed from pending work, find the relevant artifacts, and continue without repeating work or external side effects.

## Save a checkpoint

### Ground the state

Inspect the sources that establish the current state, as relevant: the user's request, task or specification, current session, delegated-agent results, workspace, repository and branch, changed files, commits, verification output, and external records.

- Use the conversation to recover intent and decisions; use artifacts and tool state to support claims about what actually happened.
- Describe work from the current agent session explicitly. Record work from prior or delegated sessions only when it is visible, and preserve attribution when known.
- Distinguish `done`, `in progress`, `blocked`, and `not started`. Do not turn plans into completed work.
- Record material corrections, review comments, and user feedback that change the expected result.
- Use `unknown` for important facts that cannot be established. Do not guess.

### Choose a durable location

The checkpoint must be written to persistent storage. A response in transient chat alone is not a checkpoint.

Use this precedence:

1. A location explicitly requested by the user.
2. The existing durable source of truth for the work, such as a task, issue, specification, project note, or to-do item, when it supports an appropriate comment, note, or checkpoint section.
3. A Markdown file in the workspace. Follow an existing checkpoint or handoff convention; otherwise use `checkpoints/<task-slug>/<YYYYMMDDTHHMMSSZ>.md` under the workspace root.

Prefer an append-only comment, note, or new timestamped file so earlier checkpoints remain available. Do not require a particular task system, comment type, or magic marker. When the handoff must cross machines or worktrees, prefer a shared task system or a version-controlled path and state whether the file is committed.

Do not change task status, assignment, specification requirements, workflow state, or unrelated files. Do not commit, push, or publish beyond the chosen durable location unless the user also requested that action.

### Record the continuation state

Keep the checkpoint concise but self-contained. Include:

- **Checkpoint metadata** — creation time, task or source locator, workspace or repository identity, and agent or session identifiers when available.
- **Goal and success criteria** — the requested outcome, important scope boundaries, and what counts as done.
- **Current state** — a short status summary and the overall phase of work.
- **Session progress** — what this agent session completed, what is partial, and any known work performed by other agents or sessions.
- **Decisions and feedback** — decisions that constrain the continuation, brief rationale, rejected alternatives when they could otherwise be reconsidered, and material user or review comments.
- **Artifacts and changes** — exact paths, URLs, task/comment/PR IDs, branch and HEAD when applicable, plus staged, unstaged, and untracked changes. Separate task-owned changes from unrelated pre-existing changes.
- **Verification** — checks already run, their exact outcomes, supporting locators, and important checks not yet run.
- **External effects** — writes, messages, deployments, or other side effects already performed, with identifiers and enough evidence to avoid repeating them. State `none` when there were none.
- **Blockers, unknowns, and risks** — unresolved facts or conditions that affect continuation.
- **Next steps** — an ordered, executable list. Make the first item the exact next action and include its expected result.

Use precise locators instead of copying large diffs, logs, specifications, or conversation history. Never store secrets, credentials, private keys, tokens, or unnecessary sensitive data; point to an authorized secure source instead.

This is a useful default shape, not a required comment format:

```markdown
# Agent checkpoint — <task>

- Created: <ISO-8601 timestamp>
- Task/source: <durable locator>
- Workspace: <path or identifier>
- Repository/branch/HEAD: <values or not applicable>

## Goal
<outcome, constraints, and done condition>

## Current state
<status and phase>

## Session progress
- Done: <completed work and evidence>
- In progress: <partial work and exact state>
- Not started: <remaining scope>

## Decisions and feedback
- <decision or comment and why it matters>

## Artifacts and changes
- <locator and state; task-owned or unrelated>

## Verification
- <check: result>
- Not run: <important missing check and reason>

## External effects
- <effect, identifier, and retry warning, or none>

## Blockers and unknowns
- <item or none>

## Next steps
1. <exact next action> — Expected: <observable result>
```

### Confirm persistence

Read the saved artifact back when the storage supports it; otherwise require a successful write response containing a stable locator. Check that the saved content is complete and readable.

If the write fails, try the next suitable authorized durable location. If none succeeds, return the proposed checkpoint text and state clearly that no durable checkpoint was created.

Finish by reporting the checkpoint's exact locator and the first next action. Do not claim success until persistence is confirmed.

## Resume from a checkpoint

1. Locate and read the specified checkpoint. If none is specified, inspect the current task or source of truth and the workspace's checkpoint convention, then use the latest checkpoint relevant to the task.
2. Treat it as a starting snapshot, not unquestionable truth. Verify time-sensitive or high-risk state before acting: task status, repository identity, branch and HEAD, working-tree changes, referenced artifacts, blockers, and external side effects.
3. Report material drift or contradictions and update the working understanding. Preserve unrelated changes and never repeat an external effect merely because the checkpoint says it was attempted.
4. Continue from the first valid incomplete action. Reuse prior evidence when it is still current; do not redo discovery or verification without a concrete reason.

If the checkpoint is missing, ambiguous, or too stale to identify a safe continuation, ask only for the smallest missing locator or decision.

---
name: agents-add-task
description: Plan an initiative into implementation-ready Symphony tasks and create the validated dependency graph in the Todoist `_agents` project with the official `td` CLI. Use for adding one well-specified task or decomposing larger work; do not use inside a managed Symphony worker that forbids direct CLI access.
---

# Add Task

Create self-contained Todoist tasks that agents can implement without rediscovering product or
architectural decisions. For a larger initiative, perform the senior analysis first, split the work
into the smallest coherent dependency graph, and then create the complete set safely.

## Establish the plan

Before mutating Todoist:

1. Read the request and inspect the relevant repositories, current behavior, tests, documentation,
   and operational constraints.
2. Resolve product, architecture, ownership, data-source, error-handling, and verification decisions
   wherever the available evidence permits. Do not pass ordinary senior decisions to implementation
   agents as “investigate” or “decide”.
3. Split the initiative into independently implementable outcomes:
   - each task has one coherent goal and normally one repository;
   - each task can be verified independently;
   - parallel tasks should avoid overlapping ownership where practical;
   - dependencies express only real prerequisites, not a preferred reading order;
   - routine implementation steps, tests, commits, and PR creation are not separate tasks.
4. Draw the dependency graph and check that it is acyclic. Use `AND` semantics: a task with several
   dependencies waits for all of them.
5. Identify actions that truly require a human. Agents should perform all setup available through
   existing CLIs, APIs, consoles, or repository automation. Human work is limited to actions such as
   accepting legal terms, completing interactive identity or payment checks, creating an account
   that cannot be automated, or provisioning a credential the agent cannot safely obtain.

For a single straightforward task, apply the same quality bar without manufacturing a larger graph.

## Write implementation-ready descriptions

Use the delegation standard from `junior-delegate`: explain the problem before mechanics, make the
important decisions, name exact verified targets, and define observable verification. A task must be
understandable without the original conversation.

Use this body after the Symphony front matter. Omit `Out of scope` only when it adds no information.

```markdown
## Goal

<1–3 sentences describing the outcome>

## What is happening today

<current behavior and a concrete example when useful>

## What should happen instead

<desired observable behavior>

## Why

<mechanism and business/engineering reason>

## Key decisions

- <source of truth, ownership, fallback, normalization, or boundary>

## Changes

### `<exact file, function, service, dashboard, or procedure>`

**Change:** <what to change>

**Reason:** <why this target is involved>

**Expected behavior:** <contract after the change>

## Verification

- <specific command or observable check>

## Done when

- [ ] <concrete completion criterion>

## Out of scope

- <intentional boundary and why>
```

Do not leave placeholders in a created task. Include all known affected targets rather than asking
the implementation agent to find them. Preserve uncertainty only when it is genuinely external and
state exactly how it will be resolved.

## Symphony front matter

Repository-backed agent tasks begin with YAML front matter:

```yaml
---
symphony:
  repo: repository-name
  merge_policy: human
  merge_branch: integration
  depends_on:
    - 6XGgmFVcrG5RRjVr
    - https://app.todoist.com/app/task/6XAbcDef12345678
  stages:
    in_progress: gpt-5.6-luna:xhigh
    verify: gpt-5.6-sol:high
    rework: gpt-5.6-luna:xhigh
    merging: gpt-5.6-luna:xhigh
---
```

- `repo` is the exact repository name below the configured `repositories.root`. Omit it only for a
  genuinely non-repository deliverable.
- `merge_policy` defaults to `human`. Use `automatic` only when the task is explicitly allowed to
  integrate after a passing verification.
- `merge_branch` is optional and defaults to the repository branch selected by `origin/HEAD`.
  `automatic` uses provider auto-merge through a PR for `main`/`master`; another explicit target is
  updated directly by fast-forward push.
- `depends_on` contains native Todoist task IDs or canonical Todoist task URLs from `_agents`. Omit
  it when empty. Every dependency must reach `Done`; a Todoist checkmark is not completion evidence.
- `stages` is optional. Prefer the workflow defaults unless the task has a concrete reason to
  override them. Verification should use the independent stronger verifier profile.
- `task_id` is optional and is only for an intentional numeric branch identifier. Do not invent one;
  otherwise Symphony uses the native Todoist ID.
- Missing references, self-dependencies, and cycles are invalid and will move the task to `Blocked`.

## Human prerequisite task

If the initiative needs any human-only actions, create exactly one top-level task whose title starts
with `[Human]`, for example `[Human] Provision external access for Cloudflare rollout`.

- Keep the parent in `Backlog`; it must never be dispatched to an agent.
- Add one Todoist subtask per human action. Each subtask states:
  - the exact action and location;
  - why an agent cannot perform it;
  - the minimum permissions or scope required;
  - where the resulting credential or confirmation must be stored;
  - which agent task becomes unblocked.
- Never place secret values in task descriptions, comments, or command output. Ask the human to put
  credentials in the approved secret store or environment and record only that the secret exists.
- Do not add “human” work for commands, configuration, project creation, or deployment the agents
  can perform themselves.
- Agent tasks that require the human actions depend on the top-level `[Human]` task, not on its
  individual subtasks. After completing all subtasks, the human moves the parent to `Done` without
  using the Todoist completion checkmark as the workflow signal.
- Do not create a `[Human]` task when no human-only action exists.

## Create the Todoist graph safely

Use only the official `td` CLI and the `_agents` project.

1. Verify the account, project, and required sections before creating anything:

   ```sh
   td auth status --json
   td project view _agents --json --full --detailed
   td section list --project _agents --all --ndjson --full
   ```

   Confirm that `Backlog`, `Todo`, `InProgress`, `Verify`, `Blocked`, `HumanReview`, `Rework`,
   `Merging`, and `Done` exist exactly once.

2. Create every top-level task in `Backlog` first. This prevents Symphony from dispatching an
   incomplete graph. Use `--dry-run` for the first representative command, then create it and retain
   the returned JSON/ID:

   ```sh
   td task add "Implement explicit outcome" \
     --project _agents \
     --section Backlog \
     --stdin \
     --json <<'EOF'
   ---
   symphony:
     repo: repository-name
   ---

   <complete implementation-ready description>
   EOF
   ```

3. If required, create the single `[Human]` parent in `Backlog`, retain its ID, and add its subtasks:

   ```sh
   td task add "[Human] Provision external access for the initiative" \
     --project _agents --section Backlog --stdin --json <<'EOF'
   Complete only the human actions listed as subtasks. Agents own every other setup step.
   Move this parent task to Done after all subtasks are complete; do not complete the parent with a
   checkmark because Symphony uses sections as workflow state.
   EOF

   td task add "Create a scoped Cloudflare API token" \
     --parent id:<human-task-id> --stdin --json <<'EOF'
   Create the least-privileged token required for <agent task>. Store it in <approved secret store>
   as <secret name>; never paste its value into Todoist. This is human-owned because <exact reason>.
   EOF
   ```

4. Once all IDs exist, update dependent task descriptions with `depends_on`. Supply the entire
   description through stdin so the front matter and task body remain one consistent document:

   ```sh
   td task update id:<dependent-task-id> --stdin --json <<'EOF'
   ---
   symphony:
     repo: repository-name
     depends_on:
       - <blocker-task-id>
       - <human-task-id>
   ---

   <complete implementation-ready description>
   EOF
   ```

5. Re-read the complete project and validate before dispatch:
   - every referenced ID exists in `_agents`;
   - the graph has no self-reference or cycle;
   - each task has the intended repository and exact dependencies;
   - agent tasks are self-contained and have observable verification;
   - the `[Human]` task is in `Backlog` and no avoidable work was assigned to the human.

   ```sh
   td task list --project _agents --all --ndjson --full --show-urls
   td task view id:<task-id> --json --full --include-children
   ```

6. Only after the complete graph passes validation, move all agent-owned tasks to `Todo`, including
   dependent tasks. Symphony will start ready roots and keep dependent tasks queued automatically.
   Leave the `[Human]` task in `Backlog`.

   ```sh
   td task move id:<agent-task-id> --project _agents --section Todo
   ```

7. Re-read every created task and report the resulting task IDs/URLs, dependency edges, ready roots,
   waiting tasks, and human prerequisite task if present. Stop on any partial creation or validation
   failure; keep affected tasks in `Backlog` and describe the exact repair instead of dispatching an
   inconsistent set.

## Quality bar

- Prefer a small DAG of meaningful outcomes over many tiny sequential tasks.
- Minimize human effort and explain every retained human action.
- Never create duplicate tasks to recover from an uncertain CLI result; inspect `_agents` first.
- Never complete, delete, or archive tasks as part of this workflow.
- Do not move any task to `Todo` until the entire created set and its dependencies have been checked.

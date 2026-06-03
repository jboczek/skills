# App Docs Maintenance

This solution defines a lightweight documentation convention for repositories maintained by AI coding agents.

The main artifact is `AGENTS.md`. Copy it into another repository as:

```text
docs/AGENTS.md
```

That file tells agents where product and feature documentation belongs, how to navigate it, and what to update after implementation work.

## Concept

The target repository should keep product documentation under `/docs`:

```text
docs/
  AGENTS.md
  big_picture.md
  roadmap.md
  changelog.md
  prds/
  features/
```

The structure is intentionally small:

- `big_picture.md` explains what the application is, what problem it solves, and the broad direction.
- `roadmap.md` describes planned versions and links to PRDs when they exist.
- `prds/` contains PRDs for planned or active work.
- `features/` contains durable feature documentation that reflects implemented behavior.
- `changelog.md` records short dated notes about completed implementation changes.

## Usage

1. Copy `solutions/app_docs/AGENTS.md` into the target repository as `docs/AGENTS.md`.
2. Create the rest of the `/docs` structure when it becomes useful.
3. Let coding agents use `docs/AGENTS.md` as the local maintenance guide.

Agents may use existing skills to create or update individual documents:

- Use `big-picture` for `big_picture.md`.
- Use `roadmap` for `roadmap.md`.
- Use `to-prd` for files under `prds/`.

This solution does not duplicate those skill instructions. It only defines where documentation should live and what agents should keep current.

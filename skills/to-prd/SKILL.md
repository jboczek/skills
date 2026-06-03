---
name: to-prd
description: Turn rough feature ideas into concise PRDs for `docs/features/` with lightweight frontmatter, consistent sections, and right-sized scope. Use when creating or revising PRDs, feature specs, or product requirement documents.
---

# To PRD

## Quick start

Create each feature in a numbered folder under `docs/prds/`:

```text
docs/prds/
└── prd-001-feature-name.md
```

Use zero-padded three-digit numbers so features sort consistently.
  
Create PRDs with this frontmatter:

```yaml
---
title: Synthesis loop
summary: Turn raw repository inputs into reviewed wiki updates.
status: planned
roadmap: v0.1
---
```

Use these enums:

- PRD `status`: `idea | planned | in_progress | done | parked`

Then use this default body structure:

```md
# Feature name

## Context
## Problem
## Goal
## Non-goals
## User stories
## Proposed experience
## Requirements
```

User stories follow this format — make the list extensive, covering all aspects of the feature:

```
1. As a <role>, I want <capability>, so that <outcome>.
```

Optional sections when they add value:

- `## Success criteria`
- `## Edge cases`
- `## Dependencies`
- `## Implementation decisions` — modules to build or modify, interfaces, schema changes, API contracts, architectural decisions. Use domain glossary vocabulary; avoid specific file paths. If a prototype snippet encodes a decision more precisely than prose can, inline only the decision-rich parts.
- `## Testing decisions` — which modules get tests, what makes a good test for this feature (test external behavior, not implementation details), and pointers to similar existing tests.
- `## Tasks` using markdown checkboxes when execution progress should be visible in the rendered UI

## Workflow

1. **Explore the repo** to understand the current state of the codebase. Use the project's domain glossary vocabulary throughout the PRD, and respect any ADRs in the area you're touching.
2. Choose the next feature number and a short kebab-case slug.
3. Create the feature folder as `docs/prds`.
4. Create the PRD file as `prd-NNN-feature-name.md`.
5. **Synthesize first** — if the conversation context and codebase give you enough to draft, do so immediately without interviewing the user.
6. Leave NO open questions in the PRD. If you have questions, ask the user for the answers, then update the PRD with the answers. If you have assumptions, write them down and ask the user to confirm or correct them.
7. If important information is genuinely missing, ask only the few questions whose answers would materially change the PRD.
8. When the feature has non-trivial implementation scope, sketch the major modules to build or modify and confirm with the user before writing. Prefer deep modules — ones that encapsulate significant functionality behind a simple, testable interface — over shallow wrappers.
9. Keep the PRD product-focused: explain why the feature exists, what it should do, what it should not do, and what remains undecided.
10. Move detailed architecture and technical trade-offs into `docs/adrs/` instead of bloating the PRD; use `## Implementation decisions` only for the choices that reviewers need to understand the PRD.
11. Keep the scope to one coherent feature, one primary user problem, and one primary outcome.

## Size guidance

- Default target: **600–1,200 words**
- If it is under roughly **300 words**, it is probably still a note rather than a real PRD.
- If it grows beyond roughly **2,000 words**, it is probably too broad or mixing product and technical design.
- Prefer splitting the feature or moving detail elsewhere over making long PRDs the norm.

## Questions to pressure-test the idea

Ask questions only when the brief would benefit from sharper thinking. Prefer questions that test evidence, scope, and necessity over questions that merely fill the template.

Useful questions include:

- What have we actually observed that makes this feature necessary now, rather than merely plausible?
- What would users do today if we did not build this at all?
- What is the smallest version that would still create real value for the user?
- Which parts of this request solve a present need, and which parts are future-proofing?
- What assumption are we most likely to be wrong about?
- If we had to remove half the scope, what would we keep?
- Why is this worth doing before the next most valuable alternative?

## Quality bar

A finished PRD should let a reader quickly understand:

- why the feature exists
- what outcome it aims for
- what behavior is expected
- what is intentionally excluded
- how it should look like, whats' the design

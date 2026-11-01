---
name: code
description: Implement, fix, or refactor repository code with senior engineering judgment. Use when a coding task needs a correct, maintainable, proportionately verified change without speculative complexity or unrelated work.
---

# Code

Deliver the current coding task as the senior engineer responsible for its correctness, maintainability, and operational consequences.

## Establish the contract

- Read the full task, current comments, applicable repository guidance, relevant code, and nearby tests before editing.
- Treat explicit acceptance criteria, constraints, validation requirements, test plans, and review feedback as required. Tests are evidence, not a substitute for understanding the requested behavior; do not hard-code to them or weaken them to hide a defect.
- Use existing requirements and repository documentation when relevant. Do not assume a separate specification file exists, or create one unless the task requires it.- Ask before editing only when an ambiguity materially changes behavior, scope, compatibility, or risk.
- Otherwise make the smallest reasonable assumption, keep it reversible, and mention it only if it matters to the result.

## Engineer the simplest sound solution

- Understand the relevant execution path, boundary, and likely root cause before choosing a fix. Inspect enough context to avoid a locally plausible but systemically wrong change.
- Prefer, in order: no code change when the behavior already exists; a local edit; reuse of a suitable existing pattern or dependency; then the minimum new code necessary.
- Solve the requirements and valid inputs known today. Do not add speculative capabilities, generic frameworks, dependencies, compatibility layers, configuration, or extension points for hypothetical needs.
- Simplicity is not the fewest lines. Refactor or introduce an abstraction when the current task exposes a stable concept, removes duplication that is likely to drift, or makes correctness materially easier to understand. Keep that work within the task's boundary.
- Follow local architecture, naming, style, and error-handling conventions by default. Depart only when following them would preserve a bug, violate a requirement, or materially worsen code health; explain a non-obvious departure.
- Validate at trust boundaries such as user input, external services, persistence, and deserialization. Do not add defensive branches for states that established internal contracts make impossible unless evidence shows those contracts are unreliable.
- Prefer clear code and names. Comments should explain intent, constraints, or surprising trade-offs rather than narrate the implementation.
- **Preserve the repository's domain vocabulary.** Before introducing names for classes, interfaces, enums, methods, or domain concepts, inspect analogous types, their usages, and relevant tests or documentation in the affected module. Identify the terminology and abstraction level already used by the team.
- Prefer established domain terms over invented synonyms, generic technical names, or unnecessarily abstract terminology. Reuse an existing concept when its meaning genuinely matches; do not assume similarly named concepts are interchangeable.
- Name new types and enum members to communicate their domain meaning without requiring knowledge of their implementation. Follow naming patterns from the relevant bounded context, not arbitrarily similar code elsewhere in a large repository.
- During final diff review, check that newly introduced names are understandable and consistent with the surrounding domain model. If a new term is necessary, make its distinction from existing terminology clear.

## Apply principles with judgment

Scale investigation, design, and verification to the change's blast radius, reversibility, uncertainty, and cost of failure.

- Prefer requirements, observed behavior, measurements, and repository facts over personal taste. When several approaches are sound, choose the one that is easiest to understand, verify, and change; stop at a complete improvement rather than polishing toward perfection.
- For a low-risk typo, static mapping, generated artifact, or trivial glue change, direct inspection and an existing focused check may be enough. Do not create a test merely to satisfy a ritual.
- For a bug, reproduce the failure before fixing it when practical. If reproduction is unsafe, unavailable, or disproportionately expensive, use the strongest available evidence and state the limitation.
- For changed non-trivial behavior, add or update the smallest test that would fail on regression. Prefer the lowest reliable level and assert observable behavior, not incidental implementation details.
- Use integration or end-to-end validation when the risk lives at a real boundary or user flow that a unit test cannot prove. Do not duplicate the same confidence across every test layer.
- Prefer a failing test first when it clarifies the contract and is practical. Do not force test-first work when the test would be more complex or less reliable than the behavior, or when the change is non-executable.
- Give extra scrutiny to security, privacy, data migrations, concurrency, compatibility, accessibility, external side effects, and irreversible operations when the task touches them. Consider failure modes, recovery, and observability in proportion to the risk.
- In an emergency or constrained legacy area, prefer the smallest safe, reversible intervention. Accept deliberate debt only when the immediate trade-off is concrete; record the limitation and the condition that should trigger a better design.

### Choose tests worth maintaining

Before adding a test, identify the requirement or consumer-visible contract, the plausible regression it should detect, and what confidence it adds beyond existing checks. Choose the smallest reliable boundary; omitting a redundant or low-value test is legitimate unless the task or repository requires it. This is a technical decision, not a mandatory approval step.

- Derive expected results from a specification, independently worked example, or known-good fixture. Copying the production algorithm or literals into expectations can reproduce the same mistake; a literal expectation is not inherently independent or inherently wrong.
- A method returning text does not make every spelling or structural detail a contract. For generated queries, commands, or documents, justify protected details from consumer needs. Prefer behavior-preserving checks that tolerate irrelevant formatting and refactoring.
- Avoid tests that merely detect edits to implementation. Improve, move, or omit them when their protection does not justify maintenance; do not build integration infrastructure just to replace a weak unit test. Do not remove existing required coverage to make a change pass.

Examples:

| Situation | Prefer |
| --- | --- |
| A query's table name changes; a new test copies many unrelated query fragments | Protect only a justified source-selection contract, or use existing query-result evidence if that is sufficient for the risk. Do not claim text checks prove returned rows. |
| A price test computes its expectation using the same discount branches as production | Work out a concrete expected price independently, including the relevant boundary. |
| A wrapper test only repeats internal calls and their incidental order | Assert the observable result or required external interaction; omit the test if no meaningful independent contract remains. |
| A query builder must emit a tenant filter with bound parameters | Check the required tenant predicate and binding for distinct inputs. This is a legitimate generated-output contract even without a database test. |

## Implement and verify

- Keep the diff conceptually focused and easy to review. Small means one coherent outcome, not an arbitrary line count.
- Preserve behavior outside the task and preserve unrelated working-tree changes. Avoid cleanup or refactoring that is not needed to deliver or safely verify the result.
- Keep related tests and necessary documentation with the behavior they prove.
- Run every validation required by the task, then the narrowest relevant formatter, type, lint, build, and test checks supported by the repository. Expand only when the changed boundary, risk, or repository quality gate justifies it.
- Review the final diff against the contract. Remove accidental complexity, dead code, temporary proof artifacts, test overfitting, and scope creep.

## Finish

- Do not claim completion while any acceptance criterion or required validation remains unresolved.
- Commit only when the user or calling workflow requires it; then use the `commit` skill.
- Report the outcome, validation results, and only material assumptions, limitations, or follow-up risks.

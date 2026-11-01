---
name: prep-req
description: Turn rough product ideas, business requests, and task discussions into concise, reviewable requirements covering the problem, desired outcome, expected behavior, scope, and acceptance criteria. Use before technical planning or when refining business-facing feature requirements. Not for architecture or implementation plans.
---

# Prepare requirements

Turn the available context into a business/product requirement that stakeholders can validate and engineers can use for technical planning.

Describe **why and what**, not **how to implement it**. Write only as much as needed to remove meaningful ambiguity.

## Understand the request

- Read the request, relevant discussion, existing product documentation, business rules, and provided designs. Inspect the repository only when necessary to verify current behavior or material constraints.
- Identify who is affected, what problem exists today, what should change, and why it matters. Do not invent evidence, benefits, or metrics.
- Preserve agreed decisions and the project's domain terminology. Distinguish confirmed facts, explicit requirements, proposals, assumptions, and open decisions; never silently turn one into another.
- Explain relevant current behavior from the user's or business process's perspective. Consider other affected roles and downstream outcomes when material.

## Shape the requirement

- Look past a suggested feature or UI element to understand the underlying need, without disregarding an explicitly agreed product decision.
- Clarify expected behavior, important scenarios, business rules, permissions, exceptions, and boundaries. Include only details that materially affect the outcome.
- State non-goals where they prevent scope creep. Do not expand the request with speculative enhancements or future-proofing.
- Keep business constraints, including relevant compliance, timing, or user-experience requirements. Leave architecture, API contracts, data schemas, module design, test organization, and task breakdowns to technical planning.
- Draft from the available evidence first. Ask only focused questions whose answers would materially change the requirement. Do not decide unresolved product questions on the user's behalf; record them with their impact.

## Write the requirement

Adapt the structure to the size of the change. A useful default is:

1. **Problem and outcome** — affected users, current problem, desired result, and reason for the change.
2. **Expected behavior** — main scenarios, business rules, intended experience, and design references when available.
3. **Scope and constraints** — what is included, what is excluded when relevant, and material restrictions or dependencies.
4. **Acceptance criteria** — outcomes that can be checked from a user or business perspective, without prescribing technical implementation.
5. **Open decisions** — only material questions still requiring a product or business decision, and what each decision changes.

Merge or omit sections that add no value. Use a small example, decision table, or user flow only when it clarifies complicated behavior. User stories and success metrics are optional; do not manufacture them to fill a template.

Write in Polish by default unless another language is requested. Use plain, concrete language and preserve exact domain terms and identifiers. Do not repeat the same rule across sections.

## Deliver and check

- When asked for analysis only, provide the analysis without editing files. When asked to create or revise a document, use the requested location or existing repository convention; update the existing document rather than creating a competing version.
- The result should let someone unfamiliar with the conversation answer: **Why is this needed? Who is affected? What exactly must happen? What is outside scope? How will we recognize an acceptable result? What remains undecided?**
- Stop at the product boundary. The `implementation-plan` skill handles the technical approach, responsibilities, flows, and architectural consequences.

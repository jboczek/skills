---
name: implementation-plan
description: Create or shorten a preliminary implementation plan for human review, grounded in a task, discussion, and repository code. Use before implementation to agree on behavior, scope, and architectural direction. Not for executing implementation or producing detailed execution tasks.
---

# Preliminary implementation plan

Create a document that lets a human understand the purpose of a change, its impact on the current process, and the decisions requiring agreement. Keep details that affect the assessment of the solution; leave mechanical steps to implementation.

**Write the plan and accompanying explanations in Polish by default**, including headings, tables, and diagram labels, even when the request or sources are in English. Follow an explicit request for another output language. Preserve exact technical identifiers, API names, and required literal values.

Use plain sentences and short bullets. Around 500–700 words is a useful starting point for a medium-sized change, not a limit. Do not remove an important rule, consequence, or useful diagram to meet a word count.

## 1. Establish the evidence

- Read the task, available discussion and comments, related contracts, and relevant repository code. Retrieve older conversation pages when available. Account for all user corrections, not just the latest message.
- Distinguish **current behavior confirmed in code**, **agreed target behavior**, **your proposal**, and **an open question**. Do not present a proposal as a source requirement. Clear wording beside each decision is usually enough; a separate register is unnecessary.
- Check whether implementation comments supersede the original contract description. Do not invent DTO names, package versions, or environment settings. Identify specific evidence gaps when a source is unavailable.
- Trace the current flow: where data comes from, who transforms it, who makes the decision, and who consumes the result. For similarly named components, inspect implementations and call sites; a class name alone does not establish responsibility.
- Inspect direct consumers of the changed result and their downstream behavior. **Silence in the task does not establish a decision to leave behavior unchanged.** If a material impact remains undecided, ask a focused question explaining the consequences of the available options.
- Ask about ambiguities affecting behavior, scope, or architecture, rather than mechanical details. You can draft the confirmed parts while clarification is pending. Do not settle a missing product decision on the user's behalf.

## 2. Propose the solution through responsibilities

- Start with the problem or expected business outcome. Do not invent benefits unsupported by the sources. Briefly explain current behavior and the intended change.
- Introduce terms before using them. During a migration, identify the old and new approaches and explain their domain names. The reader should not need the entire previous conversation.
- Describe precedence rules, collisions between old and new mechanisms, and behavior when neither produces a result. Use a small decision table for multiple combinations instead of repeating rules in prose.
- For a material architectural change, name the component, its responsibility, and the reason for changing it. Retain decisions such as separating old logic for later removal. Do not introduce unnecessary layers or classes.
- Name components by responsibility. Distinguish similar business concepts and their data sources. A `V2` suffix does not explain different responsibilities; use version labels when they actually represent versions of the same contract.
- For integrations, identify the source of truth, where data is fetched, and the boundary of batch operations when correctness or cost depends on it. Include transport or JSON details only when necessary to assess a decision.
- Separate intended behavior worth preserving from a known implementation defect. A request to work like the old mechanism requires inspecting that mechanism, rather than copying its defects. Explain any proposed fix and identify whether it is agreed or proposed. Do not independently expand scope to unrelated repairs.

## 3. Show the flow and consequences

For a change spanning multiple stages or components, include one simplified Mermaid diagram. Show the current context, where the change connects, and material consumers of the result. Mark new and changed elements and unresolved dependencies; include a short legend. Do not inventory every class. A simple local change may need only prose.

Explain technical decisions through their consequences for the process:

- **Caching:** what is cached, for how long, and how this affects when changes become visible. Expiration alone does not imply an automatic refresh or publication of the result.
- **Failures:** what happens during an outage, which stage stops, and when a retry is possible, to the extent confirmed by the code. Distinguish a valid empty result from missing knowledge caused by a failure.
- **Configuration and filters:** when the new rule actually takes effect and which existing constraints still apply. Qualification alone may not imply unconditional execution of an action.

Determine the existing mechanism and required behavior for each task. Do not carry concrete values or failure policies from a previous solution into a new plan without evidence.

## 4. Write a concise plan

If the user requests analysis only, present a proposal without editing files. When asked to create or update a document, save it in the requested location or follow the repository's existing convention. Update an existing plan instead of creating competing versions.

Adapt the structure to the change. The default outline is below; translate its headings into Polish for the output:

1. **Purpose and current behavior** — the problem, outcome, and necessary introduction.
2. **Target rules** — behavior and a decision table when useful.
3. **Architecture and flow** — key responsibilities, rationale, and diagram.
4. **Scope and consequences** — boundaries, material constraints, and failure behavior.
5. **Open decisions** — only genuinely unresolved matters and their impact.
6. **Acceptance criteria and sources** — a short list of verifiable outcomes and the evidence behind the decisions.

Combine sections when this improves readability. Mark the document as a plan awaiting approval; identify incomplete decisions when present. Do not imply that the user has approved the plan unless they have.

State each rule in one place. Acceptance criteria may refer to the decision table. Use the diagram to explain connections and prose to explain responsibilities and reasons; neither needs to repeat the entire document.

Omit file inventories, DI registrations, field and logger names, exhaustive test doubles, and line-by-line implementation instructions. Retain a component name or code link when it clarifies a solution boundary. Express acceptance criteria as observable behavior, rather than a required number of tests.

## 5. Check consistency before delivery

- Can a new reader understand the purpose and terminology without opening the conversation history?
- Are responsibilities and current behavior grounded in evidence, with proposals and gaps made explicit?
- Have material consumers been checked, without treating silence in the task as agreement to leave them unchanged?
- Are the consequences of caching, failures, and configuration understandable without translating jargon?
- Does the diagram show where the change connects, with arrows consistent with the description?
- Has the latest agreement replaced older assumptions everywhere affected, including the diagram, scope, open questions, and acceptance criteria?
- Can a human assess the direction without reading an implementation manual?
- Is the plan consistently in Polish, unless the user explicitly requested another language?

Deliver a link to the document and briefly identify any material open decisions. Continue already authorized documentation work without asking for the same approval again. This skill does not itself initiate implementation.

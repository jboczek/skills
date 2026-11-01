# Evaluate test value

Assess protection, independence of expectations, and maintenance cost together. A test need not prove the whole system to be useful. Use these examples as decision aids, not automatic findings.

## Query text that mirrors implementation

Production emits two named query pipelines, both joined by product and shop. A test searches the entire output for the shared join clause and several aggregation fragments.

- Removing the shop key from one pipeline can leave the global assertion satisfied by the other. This is a concrete coverage limit, not proof that the submitted query is wrong.
- Renaming a local query alias or changing equivalent syntax may require updating expectations without changing result rows. Explain that coupling when assessing maintenance cost.
- A check for the chosen source can still protect a useful narrow regression. Do not dismiss it just because it cannot catch every query bug, or defend all other assertions just because this one is useful.

Possible recommendation: retain justified source checks and remove redundant fragments; scope any necessary structural checks to their pipeline. If the required proof concerns returned offers, distinguish existing query-result evidence from text assertions. Do not demand a new database environment by default.

## Independent behavioral expectation

A rule gives a 10% discount at a qualifying threshold. A test that copies the same threshold branch and rounding algorithm to calculate its expected value can share a production mistake.

A stronger case uses a worked example from the rule: qualifying input 100 yields 90, with separate cases around the actual threshold and rounding requirement. Expected literals are useful when independently justified; literal assertions are not inherently implementation coupling.

## A legitimate generated-output contract

A query builder must isolate tenants and pass user input through bound parameters. Tests inspect the tenant predicate, parameter names and actual bound values across different inputs, without requiring incidental whitespace or alias choices.

Those properties matter to the consumer and security contract. They can justify focused unit tests even when no query engine is run. These checks still do not establish query-result correctness; report the boundary honestly.

## Internal-call assertions

A service test mocks every collaborator and asserts that helper A precedes helper B, although neither order nor those helpers is required by callers. A harmless refactor breaks the test, while incorrect collaborator results may remain invisible.

Prefer an observable outcome or remove the redundant test. In contrast, a required external action such as sending exactly one payment request with the correct amount can justify interaction assertions. Mock use or call-count checks alone are not defects.

## Classify the result

- **Finding:** an important required behavior lacks protection and the change claims to provide it; cite the missed scenario and its impact. Do not infer a runtime defect from test weakness alone.
- **Non-blocking recommendation:** copied fragments or unnecessary coupling create a specific, limited maintenance/confidence cost without establishing a material requirement failure. Recommend keeping, narrowing, moving, or removing the relevant checks.
- **No issue:** a narrow, independently justified contract has proportionate protection. Acknowledge its limits without manufacturing a replacement test or infrastructure requirement.

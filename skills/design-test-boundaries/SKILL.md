---
name: design-test-boundaries
description: Derive language- and framework-independent test boundaries and risk-based cases from requirements, code paths, change diffs, incidents, and existing tests. Use for テスト設計, テスト境界, 「どこにどんなテストを作るか」, mock/fixture decisions, or when an AI coding agent needs to design or review unit, component, integration, contract, persistence, or end-to-end tests; explain why an existing test exists; identify gaps or duplication; or produce a test plan, matrix, implementation order, or beginner-friendly walkthrough for .NET/C#, ASP.NET Core, Blazor, Java/Spring, JavaScript/TypeScript UI frameworks, Python, Go, or other web, desktop, mobile, batch, and event-driven stacks.
---

# Design Test Boundaries

## Core outcome

Turn a change into a traceable test design:

`contract → execution path → boundaries → risks → test levels → cases → evidence`

Classify a test by the dependencies it actually crosses, not by its filename, command, or team label. A test called "UT" that starts a database or browser is an integration or system-level test for design purposes.

Treat xUnit, MSTest, NUnit, JUnit, Vitest, pytest, Playwright, and similar products as test tooling, not as test boundaries. Determine the responsibility and crossed dependencies first, then use the repository's existing toolchain where it fits.

## Establish evidence

1. Determine whether the request is to design, explain, review, or implement tests.
2. Inspect the smallest relevant test basis: requirement or acceptance criteria, current behavior, change diff, execution path, existing tests, and CI configuration.
3. Pin repository claims to a branch, commit, PR head, or supplied artifact when possible.
4. Separate verified behavior, inference, proposed policy, and unknowns.
5. Keep external writes and code changes outside scope unless explicitly requested.

If requirements and code disagree, expose the conflict before deriving expected results. Do not let current implementation silently become the specification.

Keep the current production architecture as the test-design baseline. If an architecture choice prevents atomicity, observability, isolation, or testability, report that constraint and separate any design option from the test recommendation. Do not silently introduce an outbox, new service, schema, endpoint, or concurrency mechanism.

## Normalize the technology stack

Before choosing test levels, identify the actual runtime profile:

- entry point and hosting or execution model;
- UI, API, CLI, job, event, or device boundary that starts the behavior;
- component that owns business state and invariants;
- process, network, thread, or trust boundaries crossed;
- persistence technology and production provider;
- external systems, operating-system services, clocks, devices, and schedulers;
- test libraries, fixtures, and CI commands already in use.

Map concrete classes, components, handlers, stores, services, and repositories to architectural roles. Do not force a Controller/Service/Repository shape onto a codebase that does not have one. Load [stack-boundary-map.md](references/stack-boundary-map.md) when adapting the method to a concrete language or framework.

## Derive the design

### 1. State the contract

Write one short statement for each behavior:

```text
Given <precondition>, when <action>, then <observable outcome>, while <invariant> remains true.
```

Also record exclusions and the failure impact. Prefer business-observable outcomes over internal method calls.

### 2. Trace the execution path

Follow the request or event through the system. For every node, record:

- responsibility;
- input and output;
- collaborator or side effect;
- state read or changed;
- failure that matters.

Mark a boundary when responsibility, data representation, state ownership, trust, process, runtime, thread, or external dependency changes. Load [boundary-catalog.md](references/boundary-catalog.md) when the path spans multiple layers, processes, devices, or services.

### 3. Assign one owner for each contract

Choose the smallest test that directly observes the contract with sufficient fidelity:

- pure calculation or invariant → unit test;
- rendered or native UI contract → component or focused UI test;
- UI, controller, command, job, or message-handler orchestration → orchestration test with responsibility-external collaborators replaced;
- serialization, adapter, client state, protocol, or outbound request mapping → adapter test;
- framework routing, binding, authentication, hosting, transaction, query, or persistence → focused integration test with the real framework, runtime, or database;
- independently deployed service interface → contract test where appropriate;
- critical user journey across the deployed stack → a small number of E2E tests.

Do not repeat the same assertion at every level. A component test should not prove database persistence; an API test should not prove button text.

### 4. Derive cases from risk

Start with the representative success path, then select only applicable variations:

- valid and invalid equivalence partitions;
- boundaries and just-inside/just-outside values;
- null, missing, empty, default, and malformed representations;
- permission, ownership, authentication, and applicable request-forgery boundaries;
- dependency timeout, rejection, partial failure, and retry;
- state transition, reopen, duplicate action, idempotency, and concurrency;
- time zone, date rollover, ordering, pagination, and locale;
- compatibility and schema evolution.

Use [case-design-techniques.md](references/case-design-techniques.md) to select a systematic technique. Do not create every category mechanically.

Group equivalent cases and default to the smallest decision-ready set. Separate must-have regression evidence from optional hardening instead of listing every conceivable failure.

### 5. Choose real dependencies and test doubles

Use the real dependency when the connection itself is the contract, such as SQL behavior, framework binding, serialization, transaction handling, or authentication middleware.

Use a mock, fake, or stub when the collaborator is outside the current responsibility and replacing it preserves the behavior under test. Replace only the narrow seam needed to force a rare failure.

Prefer the highest fidelity that keeps the test deterministic, isolated, and reasonably fast. Avoid mocking code owned by the same unit merely to mirror its implementation.

### 6. Design data and isolation

Specify fixture scope, seed ownership, cleanup or rollback, clock and ID control, and test independence. Require every test to pass alone and in arbitrary order. Do not rely on data left by another test.

### 7. Remove duplication and expose gaps

For every proposed case, ask:

1. Which unique contract or risk does this protect?
2. Is a lower, faster level sufficient?
3. Does another test already provide the same evidence?
4. Would this test fail for the intended regression rather than an incidental implementation change?

Identify missing E2E, contract, failure-path, or non-functional checks as risks, not automatic defects. Do not optimize for line coverage alone.

## Explain or review existing test code

Explain each test in this order:

1. responsibility and boundary;
2. Arrange/Given;
3. Act/When;
4. Assert/Then;
5. real dependencies versus doubles;
6. regression or risk protected;
7. overlap, brittleness, or missing evidence.

Translate framework helpers such as fixtures, lifecycle hooks, dispatchers, synchronization contexts, promise flushing, reactive renders, dependency injection overrides, and mock hoisting into their timing or isolation purpose.

- Load [stack-boundary-map.md](references/stack-boundary-map.md) to translate an unfamiliar or mixed stack into common responsibilities.
- Load [dotnet-blazor-profile.md](references/dotnet-blazor-profile.md) for the C#/.NET, ASP.NET Core, Blazor, and EF Core technology profile.

## Produce the output

For normal chat, lead with a compact decision brief:

1. contract and material unknowns;
2. recommended boundary allocation;
3. must-have test conditions grouped by unique rule, transition, or failure;
4. mock/real dependency decisions;
5. next implementation step.

Do not expand each representative value into a separate row unless executable cases or a formal artifact are requested. Put optional hardening after the core design and stop when the user can make the next decision.

For a detailed or reusable artifact, include:

1. scope and pinned evidence;
2. runtime profile and concrete technology-to-role mapping;
3. execution path and boundary map;
4. boundary-to-test-level table;
5. prioritized case matrix with requirement/risk traceability;
6. real dependency, mock, fixture, and data decisions;
7. implementation order;
8. exclusions, unknowns, and residual risks.

Use [test-design-template.md](references/test-design-template.md) when a reusable artifact is requested. Keep beginner explanations concrete and introduce framework vocabulary only after explaining its purpose.

## Guardrails

- Do not invent requirements to fill a matrix.
- Do not infer hosting, transport, persistence, or state ownership from a framework name alone; inspect the actual project.
- Do not prescribe a new test runner or mocking library when the existing toolchain can express the required evidence.
- Do not choose business semantics such as status codes, date inclusivity, retry counts, or same-value updates when they are not specified; expose the decision point.
- Do not redesign production architecture inside a test plan. Present a testability concern and any architecture option separately.
- Do not treat a passing status code alone as proof of correct state.
- Do not assert private implementation details when an observable contract is available.
- Do not prescribe a fixed unit/integration/E2E ratio; base the mix on risks and boundaries.
- Do not add broad snapshots, arbitrary sleeps, shared mutable state, or order-dependent tests.
- Do not claim a test passed unless it was executed against the stated version.
- Distinguish a design gap, an unverified risk, and a confirmed defect.

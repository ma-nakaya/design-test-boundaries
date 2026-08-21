# Boundary catalog

Use this catalog after tracing the execution path. Select only boundaries present in the task.

| Boundary | Contract to protect | Preferred evidence | Usually replace | Keep real when |
| --- | --- | --- | --- | --- |
| Pure domain logic | input maps to result while invariants hold | unit test | I/O, clock, random source | rarely needed outside the unit |
| Rendered or native UI | parameters/state render correctly and user action emits the right command or event | component or focused UI test | navigation, network, complex child widgets, OS services | browser/native runtime, accessibility, dispatcher, or platform behavior is the contract |
| Entrypoint orchestration | lifecycle and user, command, job, or message actions call the correct collaborators and update owned state | orchestration test | stores, navigation, scheduler, modal library, child components | framework binding, hosting, or dispatch behavior is under test |
| Client/application adapter to remote boundary | endpoint or protocol, payload, response normalization, and local state transition are correct | adapter test | live remote system | serializer, SDK, HTTP client, or protocol integration is the risk |
| Framework/transport entrypoint to application | route or command binding, validation, authorization, error mapping, and response contract are correct | framework integration test | unrelated downstream systems | middleware, hosting, binding, and framework behavior are the contract |
| Application to database | query, mapping, constraints, transaction, ordering, and persistence are correct | database integration test | unrelated services | SQL/database semantics are the contract |
| Service to service | producer and consumer agree on schema and semantics | contract test plus focused integration | remote availability | protocol/security/SDK behavior is the contract |
| Authentication/authorization | identity, permission, ownership, and applicable request-forgery boundaries are enforced | security-focused integration test | identity provider only when outside scope | middleware, cookie/token handling, or policy integration is under test |
| Queue/event boundary | publish/consume schema, acknowledgement, retry, ordering, and idempotency are correct | contract and integration tests | broker for pure handler logic | broker semantics or delivery behavior matter |
| Batch/job boundary | input selection, chunking, checkpoint, transaction, restart, and duplicate execution preserve the outcome | unit plus focused integration | scheduler for pure job logic | scheduler, transaction, or restart behavior is the contract |
| Device/OS/process boundary | protocol, encoding, permissions, lifecycle, timeout, reconnect, and cleanup are correct | adapter plus focused integration or simulator test | physical device or OS service for pure mapping logic | real driver, device, IPC, or platform behavior is the risk |
| File/object storage | encoding, path/key, metadata, cleanup, and errors are correct | adapter plus focused integration | remote storage | provider behavior is the risk |
| Time/concurrency | clocks, deadlines, races, retries, and duplicate actions preserve invariants | deterministic unit/integration test | real clock, scheduler, nondeterministic delays | actual database locking or runtime scheduling is the contract |
| Deployed user journey | critical business outcome works across the stack | minimal E2E test | nonessential third parties | the deployed wiring itself is the risk |

## Selection rules

1. Place a contract at the lowest level that can observe it faithfully.
2. Add a higher-level test only for integration risk that the lower test cannot detect.
3. Prefer one focused integration boundary over a broad E2E test when both catch the same defect.
4. Treat database, filesystem, network, multiple processes, and browser automation as evidence that the test is larger, regardless of its directory name.
5. Keep larger tests fewer, isolated, and focused on high-impact paths; do not enforce a universal percentage split.

## Common allocation mistakes

- Mocking the database while claiming to verify SQL.
- Testing a store's private helper instead of request and state contracts.
- Repeating the same happy path in component, view, API, and E2E tests without a distinct risk.
- Using E2E to cover validation combinations that a lower-level decision-table test can cover.
- Calling a test a unit test because the CI step is named `UT` even though it starts external infrastructure.

## Cross-cutting quality risks

Check these only when requirements, change impact, incidents, or domain risk make them material.

| Quality risk | Evidence to consider |
| --- | --- |
| Security/privacy | authorization matrix, ownership isolation, injection resistance, secret/PII exposure, auditability |
| Reliability/recovery | timeout, retry, idempotency, partial failure, rollback, restart recovery |
| Performance/capacity | latency budget, query count, load/concurrency limit, payload size, resource growth |
| Accessibility/usability | keyboard path, focus, labels/roles, error identification, screen-reader semantics |
| Compatibility/migration | old/new schema, client version skew, data migration, rollback, feature flag states |
| Observability | actionable logs/metrics/traces, correlation ID, audit event, failure alerting |

Do not convert this table into a mandatory checklist. Record excluded quality risks and why only when the omission is decision-relevant.

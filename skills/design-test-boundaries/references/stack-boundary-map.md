# Stack boundary map

Use this map to translate concrete technologies into test responsibilities. Inspect the repository first. The examples are vocabulary aids, not required architecture or tool choices.

## Map by responsibility

| Responsibility | C#/.NET | Java/JVM | JavaScript/TypeScript | Python | Go |
| --- | --- | --- | --- | --- | --- |
| Rendered or native UI | Blazor/Razor component, WPF/WinUI/MAUI view or ViewModel, WinForms form | server template, Android UI, or a separate web UI | Vue/React/Svelte component, page, hook, composable, or store | server template or the project's separate UI | server template or the project's separate UI |
| Application/domain | use case, application service, handler, domain object | use case, service, handler, domain object | service/use-case module, reducer, domain function | service/use-case function, domain object | use case, application service, domain function |
| Outbound adapter | typed `HttpClient`, SDK wrapper, repository implementation, mapper, clock | API client, SDK wrapper, repository implementation, mapper, clock | `fetch`/HTTP wrapper, SDK, mapper, queue/cache adapter | API client, serializer, repository/storage adapter | interface implementation for HTTP, queue, file, device, or clock |
| Framework/transport entrypoint | ASP.NET Core controller, Minimal API, middleware, model binding, hosted service | Spring MVC/WebFlux controller, security/filter chain, message listener | route/controller/middleware/schema in the actual server framework | Flask/FastAPI/Django route, dependency, decorator, middleware | `net/http` or selected router handler, middleware, decoder/encoder |
| Persistence | EF Core `DbContext`, LINQ query, migration, transaction, concurrency | JPA/JDBC query, migration, transaction | ORM/query builder/raw SQL, migration, transaction | SQLAlchemy/ORM/raw SQL, migration, transaction | `database/sql`, selected ORM, query, migration, transaction |
| Deployed journey | real Blazor/native/web entrypoint through required services and data | real UI/API through required services and data | real browser/API through required services and data | real browser/API/CLI through required services and data | real browser/API/CLI through required services and data |

## Adapt an unlisted stack

For each concrete element, answer:

1. What externally observable contract does it own?
2. Does it contain a pure rule, coordinate collaborators, translate representations, or connect to infrastructure?
3. What state does it own or change?
4. Which runtime, process, thread, trust, or network boundary does it cross?
5. Which failure can only be exposed with the real framework, platform, device, or provider?

Map the answers to domain, orchestration, adapter, framework integration, persistence integration, contract, or E2E evidence. If a layer does not exist, mark it out of scope instead of inventing it.

## Platform-specific cautions

- Blazor: identify the actual render mode and hosting model. A component test does not prove browser JavaScript, a server circuit, WebAssembly startup, HTTP wiring, or deployed authentication.
- WPF, WinUI, MAUI, and WinForms: separate view or command behavior from dispatcher, window lifecycle, native control, file/device, and operating-system integration risks.
- Vue, React, and similar UI frameworks: classify a store, hook, or composable by its responsibility. Pure rules, UI orchestration, and HTTP mapping may need different test boundaries even when stored together.
- Spring, ASP.NET Core, Flask, Node servers, and Go servers: start the real framework when routing, binding, dependency injection, middleware, security, or error mapping is the risk.
- ORM-backed code: an in-memory substitute does not prove production SQL, constraints, collation, transaction, or provider-specific behavior.
- Batch and workers: include restart, checkpoint, duplicate delivery, cancellation, and partial-failure boundaries only when the job's contract makes them relevant.
- Device and IPC code: use a fake for pure protocol decisions, then add focused evidence with the real driver, simulator, process, or hardware when that connection is the risk.

Use the test runner and mocking library already adopted by the project unless a verified limitation prevents the required evidence.

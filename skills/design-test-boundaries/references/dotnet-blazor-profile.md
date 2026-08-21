# C#/.NET and Blazor profile

Use this profile for .NET-shaped systems. Confirm the actual project structure, render mode, hosting model, database provider, test runner, and CI commands before applying it.

## Confirm the Blazor runtime profile

| Application shape | Common execution path | Boundary implication |
| --- | --- | --- |
| Interactive Server | Razor component → application service → persistence | there may be no HTTP API boundary between the component and service; server circuit and reconnect behavior are separate runtime risks |
| Interactive WebAssembly | Razor component → `HttpClient` → ASP.NET Core API → application service → persistence | client mapping and server HTTP contracts are separate boundaries |
| Static SSR | browser request/form → ASP.NET Core pipeline → component or handler → application service | binding, redirects, form processing, and applicable antiforgery behavior are material |
| Interactive Auto or mixed modes | initial server render → selected interactive runtime | test both phases only when the transition affects the contract |

Do not infer the shape from the word "Blazor." Inspect the render-mode declarations, project references, endpoints, and deployed topology.

## Example execution path

```text
Blazor/Razor component or native .NET UI
  → injected client/application service
  → ASP.NET Core endpoint or worker entrypoint
  → application/domain behavior
  → EF Core or another persistence adapter
  → production-compatible database

UI side boundaries may also include NavigationManager, EditContext,
authentication state, IJSRuntime, dispatcher, OS APIs, files, or devices.
```

Omit elements that do not exist. A Blazor WebAssembly client, interactive server component, static SSR page, hosted worker, desktop application, and API-only service cross different runtime and network boundaries.

## Boundary allocation

| Test target | Contract | Representative evidence | Dependency decision |
| --- | --- | --- | --- |
| Pure C# domain or application rule | invariant, calculation, decision, and state transition are correct | valid/invalid partitions, boundaries, repeated command, cancellation or time rule when applicable | keep framework, database, clock, network, and UI outside the unit |
| Blazor/Razor component | parameters and cascading values render correctly; input and events produce the owned UI outcome | loading/empty/error states, form validation, event callback, parameter change, same-item reopen, duplicate submit | use bUnit or the existing component harness; replace responsibility-external services and JS interop |
| Native .NET UI or ViewModel | binding, command, validation, selection, and owned presentation state are correct | initial state, command enablement, change notification, repeated selection, cancellation | keep pure ViewModel tests free of dispatcher/OS dependencies; add focused UI evidence only for platform behavior |
| Client or infrastructure adapter | URI/protocol, DTO, headers, serialization, response normalization, and error translation are correct | request shape, null/default mapping, cancellation, timeout/error mapping | replace the live remote system; keep the serializer/client pipeline real when it is the risk |
| ASP.NET Core entrypoint | route, model binding, filters/middleware, authorization, DI wiring, serialization, and error mapping work together | valid/invalid request, auth policy, not found/conflict mapping, unchanged state after rejection | use a focused test host such as `WebApplicationFactory` when these framework contracts matter |
| EF Core/database boundary | LINQ translation, query results, constraints, transaction, concurrency, and persistence match production expectations | provider-specific query, ordering, uniqueness/FK, rollback, optimistic concurrency | prefer the production database provider or a compatible isolated instance when database semantics are the contract |
| EF Core migration | a supported prior schema becomes the intended new schema without losing required data or constraints | apply the migration to representative prior data and verify the new model and rollback/recovery policy where specified | keep the production provider and representative prior schema real |
| Critical journey | the selected deployed UI, API, authentication, and persistence wiring deliver a business outcome | authenticate → perform one high-value action → observe durable result | use a real browser or application host only for critical wiring and platform risks |

## Key decisions

### Component test versus browser test

Use a component test for Razor rendering, parameters, event handlers, component lifecycle, injected-service interactions, and semantic output. Use browser-level evidence when the contract depends on CSS, real DOM/browser APIs, JavaScript execution, navigation, render-mode transition, WebAssembly startup, server-circuit behavior, or deployed wiring.

Mocking `IJSRuntime` can prove that a component requests the correct interop call. It cannot prove that the JavaScript implementation works in a browser.

### ASP.NET Core boundary

Use a real test host when routing, binding, middleware, authentication/authorization, filters, dependency-injection composition, or serialization is the reason for the test. Replace only downstream systems that are outside the responsibility. A direct controller-method test does not prove the HTTP/framework pipeline.

### EF Core boundary

Do not use the EF Core InMemory provider or an in-memory collection to claim proof of production SQL, LINQ translation, relational constraints, transactions, collation, or provider-specific functions. A mocked repository can isolate application behavior, but it does not test the repository query itself. Use the real provider in an isolated test database when those semantics are material.

### Authentication and authorization

A component's authorized/unauthorized rendering is presentation evidence, not proof that the server endpoint enforces its authorization policy. Test the policy at the ASP.NET Core integration boundary and use a small E2E path only when deployed identity wiring is the risk.

`AuthorizeView` and similar UI controls protect visibility, not the underlying operation by themselves. For cookie-authenticated state-changing requests or static SSR forms, include antiforgery evidence only when the actual application path and requirements use it.

### Async rendering and isolation

Wait for an observable render or state condition instead of sleeping for a fixed duration. Control clocks, cancellation, IDs, and shared services; reset fixtures per test; and ensure tests pass alone and in arbitrary order.

## Sources

- [Microsoft Learn: Test Razor components in ASP.NET Core Blazor](https://learn.microsoft.com/en-us/aspnet/core/blazor/test)
- [Microsoft Learn: Integration tests in ASP.NET Core](https://learn.microsoft.com/en-us/aspnet/core/test/integration-tests)
- [Microsoft Learn: Choosing an EF Core testing strategy](https://learn.microsoft.com/en-us/ef/core/testing/choosing-a-testing-strategy)
- [bUnit documentation](https://bunit.dev/)

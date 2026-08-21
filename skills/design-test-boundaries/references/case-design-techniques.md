# Case design techniques

Select techniques from the shape of the rule or risk. Combine them when necessary, but avoid multiplying cases without a distinct purpose.

| Technique | Use when | How to derive cases | Typical examples |
| --- | --- | --- | --- |
| Equivalence partitioning | values in the same category should behave alike | identify valid and invalid partitions; choose a representative from each | supported/unsupported role, present/missing field, valid/invalid format |
| Boundary value analysis | defects are likely around ordered limits | test the boundary and nearest values on both sides where representable | min/max length, date cutoff, page size, numeric range |
| Decision table | outcome depends on combinations of conditions | list conditions and actions, remove impossible combinations, cover distinct rules | role × ownership × resource state; authentication × request-forgery token × method when applicable |
| State transition | behavior depends on current or previous state | model states, events, valid transitions, invalid transitions, and repeated events | modal reopen, order lifecycle, retry, double-submit, lock/unlock |
| Scenario/use-case | a sequence delivers a business outcome | test main success path and high-value alternate/exception paths | login → create → list; approve → publish |
| Error guessing | prior defects or technology risks suggest failures | convert concrete failure history into regression cases | time zones, stale cache, null body, test order dependence |
| White-box branch analysis | an implementation branch lacks lower-level evidence | map decisions and exceptional exits to cases without coupling to irrelevant details | insert returns no row; mapping fallback; error handler |
| Pairwise/combinatorial | many mostly independent parameters create an impractical Cartesian product | cover interacting pairs, then add known high-risk combinations | browser × locale × role × feature flag |
| Property/invariant testing | many values share a rule stronger than examples | state an invariant and generate inputs within controlled domains | round-trip encoding, sort order, idempotency, totals preserved |

## Practical sequence

1. Write the observable contract and oracle first.
2. Use equivalence partitions to avoid redundant examples.
3. Add boundary values for ordered partitions.
4. Use a decision table when two or more conditions jointly determine behavior.
5. Use state transitions when history, retry, reopen, or duplicate actions matter.
6. Add experience-based regression cases from incidents and code-risk evidence.
7. Check implementation branches only after black-box risks are covered.

## Oracle checklist

Do not stop at "no exception" or an HTTP status. Select applicable outcomes:

- full response or emitted event;
- persisted state read back through a supported interface;
- unchanged state after rejection;
- side effect called once with the right data;
- side effect not called;
- ordering, filtering, ownership, and isolation;
- user-visible error and recoverability;
- audit/event evidence when required.

## Sources

- [ISTQB Certified Tester Foundation Level Syllabus v4.0.1](https://istqb.org/wp-content/uploads/2024/11/ISTQB_CTFL_Syllabus_v4.0.1.pdf) — equivalence partitioning, boundary value analysis, decision tables, and state-transition testing.
- [The Practical Test Pyramid](https://martinfowler.com/articles/practical-test-pyramid.html) — focused unit/integration/E2E allocation and the value of real external parts at integration boundaries.
- [Google Testing Blog: Test Sizes](https://testing.googleblog.com/2010/12/test-sizes.html) — classify test size by dependencies and require isolation/order independence.
- [Google Testing Blog: Increase Test Fidelity By Avoiding Mocks](https://testing.googleblog.com/2024/02/increase-test-fidelity-by-avoiding-mocks.html) — prefer fidelity when it does not unnecessarily enlarge the test.

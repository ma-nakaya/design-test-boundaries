# Test design brief

Use this template for a reusable test-plan artifact. Remove sections that add no decision value.

## Scope and evidence

- Change or behavior:
- Actor and outcome:
- Pinned version or artifact:
- In scope:
- Out of scope:
- Unknowns or requirement conflicts:

## Runtime and stack profile

- Entry point and hosting/execution model:
- State and invariant owner:
- Process, network, thread, trust, device, or persistence boundaries:
- Existing test libraries and CI commands:

| Concrete element | Architectural role | Contract owned | Boundary risk |
| --- | --- | --- | --- |
|  |  |  |  |

## Execution path

| Step | Responsibility | Input → output | State/side effect | Material failure |
| --- | --- | --- | --- | --- |
|  |  |  |  |  |

## Boundary allocation

| Boundary | Contract/risk | Test level | Real dependency | Double | Why this level |
| --- | --- | --- | --- | --- | --- |
|  |  |  |  |  |  |

## Prioritized case matrix

| Priority | Requirement/risk | Technique | Preconditions | Action/input | Expected observable result | Test location |
| --- | --- | --- | --- | --- | --- | --- |
| P0 |  |  |  |  |  |  |

Use P0 for release-blocking safety or core outcomes, P1 for important variants and recoverable failures, and P2 for lower-risk hardening. Do not invent a priority policy when the project already defines one.

## Data, fixtures, and isolation

- Fixture scope:
- Seed ownership:
- Cleanup/rollback:
- Clock, ID, random, and network control:
- Parallel and arbitrary-order behavior:

## Implementation order

1. Add the smallest test that reproduces the risk or specifies the contract.
2. Implement or fix the behavior.
3. Add focused integration evidence for changed boundaries.
4. Add or update a critical E2E journey only when deployed wiring changed or lacks evidence.
5. Run the smallest relevant suite, then the required CI-equivalent checks.

## Residual risks

- Not covered:
- Reason:
- Human/manual verification:
- Follow-up trigger:

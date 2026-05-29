---
title: Payments Service Decoupling
owner: Alex Morgan
priority: P1
goal: G2
status: green
checkpoint: 2026-06-15
checkpoint-type: delivery
created: 2026-05-04
last-updated: 2026-05-20
---

## Objective
Split the monolithic payments module into an independently deployable service so the team can ship payment changes without a full-platform release.

## Key Decisions
- Decouple at the deployment boundary first; internal refactor follows later
- Keep the existing public API contract stable during the split (no client changes)
- "Done" = payments service deploys independently in staging + prod with no regression

## Delegations
| Date | Delegate | Task | Checkpoint | Status |
|------|----------|------|------------|--------|
| 2026-05-04 | Jordan Lee | Extract payments module behind a stable interface | 2026-05-30 | active |
| 2026-05-06 | Sam Rivera | Stand up the independent deploy pipeline | 2026-06-10 | active |

## Logical Steps

### Phase 1: Boundary (May)
1. [x] Map current payments dependencies and call sites
2. [x] Define the stable interface the rest of the platform calls
3. [ ] Extract the module behind that interface (Jordan)

### Phase 2: Independent Deploy (May - Jun)
4. [ ] Stand up a separate deploy pipeline (Sam)
5. [ ] Deploy payments service to staging independently
6. [ ] Run regression suite against staging

### Phase 3: Production (Jun)
7. [ ] Cut over production traffic with a feature flag
8. [ ] Validate: independent deploy completed with no incident

## Context & Dependencies
- **Jordan Lee**: owns the code extraction; first checkpoint 2026-05-30
- **Sam Rivera**: owns the pipeline; depends on the interface being stable first
- **Risk**: hidden synchronous coupling to the orders module surfaces late → mitigate by mapping call sites in Phase 1
- **Risk**: regression suite has gaps around refunds → add refund test cases before cutover

## Status Log
### 2026-05-20
Phases 1.1 and 1.2 complete. Interface agreed and documented. Jordan extracting the module now, on track for the 2026-05-30 checkpoint. No blockers. Sam starting pipeline work in parallel.

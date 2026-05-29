# Execution Plan — Structured Reference

**Owner**: Alex Morgan — Staff Engineer — Platform Team
**Purpose**: Read-only reference for the situational-awareness skill to detect plan-vs-actual drift.

> This file is OPTIONAL. It enables Section 6 of the briefing (execution-plan
> drift detection) and the "behind plan" amber rule. Without it, stall detection
> still works on checkpoints and update cadence — you just lose plan-vs-actual.
>
> It is fine for this to be partial. Bootstrap seeds your top priority; you fill
> in the rest over time.

---

## Top Priorities

| Priority | Theme | Period-End Outcome |
|----------|-------|--------------------|
| P1 | Delivery velocity | Top 3 monolith modules decoupled into independent deploys |
| P2 | Reliability | Tier-1 availability at 99.9%; MTTD down 40% |
| P3 | People & quality | 2 mentees with growth plans; onboarding under 2 days |

---

## Month-by-Month Action Items

### May 2026
| Pri | Action Item | DoD | Goal |
|-----|-------------|-----|------|
| P1 | Decouple payments module (boundary + interface) | Interface agreed; extraction in progress | G2 |
| P2 | Stand up MTTD baseline dashboard | Dashboard live; baseline captured | G1 |
| P3 | Refresh onboarding guide | Draft circulated to recent hires | G3 |

### June 2026
| Pri | Action Item | DoD | Goal |
|-----|-------------|-----|------|
| P1 | Payments service deploys independently to prod | Cutover complete; no regression | G2 |
| P2 | Add alerting for top 5 critical paths | Alerts firing in staging | G1 |
| P3 | First monthly learning session held | Session run; topic documented | G4 |

### July 2026
| Pri | Action Item | DoD | Goal |
|-----|-------------|-----|------|
| P1 | Begin decoupling the orders module | Dependency map drafted | G2 |
| P2 | Quarterly reliability scorecard | Report published | G1 |

---

## KPI Targets

| # | Metric | Target |
|---|--------|--------|
| 1 | Tier-1 availability | 99.9% |
| 2 | Lead time to production | -30% vs. last year |
| 3 | MTTD | -40% vs. last year |
| 4 | Onboarding to first PR | < 2 days |

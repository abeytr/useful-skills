---
title: Checkout 500s on Mobile
driver: Jordan Lee
priority: critical
status: escalated
created: 2026-05-19
last-updated: 2026-05-21
---

## Impact
Mobile checkout returns 500 for ~8% of users; revenue-impacting and rising.

## Current State
Traced to a timeout in the payments call under load. Mitigation (raised timeout + retry) deployed to staging; needs a prod decision today. Escalated because the rollback path is risky during peak hours.

## Status Log
| Date | Entry |
|------|-------|
| 2026-05-19 | Reported by support; reproduced on mobile clients only |
| 2026-05-20 | Isolated to payments timeout under concurrent load |
| 2026-05-21 | Mitigation validated in staging; escalating for prod deploy window decision |

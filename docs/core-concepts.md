# Core Concepts

The situational-awareness ecosystem is built on a few opinionated ideas. Understanding them helps you get the most out of it — and decide what to customize.

## Initiatives vs. Firefighting

The system tracks two fundamentally different kinds of work, separately:

| | Initiatives | Firefighting |
|---|-------------|--------------|
| **Nature** | Durable work you own and drive | Urgent operational issues |
| **Time horizon** | Weeks to quarters | Hours to days |
| **Tracked with** | Status, checkpoints, delegations, logical steps | Driver, impact, current state |
| **Owner** | Always you (accountable) | You're accountable; a `driver` does the day-to-day |
| **Lifecycle** | Created → tracked → archived/done | Captured → resolved → archived |

A fire that keeps recurring or reveals a systemic problem can be **promoted to an initiative** — the system prompts you for this when a fire stays active 14+ days or when you resolve it.

## You are always the owner

Every initiative's `owner` is you (from `config.yaml`). This is deliberate: the system models *your* accountability. Work you've handed to others is tracked in the **Delegations** table, not by reassigning ownership. You stay accountable; delegates are named as the people doing the work.

## Status as a signal, not a label

Initiative status (`green`/`amber`/`red`/`blocked`/`on-hold`/`done`) is computed *and* stored. The skill computes what the status *should* be based on checkpoints, delegations, and plan drift — but never silently overwrites your stored value. If the computed status is worse than what you set, it flags it and asks. This preserves nuance you know that the rules don't.

## Stall detection

The system's real job is catching things before they rot:

- **Initiatives:** overdue delivery checkpoints, stalled delegations, silence past 3 weeks, drift behind the execution plan.
- **Fires:** active 7+ days with no update, waiting 14+ days, active 14+ days (promotion candidate).

`on-hold` and `done` are never flagged — pausing is a valid choice.

## Adaptive depth

The system grows with you. It activates features based on what files exist, not on a configured "role":

| Feature | Activates when |
|---------|----------------|
| Initiative + fire tracking | Always |
| Stall detection | Always |
| HTML dashboard | Always |
| Goal-mapping on new initiatives | `reference/goals.md` exists |
| Execution-plan drift detection | `reference/execution-plan.md` exists |
| Portfolio Governance section | 5+ initiatives OR any delegations |
| Leadership Prep Mode | `manager` set in `config.yaml` |

Start with pure operational tracking. Add the strategic layer (goals, plan) when you want it. Nothing breaks if you never do.

## Priorities are fixed: P1 / P2 / P3

This is an opinionated convention, not a configurable field:

- **P1** — must succeed this period.
- **P2** — important, but survivable if delayed.
- **P3** — valuable, deferrable.

Keeping three fixed tiers means the stall rules, sorting, and governance checks all speak the same language — and so do you and anyone you share your setup with.

## Persistence and sync

After every confirmed change, the system persists. *How* depends on `config.yaml`:

- **Git off (default):** files are saved to disk. Your folder syncs however you already sync it (OneDrive, Dropbox, iCloud, or nothing).
- **Git on:** the system commits (and optionally pushes/merges) on your behalf, giving you version history.

Either way, the guarantee is the same: when you finish, everything is saved. You can trust the next briefing to be current.

## Decisions are worth keeping

Reviews surface decisions — scope changes, rejected alternatives, accepted risks. The system can capture these in `reference/decisions/` so the *rationale* survives, not just the outcome. This is optional but recommended: six months later, "why did we decide X?" is answerable.

# Situational Awareness

> A periodic review system for knowledge workers juggling parallel initiatives.
> **Not a task tracker** — a context-switch recovery tool that tells you where
> everything stands, what's stalling, and what needs your attention next.

**Requires:** core ecosystem (creates its own `initiatives/`, `firefighting/`, `reference/` structure on bootstrap).

## What it does

When you're running many things in parallel and a fire pulls you away for days, you lose the thread. This skill rebuilds it instantly:

- **Briefing** — one scannable view of every initiative (Green / Amber / Red / Blocked / On-Hold) and every active fire.
- **Stall detection** — flags initiatives drifting past checkpoints, delegations gone quiet, and fires sitting too long.
- **Interactive updates** — walks you through what needs attention and logs updates as you go.
- **HTML dashboard** — a visual portfolio dashboard, regenerated every run.
- **Adaptive depth** — connect goals and an execution plan when you want strategic alignment; skip them and it's a pure operational tracker.

## Two core concepts

- **Initiatives** — durable work you own and drive (often through others). Tracked with status, checkpoints, delegations, and logical steps.
- **Firefighting** — urgent operational issues pulling at your attention right now. Tracked separately, with a driver and an impact line, and a path to "promote to initiative" if a fire turns out to be systemic.

## Install

This skill is a **directory**, not a single file. Copy the whole folder:

```bash
cp -r skills/situational-awareness ~/.claude/skills/
```

The folder includes `SKILL.md` and `report-template.html` (the dashboard). Both are needed.

## First run

In the project folder where you want your tracking data to live:

```
/situational-awareness bootstrap
```

(Or just run `/situational-awareness` — it auto-detects a first run when there's no `config.yaml`.)

Bootstrap has two tiers:

1. **Get tracking (~3 min, mandatory):** your name, git preference, and the 1-3 things you're working on right now. You walk away with a working system.
2. **Connect to goals (optional, anytime):** paste a goals/OKR document or answer a short interview to generate `reference/goals.md` and `reference/execution-plan.md`. This unlocks goal-mapping, drift detection, and Leadership Prep Mode.

## Everyday use

| Command | What it does |
|---------|--------------|
| `/situational-awareness` | Full briefing + interactive update session |
| `/situational-awareness status payments` | Quick status card for one item |
| `/situational-awareness update payments` | Log an update after a meeting |
| `/situational-awareness new fire` | Capture an urgent issue |
| `/situational-awareness new initiative` | Start tracking new work |
| `/situational-awareness bootstrap goals` | Add the goals layer later |

## Configuration

All behavior comes from `config.yaml` at your project root. See `config.example.yaml`. Highlights:

- **`owner`** — your name (every initiative's owner).
- **`manager`** — optional; set it to enable Leadership Prep Mode.
- **`git`** — off by default. Leave it off if your folder already syncs (OneDrive/Dropbox/iCloud). Turn it on for git version history.
- **`reports_dir`** — where the dashboard is written (default `.reports`; Conductor users may use `.context/reports`).

## Examples

See the repo's [`examples/`](../../examples/) directory for sample initiative, firefighting, goals, and execution-plan files — including one initiative with no `goal` field to show how the system works before you connect goals.

# useful-skills

A collection of [Claude Code](https://claude.com/claude-code) skills for
knowledge work — tracking parallel initiatives, recovering from context
switches, delegating, and learning.

Skills are self-contained. Some share a common project structure (the **core
ecosystem**); others are **standalone**. Pick the ones you want.

---

## Skills

### 🧭 situational-awareness — *core*

A periodic review system for anyone juggling parallel initiatives and day-to-day
fires. Not a task tracker — a **context-switch recovery tool** that tells you
where everything stands, what's stalling, and what needs your attention next.
Produces a scannable briefing, an HTML dashboard, stall detection, and an
interactive update session. Bootstraps itself on first run.

→ [Skill README](skills/situational-awareness/README.md) · [Getting started](docs/getting-started.md)

```bash
cp -r skills/situational-awareness ~/.claude/skills/
# then, in your project folder:
/situational-awareness bootstrap
```

*More skills (handoff, explore) coming soon — see below.*

---

## Two ways to adopt

**Adopt the ecosystem.** Install `situational-awareness` and use it as your
operational backbone. It creates and owns a shared project structure
(`initiatives/`, `firefighting/`, `reference/`) that future core skills build on.

**Cherry-pick.** Each skill works on its own. A standalone skill declares its
own needs and doesn't require the core structure.

## Repository layout

```
useful-skills/
├── README.md                ← you are here
├── skills/
│   └── situational-awareness/
│       ├── SKILL.md             the skill itself
│       ├── report-template.html the dashboard template (ships with the skill)
│       ├── config.example.yaml  configuration template
│       └── README.md            skill-specific docs
├── examples/                ← sample initiative / fire / goals / plan files
└── docs/
    ├── getting-started.md
    ├── core-concepts.md
    └── customization.md
```

A skill that is a **directory** (like situational-awareness) must be copied
whole — it bundles assets (the dashboard template) alongside its `SKILL.md`.

## Core concepts (the shared model)

The core ecosystem tracks two kinds of work, separately:

- **Initiatives** — durable work you own and drive, with status, checkpoints,
  delegations, and steps.
- **Firefighting** — urgent operational issues, with a driver and an impact
  line, and a path to "promote to initiative" when a fire turns out to be
  systemic.

You're always the accountable **owner**; work handed to others is tracked as
**delegations**. Priorities are a fixed **P1 / P2 / P3**. Depth is **adaptive**
— start as a pure operational tracker, add goals and an execution plan when you
want strategic alignment.

Read more in [docs/core-concepts.md](docs/core-concepts.md).

## Requirements

- [Claude Code](https://claude.com/claude-code)
- `python3` (used to write the dashboard data file)
- `git` + `gh` CLI — only if you opt into git sync (off by default)

## Status

| Skill | State | Type |
|-------|-------|------|
| situational-awareness | ✅ Available | core |
| handoff | 🚧 Coming soon | core |
| explore | 🚧 Coming soon | core |

## License

MIT — see [LICENSE](LICENSE).

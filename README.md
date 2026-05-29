# useful-skills

A collection of [Claude Code](https://claude.com/claude-code) skills for
knowledge work and engineering — tracking parallel initiatives, recovering from
context switches, thinking through problems, stress-testing plans, and reviewing
architecture.

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

### 🔥 grill-me — *standalone*

Stress-test a plan, design, or decision until every branch, dependency, and
tradeoff is explicit. One skeptical question at a time, each with a recommended
answer, ending in an ordered plan. (This skill ran the design of this repo.)

→ [Skill README](skills/grill-me/README.md)

```bash
cp -r skills/grill-me ~/.claude/skills/
```

### 💡 brainstorm — *standalone*

A Socratic thinking partner for understanding a problem and finding the
*simplest* resolution. Grounded in mental models from "The Great Mental Models."
Three phases: understand → research solutions (simplest-first) → document.

→ [Skill README](skills/brainstorm/README.md)

```bash
cp -r skills/brainstorm ~/.claude/skills/
```

### 🏛️ system-architecture-review — *standalone*

A rigorous, principled review of an existing system's architecture — Socratic
domain understanding, a review grounded in Richards & Ford, and an adversarial
cross-model challenge (via a second AI model or human peer) to catch blind spots.

→ [Skill README](skills/system-architecture-review/README.md)

```bash
cp -r skills/system-architecture-review ~/.claude/skills/
```

*More core skills (handoff, explore) coming soon — see status below.*

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
│   ├── situational-awareness/   core: portfolio review system
│   │   ├── SKILL.md
│   │   ├── report-template.html the dashboard template (ships with the skill)
│   │   ├── config.example.yaml  configuration template
│   │   └── README.md
│   ├── grill-me/                standalone: plan stress-testing
│   │   ├── SKILL.md
│   │   └── README.md
│   ├── brainstorm/              standalone: Socratic problem exploration
│   │   ├── SKILL.md
│   │   └── README.md
│   └── system-architecture-review/  standalone: principled architecture review
│       ├── SKILL.md
│       └── README.md
├── examples/                ← sample initiative / fire / goals / plan files
└── docs/
    ├── getting-started.md
    ├── core-concepts.md
    └── customization.md
```

Each skill is a **directory** — copy it whole. situational-awareness bundles its
dashboard template alongside `SKILL.md`; copying just the `.md` would leave the
dashboard broken.

## Core concepts (the situational-awareness ecosystem)

The **core** skills share a model. (The standalone skills — grill-me, brainstorm,
system-architecture-review — don't need any of this; they work on their own.)
The core tracks two kinds of work, separately:

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

- [Claude Code](https://claude.com/claude-code) — all skills.
- `python3` — situational-awareness only (writes the dashboard data file).
- `git` + `gh` CLI — situational-awareness only, and only if you opt into git
  sync (off by default).
- A second AI model (Codex/Gemini CLI) or a human peer — system-architecture-review
  only, for the adversarial pass (optional; degrades gracefully without one).

grill-me and brainstorm need nothing beyond Claude Code.

## Status

| Skill | State | Type |
|-------|-------|------|
| situational-awareness | ✅ Available | core |
| grill-me | ✅ Available | standalone |
| brainstorm | ✅ Available | standalone |
| system-architecture-review | ✅ Available | standalone |
| handoff | 🚧 Coming soon | core |
| explore | 🚧 Coming soon | core |

## License

MIT — see [LICENSE](LICENSE).

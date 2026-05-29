# Getting Started

This guide takes you from zero to your first situational-awareness briefing.

## 1. Install the skill

Skills live in `~/.claude/skills/`. The situational-awareness skill is a
**directory** (it ships a dashboard template alongside the SKILL.md), so copy
the whole folder:

```bash
cp -r skills/situational-awareness ~/.claude/skills/
```

Verify Claude Code sees it by typing `/situational-awareness` — it should
autocomplete.

## 2. Pick a project folder

Your tracking data lives in a folder of your choosing — your "portfolio." This
is separate from where the skill is installed. For example:

```bash
mkdir ~/my-portfolio && cd ~/my-portfolio
```

You can use any folder: a local directory, a synced OneDrive/Dropbox/iCloud
folder, or a git repo. The skill operates in whatever directory you run it from.

## 3. Bootstrap

From inside your project folder, run:

```
/situational-awareness bootstrap
```

You'll be asked:

1. **Your name** — becomes the owner of every initiative.
2. **Git sync?** — say *no* if your folder already syncs (OneDrive/Dropbox/iCloud);
   say *yes* if you want git version history. Off by default.
3. **Dashboard location** — defaults to `.reports`. Conductor users may prefer
   `.context/reports`.
4. **What you're working on** — 1-3 current initiatives.
5. **Any fires** — urgent issues, if any.

That's it. You now have a working system:

```
my-portfolio/
├── config.yaml
├── initiatives/
│   └── (your initiatives)
├── firefighting/
│   └── (your fires)
├── reference/
│   └── decisions/
└── archive/
    └── firefighting/
```

## 4. Run your first briefing

```
/situational-awareness
```

You'll get a scannable portfolio briefing and an HTML dashboard
(`.reports/situational-awareness.html`), then an interactive update session.

## 5. (Optional) Connect to goals

When you're ready for strategic alignment:

```
/situational-awareness bootstrap goals
```

Paste a goals/OKR document or answer a short interview. This generates
`reference/goals.md` and `reference/execution-plan.md`, which unlock:

- **Goal-mapping** when you create initiatives
- **Drift detection** (plan-vs-actual) in the briefing
- **Leadership Prep Mode** (if you also set a `manager` in config)

You can skip this forever and the system still works as a pure operational
tracker.

## Daily habits

- **Weekly:** run `/situational-awareness` for the full review (20-30 min).
- **After a meeting:** `/situational-awareness update <name>` to log what changed.
- **When a fire starts:** `/situational-awareness new fire`.
- **Before a 1:1:** `/situational-awareness preparing for <manager>` (if goals + manager are set).

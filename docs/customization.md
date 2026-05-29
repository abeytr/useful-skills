# Customization

The skill is opinionated by design, but several things are meant to be adapted.

## Configuration knobs (`config.yaml`)

| Setting | Default | When to change |
|---------|---------|----------------|
| `owner` | — | Always set during bootstrap. |
| `manager` | empty | Set to enable Leadership Prep Mode. |
| `git.enabled` | `false` | Turn on for git version history. |
| `git.auto_push` | `false` | Push automatically after commits. |
| `git.push_target` | `current` | `current`, `main`, or a branch name. |
| `git.merge_to_main` | `false` | Advanced: commit on a branch, then merge to main via the GitHub API. |
| `reports_dir` | `.reports` | Set to `.context/reports` on Conductor, or anywhere you like. |

## Git workflows

The default is **no git** — best for synced folders. If you enable it, the
common setups are:

- **Solo, direct:** `enabled: true`, `auto_push: true`, `push_target: current`
  (or `main`). Commits and pushes after every change.
- **Commit only:** `enabled: true`, `auto_push: false`. The skill commits; you
  push when you want.
- **Branch + merge (advanced):** `merge_to_main: true`. Work on a branch, the
  skill merges to main via the GitHub API after pushing. Requires `gh` CLI and
  a GitHub remote. The repo owner/name is derived from `git remote -v` at
  runtime — nothing is hardcoded.

## The dashboard

`report-template.html` is a self-contained static template (dark/light theme,
no dependencies). It renders from `dashboard-data.json`. You can restyle it
freely — the skill only writes the JSON; it never touches your HTML once
deployed. To pick up template changes, delete your project's
`{reports_dir}/situational-awareness.html` and re-run; the skill re-copies the
template.

## Goals and execution plan

These are plain Markdown files (`reference/goals.md`,
`reference/execution-plan.md`). Edit them by hand anytime, or re-run
`/situational-awareness bootstrap goals` / `bootstrap document` to regenerate.
The skill reads the goal numbering (`Goal 1`, `Goal 2`, …) for goal-mapping and
the month headings in the execution plan for drift detection — keep those
structures and the rest is yours.

## What's deliberately NOT configurable

- **The two work types** (initiatives, firefighting) — they're the model.
- **Priority tiers** (P1/P2/P3) — fixed so every rule and every shared setup
  speaks the same language.
- **The accountability model** — you are always the owner; others are delegates.

If you need to change these, you're forking the philosophy, not just the config
— which is fine, the skill is yours to edit.

## Adding your own skills

This repo is a **core + extensions** layout. To add a skill:

1. Create `skills/<your-skill>/SKILL.md`.
2. In its README, declare whether it `requires: core` (uses the shared
   `initiatives/`, `firefighting/`, `reference/` structure) or is `standalone`
   (owns its own folders).
3. If it needs bootstrapping, build that into the skill itself — let it create
   its own folder structure on first run, the way situational-awareness does.
   Share only `config.yaml` at the project root; append to it, never overwrite.

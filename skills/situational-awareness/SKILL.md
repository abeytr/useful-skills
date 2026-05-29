---
name: "situational-awareness"
description: |
  Periodic portfolio review system for knowledge workers juggling parallel
  initiatives and day-to-day firefighting. Produces a complete situational
  briefing (where everything stands, what's stalling, what needs attention),
  drives an interactive update session, and keeps your tracking data fresh.
  Built for context-switch recovery: when fires pull you away and you come
  back, this tells you exactly where every initiative stands.
  Use when the user says "situational awareness", "where do things stand",
  "weekly review", "portfolio review", "status check", or wants to capture
  a new initiative or fire.
argument-hint: "[bootstrap | status <query> | update <query> | new fire | new initiative]"
user-invocable: true
disable-model-invocation: false
---

# Situational Awareness

## Purpose

Provide a complete portfolio briefing and interactive update session. This is a periodic review system — giving you instant situational awareness of all active initiatives AND day-to-day firefighting items, flagging stalls, ensuring closure, and keeping data fresh through prompted updates. Initiatives and fires are tracked separately but reviewed in one session.

The core value: **context-switch recovery**. When you're juggling many things in parallel and a fire pulls you away for days, this system tells you exactly where everything stands the moment you return — no reconstruction from memory.

## When to Use

- Periodic review (recommended: a fixed weekly calendar block, e.g. Monday morning)
- Coming back from a firefighting period and need to regain context
- Before a 1:1 with your manager or a periodic/quarterly review
- Any time you want to know "where do things stand?"
- When you need to capture or check on active fires / high-priority operational issues
- Quick status check on a single item (right before or after a meeting)
- Quick update log right after a meeting (before you forget)
- Capture a new fire immediately when it happens
- **First-time setup** — run `/situational-awareness bootstrap` (or just `/situational-awareness` in an empty project)

---

## Configuration & First-Run Detection

This skill operates in the **current working directory** — the project folder where your tracking data lives. All paths below are relative to that directory.

### config.yaml

Identity and behavior come from `config.yaml` at the project root:

```yaml
# Identity (required)
owner: ""              # Your name — appears as initiative owner
manager: ""            # Who you report to (optional) — enables Leadership Prep Mode

# Git sync (optional — off by default)
git:
  enabled: false       # true = skill commits/pushes after changes
  auto_push: false     # true = push after commit (requires enabled: true)
  push_target: current # "current" | "main" | branch name
  merge_to_main: false # true = also merge to main after pushing (advanced)

# Output
reports_dir: .reports  # Where the HTML dashboard + JSON data are written
                       # Conductor users: set to ".context/reports"
```

The skill reads `owner` for the initiative `owner` field, `manager` to enable Leadership Prep Mode, `git` to decide whether/how to commit, and `reports_dir` for dashboard output.

### First-Run Detection

At the start of **every** invocation, check whether `config.yaml` exists in the working directory.

- **If `config.yaml` does NOT exist** → this is a first run. Trigger the **Bootstrap** flow (see below) before doing anything else.
- **If `config.yaml` exists but `initiatives/` is empty and no firefighting files exist** → offer (don't force): "Your system is set up but empty. Want to capture what you're working on right now? (yes/skip)" If skip, proceed to the normal briefing (which will simply report an empty portfolio).
- **If `config.yaml` exists and there is data** → proceed normally.

### Adaptive Features

The skill activates features based on what files actually exist — no role or tier configuration:

- **Goal mapping** (in `new initiative`): active only if `reference/goals.md` exists. If absent, the goal question is skipped silently and the `goal` frontmatter field is omitted.
- **Execution-plan drift detection** (Phase 1, Section 6): runs only if `reference/execution-plan.md` exists. If absent, the section is skipped.
- **Leadership Prep Mode**: available only if `manager` is set in `config.yaml`.
- **Portfolio Governance** (Phase 1, Section 10): renders only if there are 5+ active initiatives OR any delegation records exist across initiative files. Below that threshold, it's skipped — it adds no value for small portfolios.
- **Delegate load tracking**: computed only if delegations exist in any initiative file.

Stall detection, checkpoint tracking, and the core briefing always run regardless of portfolio size.

---

## Bootstrap (First-Time Setup)

The skill bootstraps its own folder structure and configuration. There is no separate setup tool. Bootstrap is **idempotent** — re-running never destroys existing data.

### Trigger

- Automatically, when `config.yaml` is missing (first run).
- Explicitly, via:
  - `/situational-awareness bootstrap` — full setup (or re-run to reconfigure)
  - `/situational-awareness bootstrap goals` — set up just the goals + execution-plan layer
  - `/situational-awareness bootstrap document` — ingest a document to generate/refresh goals and execution plan

### Folder Structure Created

Bootstrap creates these directories if they don't exist (the skill owns its own structure):

```
config.yaml
initiatives/
firefighting/
reference/
reference/decisions/
archive/
archive/firefighting/
```

`.reports/` (or the configured `reports_dir`) is created lazily on first dashboard generation. If git is enabled during bootstrap, add `.reports/` to `.gitignore`.

### Tier 1: Get Tracking (mandatory, ~3 minutes)

The goal: a functional system the user can review immediately. Ask these in sequence:

1. **"What's your name?"** → `owner` in config.yaml.
2. **"Do you want git-backed sync?"** → Most users on a synced folder (OneDrive, Dropbox, iCloud) should say no — the folder syncs itself. Say yes only if you want version history via git. If yes, ask: "Push automatically after changes? Push to which branch (current/main)?" Set the `git` block accordingly. Default is `git.enabled: false`.
3. **"Where should the dashboard be written?"** → Default `.reports`. (Mention: Conductor users may prefer `.context/reports`.) Set `reports_dir`.
4. **"What 1-3 things are you actively working on?"** → For each, run a lightweight initiative capture (title, one-line objective, priority P1/P2/P3, optional first step). Create initiative files in `initiatives/`.
5. **"Any fires right now — urgent operational issues pulling at your attention?"** → If yes, capture each (title, driver, critical/high, one-line impact). Create files in `firefighting/`.

After Tier 1, write `config.yaml`, create the initiative/fire files, and tell the user: "You're set up. Run `/situational-awareness` any time for your briefing. When you're ready to connect this to goals and a plan, run `/situational-awareness bootstrap goals`."

Then offer: "Want to connect this to your goals now, or later?" If now, proceed to Tier 2. If later, stop.

### Tier 2: Connect to Goals (optional, run anytime)

This adds the strategic layer that unlocks goal-mapping, drift detection, and Leadership Prep Mode.

**Ask first: "Do you have a document with your goals/priorities/OKRs? (paste it, give a file path, or say 'no')"**

**Document path** (`bootstrap document`, or "yes" above):
1. Read/ingest the document (OKRs, performance plan, strategy deck, OneNote export, brain dump — anything).
2. Extract goals (with success measures where present) → write `reference/goals.md`.
3. Extract priorities and any time-phased action items → write `reference/execution-plan.md`.
4. Show the user what you extracted and ask: "Does this look right? Anything to adjust?"

**Interview path** ("no" above): ask these, tightly —
1. "What are your 3-5 goals for this period? One line each, with what success looks like."
2. "What are your top 3 priorities right now, and which goals do they serve?"
3. "For your #1 priority: key milestones or deliverables over the next 3-6 months?"
4. (Skip if already captured in Tier 1) "Anything else you're actively working on that you'd track?"
5. "Who do you report to, and how often do you sync?" → sets `manager` in config, suggests checkpoint cadence.

Produce:
- `reference/goals.md` — numbered goals (G1, G2, …) with one-line success measures. This numbering is what `new initiative` displays for goal-mapping.
- `reference/execution-plan.md` — at minimum the P1 milestones seeded into a month-by-month structure. This is what drift detection compares against. It's fine for this to be partial; the user fills it out over time.

If `manager` was provided, update `config.yaml`.

### goals.md format (generated)

```markdown
# Goals — Structured Reference

**Owner**: {owner}

---

## Goal 1: {goal title}
**Success Measures**:
- {one-line measure}

## Goal 2: {goal title}
...
```

### execution-plan.md format (generated)

```markdown
# Execution Plan — Structured Reference

**Owner**: {owner}
**Purpose**: Read-only reference for the situational-awareness skill to detect plan-vs-actual drift.

---

## Top Priorities

| Priority | Theme | Outcome |
|----------|-------|---------|
| P1 | {theme} | {year/period-end outcome} |

---

## Month-by-Month Action Items

### {Month Year}
| Pri | Action Item | DoD | Goal |
|-----|-------------|-----|------|
| P1 | {action} | {definition of done} | G1 |
```

Both files are undated (no year in the filename). When the planning cycle refreshes, re-run `bootstrap goals` or `bootstrap document` to update them; git history (if enabled) preserves prior versions.

---

## Quick Actions

The skill supports targeted actions via argument patterns. These bypass the full review and execute in under 30 seconds.

### Argument Patterns

| Invocation | Mode | Behavior |
|---|---|---|
| `/situational-awareness` | Full review | Phase 1 briefing + Phase 2 updates (or bootstrap if first run) |
| `/situational-awareness bootstrap` | First-time / re-setup | Tier 1 + offer Tier 2 |
| `/situational-awareness bootstrap goals` | Goals setup | Tier 2 only |
| `/situational-awareness bootstrap document` | Document ingest | Tier 2 via document extraction |
| `/situational-awareness status [query]` | Single-item status | Show compact card for one item |
| `/situational-awareness update [query]` | Quick update | Log entry for one item after a meeting |
| `/situational-awareness new fire` | New fire capture | 5-question inline capture |
| `/situational-awareness new fire [title]` | New fire (pre-filled) | Pre-fills title, asks remaining 4 questions |
| `/situational-awareness new initiative` | New initiative capture | 6-question inline capture (Q4 skipped if no goals.md) |
| `/situational-awareness new initiative [title]` | New initiative (pre-filled) | Pre-fills title, asks remaining questions |

**Mode detection:** The first word(s) after `/situational-awareness` determine the mode. `bootstrap`, `status`, `update`, `new fire`, and `new initiative` are reserved command verbs — everything after them is the query.

### Matching Logic

When `[query]` is provided:
1. Case-insensitive substring match against both the `title` frontmatter field AND the filename
2. Searches both `initiatives/` and `firefighting/` directories
3. **One match** → proceed immediately with that item
4. **Multiple matches** → show numbered list, ask "Which one?" — user picks by number
5. **Zero matches** → "No match for '[query]'. Did you mean one of these?" + list all items

### Quick Action: `status [query]`

Read the matched file and return a compact card (5-8 lines max). No recommendations, no stall detection, no execution plan cross-referencing — just the facts for instant awareness.

**Initiative card format:**
```
[Title] — [STATUS] — [Priority]/[Goal if present]
Checkpoint: [date] ([type]) — in [N] days
Current step: [Phase X — description of next unchecked step]
Delegates: [names] ([status], checkpoint [date])
Last log: [date] — [most recent log entry, truncated to ~1 line]
```

**Firefighting card format:**
```
[Title] — [STATUS] — [priority]
Driver: [name] — [N] days active
Impact: [one line from file]
Last log: [date] — [most recent log entry, truncated to ~1 line]
```

No follow-up questions. Display and done.

### Quick Action: `update [query]`

1. Match the item (initiative or fire)
2. Show the compact status card (so user has context)
3. Ask: "What's the update?"
4. User provides freeform text (1-5 sentences)
5. Append as a dated log entry to the Status Log in the file
6. Update `last-updated` field to today's date
7. **One smart follow-up** — infer if a status/checkpoint change is implied:
   - Update mentions a blocker/problem/failure → "Sounds like this might be amber/blocked. Change status? (y/n)"
   - Update mentions completion/resolution → "Mark as resolved/done? (y/n)"
   - Update mentions a new date/deadline → "Update checkpoint to [inferred date]? (y/n)"
   - None of the above → no follow-up
8. Save file and persist (see Persistence)

Total interaction: show card → one freeform answer → maybe one y/n → saved. Under 30 seconds.

### Quick Action: `new fire` / `new fire [title]`

Same 5-question inline capture as the full review:
1. "What's the title?" (skip if pre-filled from argument)
2. "Who's driving it?" (name)
3. "Critical or high?" (priority)
4. "One-line impact?" (what breaks)
5. "Where does it stand right now?" (2-3 sentences)

Create file in `firefighting/` with today's date prefix. Persist.

### Quick Action: `new initiative` / `new initiative [title]`

Capture flow for creating a new initiative. Questions adapt to what reference files exist:

1. "What's the title?" (skip if pre-filled from argument)
2. "One-line objective?" (what success looks like)
3. "Priority?" → Show options: `P1 / P2 / P3`. User picks one.
4. **Goal alignment — only if `reference/goals.md` exists.** Read it, show a numbered list of goals (one line each). User picks by number (multiple allowed, comma-separated). **If `reference/goals.md` does NOT exist, skip this question entirely** and omit the `goal` field from the file.
5. "First checkpoint date and type?" → User provides a date + `update` or `delivery`.
6. **Logical steps:**
   - **If `reference/execution-plan.md` exists:** attempt to match the initiative's title/objective against action items. If matches found, propose 3-5 steps with month targets and ask: "Use these, edit them, or write your own?" If no match, ask: "List 3-5 logical steps with target months."
   - **If `reference/execution-plan.md` does NOT exist:** ask "What are the next 2-4 steps to move this forward?" — no month targets required, just a lightweight checklist.

After capture, create the file in `initiatives/` with a plain slug filename (lowercase, hyphens, common words stripped). Frontmatter:

```yaml
---
title: [from Q1]
owner: [from config.yaml]
priority: [from Q3]
goal: [from Q4 — OMIT THIS LINE ENTIRELY if no goals.md]
status: green
checkpoint: [from Q5 date]
checkpoint-type: [from Q5 type]
created: [today]
last-updated: [today]
---
```

Body structure:
```markdown
## Objective
[From Q2]

## Delegations
| Date | Delegate | Task | Checkpoint | Status |
|------|----------|------|------------|--------|
| TBD | TBD | TBD | TBD | — |

## Logical Steps
[From Q6 — numbered, unchecked; month targets in parentheses if execution plan exists]

## Context & Dependencies
- [Empty — user adds later]

## Status Log
### [today's date]
Initiative created.
```

Persist after user confirms.

---

## Decision Reference Handling

If a `reference/decisions/` directory exists (or the user has opted into decision tracking), capture decisions surfaced during interactive sessions in `reference/decisions/{initiative-slug}-decisions.md`. This is recommended practice but not mandatory — skip gracefully if the user doesn't use it.

### When to Create or Update a Decision File

Whenever an update, new initiative, promoted fire, killed initiative, scope change, checkpoint change, governance change, or sequencing change surfaces a decision, create or update the corresponding file.

**Decision-bearing signals** — detect these in user responses during updates:
- Architectural choice made or confirmed
- Scope added, removed, deferred, or killed
- Priority change with rationale
- Accepted risk
- Rejected alternative
- Sequencing rationale ("X before Y because...")
- Governance or ownership change
- Fire promoted to initiative (why it became systemic)
- Initiative killed or archived (outcome + lessons)

### New Initiative Creation

When creating a new initiative, optionally also create `reference/decisions/{initiative-slug}-decisions.md`:

```markdown
# [Initiative Title] — Decision Reference

**Date**: [today]
**Session type**: situational-awareness new initiative
**Initiative file**: `initiatives/{slug}.md`

---

## Initial Decisions

### Scope & Objective
[One line — what was decided about scope]

### Priority & Goal Alignment
- Priority: [P1/P2/P3] — because [rationale if provided]
- Goal: [if goals.md exists] — because [rationale if provided]

### Sequencing Logic
[From the logical steps — why this order, any dependencies noted]

---

*Further decisions will be appended as dated sections during reviews and updates.*
```

If the user provides minimal context, create the file with what's available. Do not block initiative creation waiting for a perfect decision record.

### Quick Update Flow

During `update [query]`, if the user's update text contains decision-bearing signals:
1. Append the update to the initiative's Status Log (existing behavior)
2. Ask one follow-up: "That sounds like a decision worth preserving. What was the rationale or rejected alternative?"
3. If the user provides rationale: append a dated section to the decision file
4. If the user says "skip": proceed without the decision file update

One follow-up max. If the decision file doesn't exist yet, create it with the template above before appending.

### Full Review (Phase 2) Updates

During Phase 2 interactive updates, if a user's response contains decisions, extract the decision naturally and append a dated section to the decision file after the user confirms.

At the end of Phase 2, before persisting, ask the forcing question:
> "Any decision made today that should be preserved beyond the status log?"

If yes: ask which initiative, extract the decision, append to the relevant decision file. If no: proceed.

### Decision File Section Format (for appending)

```markdown
## YYYY-MM-DD: [Short Decision Title]

### Decision
[What was decided]

### Rationale
[Why — even one sentence is valuable]

### Alternatives Considered
- [Alternative] — rejected because [reason]
(Omit if user didn't mention alternatives)

### Constraints Surfaced
- [Constraint]
(Omit if none)

### Accepted Risks
- [Risk]
(Omit if none stated)
```

### Kill/Archive Flow

When an initiative is killed or archived, append a final section to its decision file:

```markdown
## YYYY-MM-DD: Initiative Closed

### Outcome
[Completed / Killed / Merged into X]

### Final Rationale
[Why closed now]

### Lessons
[Anything worth preserving for future similar work]
```

---

## Repo Locations (relative to working directory)

| Path | Purpose |
|------|---------|
| `config.yaml` | Identity, git, and output settings |
| `initiatives/` | Active initiative tracking files |
| `firefighting/` | Active operational fires |
| `reference/goals.md` | Goals (optional — enables goal-mapping) |
| `reference/execution-plan.md` | Month-by-month plan (optional — enables drift detection) |
| `reference/decisions/` | Decision records from review sessions (optional) |
| `archive/` | Completed or killed initiatives |
| `archive/firefighting/` | Resolved fires |
| `{reports_dir}/` | Generated dashboard + JSON data (default `.reports/`) |

## Firefighting Item File Schema

```yaml
---
title: string
driver: string (person actively working it)
priority: critical | high
status: active | waiting | escalated | resolved
created: YYYY-MM-DD
last-updated: YYYY-MM-DD
resolved-date: YYYY-MM-DD (optional, set when resolved)
---
```

**File naming:** `YYYY-MM-DD-slug.md` (e.g., `2026-05-21-deploy-failure.md`)

**Status values:**
- `active` — someone is actively working this right now
- `waiting` — blocked on external input (vendor, other team, deployment window)
- `escalated` — needs your direct intervention or a leadership decision
- `resolved` — closed, root cause addressed or workaround in place

**Body structure:**

```markdown
## Impact
One line — what breaks or degrades if this isn't fixed.

## Current State
2-3 sentences max — where things stand right now.

## Status Log
| Date | Entry |
|------|-------|
| YYYY-MM-DD | Status update here |
```

**Ownership model:** You (the `owner` in config.yaml) are always the accountable owner — implicit, not in the schema. The `driver` field names the person doing the day-to-day work (which may be you).

---

## Initiative File Schema

```yaml
---
title: string
owner: [from config.yaml — always you; you are accountable]
priority: P1 | P2 | P3 | P1/P2 (multi-value allowed)
goal: G1 | G2 | ... (OPTIONAL — present only if goals.md exists; multi-value allowed)
status: green | amber | red | on-hold | done | blocked
checkpoint: YYYY-MM-DD
checkpoint-type: update | delivery
created: YYYY-MM-DD
last-updated: YYYY-MM-DD
---
```

**Required fields:** `title`, `owner`, `priority`, `status`, `checkpoint`, `checkpoint-type`, `created`, `last-updated`.
**Optional field:** `goal` (omit entirely when no goals framework is set up).

**Status values:**
- `green` — on track, progressing
- `amber` — drifting, needs attention but not urgent
- `red` — overdue or stuck, requires immediate action
- `on-hold` — intentionally paused (will NOT trigger stall alerts)
- `blocked` — waiting on external dependency
- `done` — completed, ready to archive

## HTML Dashboard

### File Locations
Static template (ships with the skill): `~/.claude/skills/situational-awareness/report-template.html`
Dashboard HTML (deployed once): `{reports_dir}/situational-awareness.html`
Data file (written every run): `{reports_dir}/dashboard-data.json`

`{reports_dir}` defaults to `.reports` and is read from `config.yaml`.

> **Install note:** This skill is a directory, not a single file. Copy the entire `situational-awareness/` folder (SKILL.md + report-template.html) into `~/.claude/skills/`. The skill reads the template from its own install location.

### Generation

The HTML dashboard uses a **static HTML + dynamic JSON** pattern to minimize token usage. The HTML file loads its data from a sibling `dashboard-data.json` via fetch. After reading all initiative and firefighting files and computing the briefing data:

**IMPORTANT — Generate the dashboard BEFORE presenting the text briefing.** The dashboard is not optional or secondary. It must be written as the first action of Phase 1, before any text output. Sequence: read files → compute data → write dashboard → then present text briefing.

1. Ensure `{reports_dir}` exists: `mkdir -p {reports_dir}`
2. Assemble a JSON object with pre-computed display values (see Data Shape below)
3. Write the JSON data and JS wrapper using Bash. **Do NOT use the Write tool for these files** — it requires a prior Read, wasting tokens on generated data that gets fully overwritten. Use Bash redirection:
   ```bash
   cat > {reports_dir}/dashboard-data.json << 'DATAJSON'
   { ... assembled JSON ... }
   DATAJSON

   python3 -c "import json; d=json.load(open('{reports_dir}/dashboard-data.json')); print('window.DASHBOARD_DATA = ' + json.dumps(d, separators=(',',':')) + ';')" > {reports_dir}/dashboard-data.js
   ```
4. If `{reports_dir}/situational-awareness.html` does not exist, copy it from the install location: `cp ~/.claude/skills/situational-awareness/report-template.html {reports_dir}/situational-awareness.html` and open it: `open {reports_dir}/situational-awareness.html`
5. If the HTML already exists, note: "Dashboard updated → {reports_dir}/situational-awareness.html"

**Token optimization:** Do NOT read the template file into context. Do NOT write full HTML. Only write the JSON data + JS wrapper (~100 lines total) each run.

### Data Shape

```json
{
  "generated": "2026-05-21T08:30:00",
  "summary": {
    "initiatives": { "total": 10, "green": 6, "amber": 2, "red": 1, "onHold": 1, "blocked": 0 },
    "fires": { "active": 2, "escalated": 1, "waiting": 1 }
  },
  "fires": [
    {
      "title": "Deploy Failure",
      "driver": "Jordan Lee",
      "priority": "critical",
      "status": "escalated",
      "daysActive": 5,
      "stalling": false,
      "stallReason": null,
      "impact": "Production deployments blocked",
      "currentState": "Investigating root cause in CI pipeline",
      "statusLog": [
        { "date": "2026-05-20", "entry": "Identified failing integration test" }
      ]
    }
  ],
  "initiatives": {
    "red": [
      {
        "title": "Initiative Name",
        "priority": "P1",
        "goal": "G4",
        "checkpoint": "2026-06-15",
        "trigger": "Delivery checkpoint overdue by 5 days",
        "currentStep": "Phase 2, Step 6: Finalize operating model",
        "delegations": "Jordan Lee, Sam Rivera",
        "lastLog": "2026-05-10 — First draft reviewed",
        "recommendedAction": "Follow up with Jordan on finalized model"
      }
    ],
    "amber": [],
    "blocked": [],
    "green": [],
    "onHold": []
  },
  "checkpoints": [
    { "title": "Service Decoupling", "date": "2026-06-15", "type": "delivery" }
  ]
}
```

The `goal` field may be absent on initiatives with no goals framework — the template renders "—" in that case. All values are pre-computed by the skill. The template only renders — no logic in the HTML.

---

## Workflow

### Phase 1: Briefing (5 minutes, read-only)

Read all files in `initiatives/` and `firefighting/`, plus `reference/execution-plan.md` if it exists.

**Step order (mandatory):**
1. Read all files and compute briefing data
2. **Generate the HTML dashboard FIRST** (write JSON + JS wrapper via Bash)
3. **Then** present the structured text briefing

Do NOT present the text briefing without having already written the dashboard files.

**1. Portfolio Health Summary**
```
Portfolio: X active initiatives | Y Green | Z Amber | W Red | H On-Hold | B Blocked
Fires: A active | E escalated | W waiting
```

**2. Firefighting Tracker**

Read all files in `firefighting/`. Show in this order: escalated first, then active, then waiting.

For each `escalated` item:
- Title — **ESCALATED** — Driver: [name]
- Impact: [one line from file]
- Current state: [latest log entry or current state field]
- Days active: [computed from created date]

For each `active` item:
- Title — Driver: [name] — [days active] days
- Current state: [latest log entry or current state field]
- Stall flag: show ⚠️ if active 7+ days with no update

For each `waiting` item:
- Title — Waiting on: [context from current state] — [days waiting]
- Stall flag: show ⚠️ if waiting 14+ days

For `resolved` items in cool-down (resolved since last review):
- Title — Resolved [date] — pending archive confirmation

**3. Red Items (Immediate Attention)**
For each:
- Initiative name
- Trigger: [specific reason it's red]
- Delegations: [who is doing what — from the Delegations table]
- Last status: [quote last log entry with date]
- Recommended action: [specific follow-up suggestion]

**4. Amber Items (Drifting)**
For each:
- Initiative name
- Trigger: [specific reason — behind plan / checkpoint approaching / delegation stalled]
- Last status: [quote last log entry with date]
- What the plan says should be happening now (only if execution plan exists)

**5. Blocked Items**
For each:
- Initiative name
- What's blocking it
- How long it's been blocked

**6. This Period's Execution Plan Items** *(only if `reference/execution-plan.md` exists)*
Cross-reference the execution plan for the current month. For each action item:
- Show which initiative it maps to (match by theme/content, not exact text)
- Show whether a corresponding step is checked or has a recent log entry
- Flag items with no apparent progress

If no execution plan exists, skip this section entirely.

**7. Green Items (On Track)**
One line each:
- Initiative name — current step — next checkpoint [date]

**8. Upcoming Checkpoints (Next 2 Weeks)**
List any checkpoints falling within the next 14 days.

**9. On-Hold Items (Information Only)**
One line each — what's on hold and why (from last log entry).

**10. Portfolio Governance** *(only if 5+ active initiatives OR any delegations exist)*

Surfaces cross-cutting concerns that individual initiative views miss:

- **P1 count**: Show total P1 initiatives. If >4, flag: "P1 overload — consider deferring or demoting."
- **Delegate load**: Show delegates with 3+ active commitments across initiatives. Flag overloaded people.
- **Blocked-by-team clustering**: If 2+ initiatives are blocked/waiting on the same team or person, flag the bottleneck.
- **Decision queue** *(if `reference/decisions/` is in use)*: List initiatives with no decision record — these lack durable rationale.
- **Checkpoint gaps**: Flag P1 initiatives with no checkpoint in the next 30 days.
- **Changes since last review**: Summarize what moved since the `last-updated` dates suggest activity.

If the portfolio is below the threshold, skip this section.

### Phase 2: Update Prompts (15-25 minutes, interactive)

After presenting the briefing, enter interactive mode. **Firefighting items are prompted first, before initiatives.**

#### Firefighting Updates

**For each `escalated` item**, ask:
> "[Title] — you escalated this. Any movement? Options: update, de-escalate to active, resolve, skip."

**For each `active` item**, ask:
> "[Title] — [Driver] is on this. Heard anything? Options: update, escalate, mark waiting, resolve, skip."

If active 14+ days, add the option:
> "This has been active for [N] days. This might be bigger than a fire. Options: update, escalate, resolve, **promote to initiative**, skip."

**For each `waiting` item**, ask:
> "[Title] — still waiting on [context]. Any movement? Options: update, back to active, escalate, resolve, skip."

**For each `resolved` item in cool-down**, ask:
> "[Title] was resolved on [date]. Still good? Does it point to a systemic issue worth tracking long-term? Options: confirm & archive, promote to initiative, reopen as active, skip."

If skipped during cool-down, show one more time on the next review, then auto-archive with a note: "Auto-archived after two review cycles with no objection."

**New fires capture** — after all existing firefighting updates, ask:
> "Any new fires to track?"

If yes, do the 5-question inline capture (title, driver, priority, impact, current state). Create the file in `firefighting/` and move on. If "no new fires," proceed to initiative updates.

---

#### Initiative Updates

**For each Red item**, ask:
> "[Initiative]: [Trigger reason]. Any update? Options: update status, adjust checkpoint, change to on-hold, or skip."

**For each Amber item**, ask:
> "[Initiative]: [Trigger reason]. Any progress or context to log?"

**For each Blocked item**, ask:
> "[Initiative]: Still blocked on [X]? Any movement?"

**For delegations that appear stalled** (checkpoint passed + no log entry), ask:
> "[Initiative] → [Delegate name]: checkpoint was [date]. Have you heard back? Options: log update, extend checkpoint, mark stalled."

**For Green items with checkpoints in the past week**, ask:
> "[Initiative]: Checkpoint was [date]. Anything to log?"

After all updates are collected, the user can also:
- Change status (to on-hold, blocked, done, etc.)
- Add a new delegation (triggers mini-handoff: who, what, checkpoint)
- Kill an initiative (move to `archive/` + final log entry with outcome summary)
- Adjust checkpoint dates
- Add evidence links (docs, PRs, decisions) to Context & Dependencies

If the user says "no updates" or "skip", move on immediately.

**New initiatives capture** — after all existing initiative updates, ask:
> "Any new initiatives to track?"

If yes, run the same capture flow as the `new initiative` quick action. If "no new initiatives," proceed to the decision capture gate.

#### Decision Capture Gate (End of Phase 2)

Before persisting, ask:
> "Any decision made today that should be preserved beyond the status log?"

- If yes: ask which initiative, extract the decision, and append to `reference/decisions/{initiative-slug}-decisions.md`.
- If no: proceed to Phase 3.

This is the safety net for decisions mentioned in passing. One question — not a long interview. Skip entirely if the user doesn't use decision tracking.

### Phase 3: Persist (Mandatory)

**Persistence is mandatory. How it happens depends on `config.yaml` git settings.**

#### If `git.enabled: false` (default)
Save all modified/created files to disk. That's it — the folder syncs via whatever mechanism the user relies on (OneDrive, Dropbox, iCloud, or just local). Confirm: "Saved. Next review recommended: [date +7 days]."

#### If `git.enabled: true`
1. Save all modified/created files
2. Stage and commit:
   ```
   git add initiatives/ firefighting/ archive/ reference/ {reports_dir}/
   git commit -m "review: [YYYY-MM-DD] — [brief summary]"
   ```
3. If `git.auto_push: true`, push to `git.push_target`:
   - `current` → `git push origin <current-branch>`
   - `main` → `git push origin main`
   - named branch → `git push origin <branch>`
4. If `git.merge_to_main: true` (advanced), after pushing the working branch, merge to main:
   ```
   gh api repos/<owner>/<repo>/merges -f base=main -f head=<current-branch> -f commit_message="[summary]"
   git fetch origin && git rebase origin/main && git push origin <current-branch>
   ```
   Derive `<owner>/<repo>` from `git remote -v` at runtime — never hardcode.
5. Confirm: "Committed[ and pushed]. Next review recommended: [date +7 days]."

**When to persist:**
- After each firefighting item is captured
- After each initiative update batch is complete
- After any status change or file modification
- Do NOT wait until end of session — persist incrementally after each confirmed change

If a git operation fails, retry once. If still failing, the files are already saved locally — alert the user and continue.

## Firefighting Stall Detection Rules

### Stalling: Active 7+ Days Without Update
- Status is `active`
- `last-updated` is 7+ days ago
- Show ⚠️ flag in briefing
- Prompt: "[Title] has been active for [N] days with no update. Still being worked? Options: update, escalate, resolve, skip."

### Stuck Waiting: 14+ Days
- Status is `waiting`
- `last-updated` is 14+ days ago (or created 14+ days ago if never updated)
- Show ⚠️ flag in briefing
- Prompt: "[Title] has been waiting for [N] days. Escalate, update, or keep waiting?"

### Promotion Trigger: Active 14+ Days
- Status is `active`
- `created` date is 14+ days ago
- Add "promote to initiative" as an option in the Phase 2 prompt
- Do not nag more than once per 7 days if user skips

### Systemic Issue Detection: At Resolution
- When status changes to `resolved`, prompt:
  > "Resolved. Does this point to a systemic issue worth tracking long-term? Options: yes (capture initiative), no, not sure yet."
- "Not sure yet" keeps it in cool-down for the systemic check during next review
- During cool-down review, ask again:
  > "With hindsight — any systemic pattern here that needs an initiative? Options: promote to initiative, archive, skip."

### Promote Fire to Initiative Flow

When the user selects "promote to initiative," reuse the standard initiative capture flow with pre-filled values:

1. **Title** → pre-filled from fire's `title` field (user can edit)
2. **Objective** → pre-filled from fire's `## Impact` section (user can edit)
3. **Priority** → asked normally
4. **Goal** → asked normally if goals.md exists, otherwise skipped
5. **Checkpoint** → asked normally
6. **Logical steps** → hybrid match if execution plan exists; also considers the fire's `## Status Log` for context

After the initiative file is created:
- Mark the fire file's status as `resolved`
- Set `resolved-date` to today
- Add a status log entry: "Promoted to initiative: [initiative-slug].md"
- The fire enters normal cool-down

Persist both files.

---

## Initiative Stall Detection Rules

### Red: Overdue Delivery Checkpoint
- `checkpoint` date has passed
- `checkpoint-type` is `delivery`
- No status log entry after the checkpoint date confirming completion or extension
- Initiative status is NOT `on-hold` or `done`

### Red: Delegation Overdue
- A delegation in the Delegations table has a checkpoint date that passed
- No status log entry mentioning that delegate after the checkpoint
- Delegation status is still `active`

### Amber: Behind Execution Plan *(only if execution plan exists)*
- Current month is past the 15th
- The execution plan has action items mapped to this initiative for the current month
- No corresponding step is checked and no recent log entry mentions progress
- Initiative status is NOT `on-hold`
- Initiative's first relevant milestone is in the current or past month (NOT future months)

### Amber: Update Checkpoint Approaching
- `checkpoint-type` is `update`
- Checkpoint date is within 7 days or has passed
- No recent status log entry

### Amber: Silent for 3+ Weeks (with exceptions)
- `last-updated` is more than 21 days ago
- Initiative status is NOT `on-hold` or `blocked`
- Initiative has at least one logical step due in the current or past month
- Does NOT apply to initiatives whose first milestone is entirely in a future month

### Green: On Track
- None of the above triggers fire
- OR a recent status log entry confirms progress
- OR status is explicitly set to `green` by the user during review

### Not Flagged (excluded from alerts)
- `status: on-hold` — user intentionally paused
- `status: done` — completed, should be archived
- `status: blocked` — shown in Blocked section, not Red/Amber

## Status Computation

When reading initiative files, compute whether the current `status` should trigger an alert. Do NOT auto-overwrite the status field without user confirmation:

1. If computed status is worse than stored status → flag it in the briefing with reason
2. Ask user during Phase 2: "This looks [red/amber] because [reason]. Update status, or is it actually fine?"
3. Only update the field after user confirms

This prevents false certainty and preserves nuance the user may have set manually.

## Quick Mode

If the user says "quick review" or "just the briefing", deliver Phase 1 only (including Section 2: Firefighting Tracker — fires are always shown). Then ask about `escalated` fires only. Active and waiting fires are shown but not prompted. After firefighting prompts (if any), ask: "Want to go into full update mode for initiatives, or is this enough for now?"

## Leadership Prep Mode *(only if `manager` is set in config.yaml)*

If the user says "preparing for [manager name]", "quarterly prep", or "leadership prep":
- Add a section: **Narrative** — summarize progress across P1/P2/P3 in 3-4 sentences each, suitable for a leadership conversation
- Highlight measurable evidence and deliverables completed (from status logs and checked steps)
- List evidence links from Context & Dependencies sections
- Flag risks or items that need your manager's help
- Surface any KPI/success metrics from `reference/goals.md` that have been addressed (if goals.md exists)

If no `manager` is configured, this mode is not available.

## Operating Rules

- Total time budget: 20-30 minutes for the full session (briefing + updates)
- Phase 1 should be scannable in under 5 minutes
- In Phase 2, if the user says "no updates" or "skip", move on immediately
- Never force the user to update something they don't have info on
- Always persist after confirmed changes; respect git settings
- Use today's date for all log entries
- When an initiative moves from Red/Amber to Green, note why in the log
- If ALL initiatives are Green, say so briefly and ask if there's anything to discuss
- Respect `on-hold` status — never flag on-hold items as stalled

## Anti-Patterns

- Do NOT produce a wall of text — use structure, headers, bullets
- Do NOT ask for updates on items the user has already said "skip" to
- Do NOT make the user repeat information already in the files
- Do NOT skip persistence (but handle failure gracefully)
- Do NOT auto-overwrite status without user confirmation
- Do NOT flag initiatives as stalled when their first milestone is in a future month
- Do NOT flag on-hold or blocked items as amber/red
- Do NOT force updates during quick mode
- Do NOT show empty adaptive sections (governance, execution-plan drift, leadership prep) when their prerequisites aren't met
- Do NOT hardcode names, repo URLs, or paths — read identity from config.yaml and derive git remotes at runtime

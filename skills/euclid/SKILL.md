---
name: "euclid"
description: "Apply the six critical-thinking habits Euclid's Elements trains — define terms, state postulates, decompose into dependency chains, construct-and-cite, reductio ad absurdum, proof by cases — to harden an argument, plan, or artifact before it leaves your hands. Default mode is a GATE: an adversarial armor-check that runs a hostile persona against a formed position (reductio on the lean, close-the-cases on the options, postulate audit), then routes high-stakes work to an external model. Also runs INLINE: five linguistic triggers that fire the right habit at the right moment during reasoning. Use when the user says euclid, wants an argument stress-tested before sharing, wants their own lean attacked, or wants to know which thinking habit a moment calls for. NOT for open problem exploration (use /brainstorm) or relentless plan interrogation (use /grill-me)."
argument-hint: "Point me at the artifact, lean, or argument to harden — or paste it inline"
user-invocable: true
disable-model-invocation: false
allowed-tools:
  - Read
  - Grep
  - Glob
  - Write
  - Edit
  - Bash
  - WebFetch
  - AskUserQuestion
  - Agent
---

# Euclid

> The most effective thinking curriculum in Western history is not an MBA, a law
> school, or a leadership course — it is a 2,300-year-old geometry book. Lincoln,
> Hobbes, Einstein, and Russell each pulled a different transferable habit from
> Euclid's *Elements*, and none of them became geometers. This skill extracts the
> six habits and turns them into an executable discipline for hardening arguments.

## Purpose

Euclid trained great thinkers to **refuse any step in an argument that isn't justified by an explicit rule** — to define terms, state assumptions in plain sight, build conclusions one cited step at a time, and, when a claim can't be proved directly, assume its opposite and follow it until it self-destructs.

This skill makes those habits operational. It is **not** another interrogation skill. Its specific job is the one habit most thinkers skip: **running the destructive test on your *own* favored answer before someone else does it for you, live, in the room.**

It does two things:

1. **GATE (default)** — Take a *formed* position (an artifact, a brief, a recommendation, a lean) and adversarially armor-check it: run a hostile persona that wants it killed, execute the reductio on the lean, close the cases on every option set, audit the postulates, then route high-stakes findings to an external model.
2. **INLINE** — While you reason, fire the right habit at the right moment via five linguistic triggers, kept deliberately light so it sharpens rather than nags.

## When to Use

- The user says "euclid" or "/euclid"
- The user has a *formed* position, brief, plan, or recommendation and wants it hardened before sharing
- The user wants their **own lean attacked** ("I think the answer is X — try to kill it")
- The user wants to know **which thinking habit** a moment calls for (the router)
- A pre-read is about to go to a hostile or skeptical audience (a design review, leadership, a customer)

## When NOT to Use

- Open-ended problem exploration with no position yet → use `/brainstorm` (you can't reductio a lean that doesn't exist)
- Relentless branch-by-branch plan interrogation → use `/grill-me`
- A heavy problem that warrants the Euclidean habits firing *inline as you think* → use `/grill-me-euclid` (this skill's inline layer woven into a full grill)
- Debugging a failure → use an investigation/debugging workflow
- Building/constructing a solution from scratch → that is *quod erat faciendum* (QEF) territory; use `/brainstorm`. This skill is for *quod erat demonstrandum* (QED) — proving a formed position holds. See "The QEF↔QED switch" below.

---

## The model: six habits, three jobs, two firing points

The six habits Euclid trains are not a flat checklist. Grouped by **the job they do on an argument**, they collapse to three pairs — and that grouping *is* the routing logic.

```
            EXPOSE              BUILD                TEST
       (the foundation)    (the bridge)        (under fire)
       cheap early,        as you reason,      needs a formed
       expensive late      mid-flow            target — comes last
     ┌────────────────┐ ┌────────────────┐ ┌──────────────────┐
     │ Define terms   │ │ Decompose into │ │ Reductio ad      │
     │                │ │  dependency    │ │  absurdum        │
     │ State the      │ │  chains        │ │                  │
     │  postulate     │ │ Construct-     │ │ Proof by cases   │
     │  (in plain     │ │  and-cite      │ │  (close every    │
     │  sight)        │ │  (name the     │ │  alternative)    │
     │                │ │  rule)         │ │                  │
     └────────────────┘ └────────────────┘ └──────────────────┘
     ──── INLINE (light, as you reason) ────►  ──── GATE (deep) ───►
```

**Why this partition is not arbitrary — it follows Euclid's own ordering:**

- **EXPOSE fires first / inline.** The *Elements* opens with 23 definitions and the postulates *before a single theorem*. Foundations are **cheap to fix early, expensive to fix late.** Catching a conflated term at the start is free; catching it after you've reasoned on top of it is rework.
- **BUILD fires mid-flow / inline.** Decomposition and cite-the-rule happen *as* you construct the argument. Cite-the-rule fires the moment you write "therefore."
- **TEST fires last / at the gate.** A reductio needs a *target*. You cannot destroy a lean that doesn't exist yet, so the destructive tests come last — on a complete, formed position. **This is the habit thinkers skip, because you do not run a reductio on the answer you've already fallen for.**

### The routing rule (the "which habit when")

> Diagnose which **job** the moment needs — Expose, Build, or Test — and the two habits inside it are obvious. Three things to hold in your head, not six.

- Default to **EXPOSE first** — most reasoning fails at the foundation, not the inference (Russell: *"most arguments fail not because the reasoning is bad, but because the assumptions are hidden"*). When you are trying to persuade a skeptic, this is decisive: **an audience that cannot find your assumptions cannot be persuaded by them, only made suspicious.**
- Move to **BUILD** when the ground is clear but the path is contested or too large to cross in one step.
- Reach for **TEST** when direct construction is blocked, or when your own conviction needs breaking before you commit to it.

### The off-switch (so the habits sharpen, not decorate)

Inversion gives each habit a "do NOT fire" condition. Over-application is the failure mode that gets a thinking tool switched off.

- **Don't Define** terms already shared and concrete (bikeshedding).
- **Don't Decompose** a one-step inference (overkill).
- **Don't Reductio** when direct construction is easy — reductio is for when you're *stuck* or *testing a lean*, not for every claim.
- Fire on **ambiguity-that-matters and stakes-that-matter**, never on every noun or every step.

---

## Euclid's three-part foundation (a detail most popular accounts flatten)

The *Elements* Book I opens with **three** kinds of starting point, not two. Keeping them separate is what makes the postulate audit sharp:

| Foundation type | What it is | Attackable? | Example |
|---|---|---|---|
| **Definition** | A precise meaning fixed before use | n/a — you *set* it | "By *done* I mean merged to main and deployed, not code-complete" |
| **Postulate** | A **domain-specific grant** taken as given but *contestable* | **YES — hunt these** | "Users won't tolerate a second login step" |
| **Common notion** | A **general logical principle**, granted | rarely | "If A depends on B and B fails, A fails" |

**The operational point: only postulates are worth the reductio.** Definitions you control; common notions are granted. So the gate's postulate audit forces you to **tag which kind each starting assumption is** — because that tells you exactly what is attackable and what isn't. An inherited "fact" from a meeting ("the customer rejected this") is almost always a *postulate wearing the costume of a common notion* — granted as if it were a law, when it is actually a contestable claim from one room on one day.

## The QEF↔QED switch (the higher-level router)

Euclid ends propositions two different ways, and the distinction tells you *which skill to reach for*:

- **Constructions ("problems")** close with *quod erat **faciendum*** — "which was to be **done**." You **built** a thing.
- **Theorems ("proofs")** close with *quod erat **demonstrandum*** — "which was to be **demonstrated**." You **proved** a claim.

> **Am I here to construct a solution, or to prove a position holds?**
> - **Construct** (QEF) → you're generating/exploring → use `/brainstorm`.
> - **Prove** (QED) → you have a position and need it to survive attack → use **`/euclid`** (or `/grill-me-euclid` for heavy problems).

If a session is still constructing, the gate has nothing to bite on yet — say so and route to brainstorm. The gate is a QED instrument.

---

## Mode 1: GATE (default invocation)

The mandatory armor-check before an artifact leaves your hands. This is where the skill earns its keep, because it forces the **reductio on your own lean** — the habit most thinkers skip on high-stakes, option-rich work.

### Step 0 — Confirm there is a target

Read the artifact / position. If there is **no formed lean or position** (it's still exploratory), STOP and say: *"There's no formed position to attack yet — this is still construction (QEF). Reductio needs a target. Want `/brainstorm` instead, or state your lean and I'll run the gate on it."*

### Step 1 — Extract the skeleton (Expose, applied to the artifact)

Before attacking, make the argument's bones explicit. Produce, briefly:

- **The lean(s):** every place the author has a preferred answer (even a soft one). Hunt for hedged leans — "options held open" documents often carry a visible lean on every page. *Held-open ≠ stress-tested.*
- **The definitions:** key terms the argument leans on. Flag any that shift meaning across the document or that the audience may not share.
- **The postulates:** every inherited "fact" the argument reasons *from*. Tag each: **definition / postulate / common notion**, and **verified / unverified**.
- **The option sets:** every "Option A/B/C" or enumerated choice.

### Step 2 — Run the hostile persona (Reductio + monoculture break)

**Adopt a genuine adversary, in a fresh frame.** Same-model self-review shares the blind spots that built the artifact — so the gate does NOT critique in the author's own voice. Spawn a subagent (via the Agent tool) with an explicit hostile persona so the attack runs in a clean context:

> *"You are the rival reviewer in the room who wants this proposal **killed**. You did not write it and you owe it nothing. Your job is to find the argument that ends it. For the author's lean, assume it is **wrong** and construct the strongest case that it is. You must surface at least one finding that, if true, sinks the recommendation — 'looks solid' is not an allowed verdict."*

For **each lean**, run the **reductio**: assume the opposite, follow it until it either (a) self-destructs — which *confirms* the lean, or (b) survives — which means the lean was **asserted, not demonstrated**, and the artifact is exposed. Report which.

If a richer multi-persona pass is wanted, an adversarial-review skill (multiple hostile personas, mandatory findings, severity promotion when two personas independently hit the same point) composes well here.

### Step 3 — Close the cases (Proof by cases)

For every option set, **prove the list is exhaustive**. Proof by cases is only valid if no case is missing. Actively hunt the **un-listed option** — the case the author didn't think of because it lives outside their frame (e.g. "a *third party* operates it" when the author only listed "we build it" vs "we do nothing"). A single missing case collapses an elimination argument. Report any case that isn't closed.

### Step 4 — Audit the postulates (State assumptions, in plain sight)

Walk the postulate list from Step 1. For each:

- Is it a **definition, postulate, or common notion**? (Only postulates are attackable.)
- Is it **verified or inherited-on-faith**? Inherited "facts" from a single meeting are postulates, not laws — mark them *"unverified postulate; reasoning downstream is conditional on it."*
- What downstream conclusions **collapse if this postulate is false**? Name them.

This is Russell's habit: assumptions stated in plain sight *so they can be challenged*. The output is a list the author can take into the room and defend or retire.

### Step 5 — Cite-the-rule sweep (Construct-and-cite)

Scan for the forbidden moves: **"clearly," "obviously," "it follows that," "therefore," "this strengthens," "more defensible."** For each, demand the warrant: *name the rule, or mark it conjecture.* Technical claims usually cite cleanly (they're grounded in how a system works); **strategic and organizational claims are where this most often fails** — plausible conclusions wearing the costume of findings. Flag every uncited inferential leap.

### Step 6 — Escalate (the adversary ladder)

State the gate's own ceiling honestly:

> **self-reductio (always, in-house) → external model (high-stakes) → the room (live).**

Same-model reductio is a **first filter, not a substitute for an external model** — it shares blind spots with the author. So:

- For **routine** work, the in-house gate is enough.
- For **high-stakes** artifacts (anything going to a review board, leadership, a customer, or an irreversible decision), **route to a genuinely external model** in adversarial/challenge mode (e.g. an OpenAI Codex or Gemini CLI "challenge" pass), and pass the lean + the survivors of Step 2. A different model family gives real cross-model diversity, not theater.
- Offer the escalation; don't force it. Use `AskUserQuestion` if unsure whether the stakes warrant it. (If no external model is available, say so — the in-house gate plus the live room are the remaining tiers.)

### Step 7 — Verdict

Produce a structured report:

```markdown
## Euclid Gate — <artifact>

**Verdict:** ARMORED / NEEDS WORK / NOT YET A POSITION (still QEF)

### Leans found, and whether each survived its reductio
- <lean>: SURVIVED (confirmed) | FELL (asserted, not demonstrated) — <one line>

### Cases not closed (missing options)
- <option set>: <the un-listed case that collapses the elimination>

### Postulates to defend or retire
| Assumption | Kind | Verified? | What dies if it's false |
|---|---|---|---|
| ... | postulate | no | ... |

### Uncited leaps (name the rule or mark conjecture)
- "<the 'therefore' phrase>" — <warrant, or "conjecture">

### Escalation
- [ ] Routed to external model (high-stakes) — <result> | Not needed (routine)

### What to do before this leaves your hands
<the 1–3 fixes that matter, ordered>
```

The point is not a clean bill of health. **The point is that the weak options are dead and the surviving claims are demonstrated before the artifact meets its hostile audience.**

---

## Mode 2: INLINE (invoke explicitly, or used by `/grill-me-euclid`)

Five linguistic triggers, each binding a habit to a cue **that already occurs in your reasoning**, so the discipline emerges from the conversation rather than being imposed. Kept **light** — fire on ambiguity/stakes that matter, respect the off-switch.

| When this appears in the conversation… | …fire this habit (one sharp question) |
|---|---|
| **A loaded noun** — "the platform," "done," "success," "risk," "scalable" | **Define:** "Does everyone mean the same thing by that word? Pin it before we build on it." |
| **An inherited fact** — "users won't tolerate that," "the customer rejected X" | **State the postulate:** "Tag it — definition, *postulate*, or common notion? Verified, or are we reasoning from faith? What dies if it's false?" |
| **An inferential connector** — "therefore / it follows / this strengthens / more defensible / clearly" | **Cite the rule:** "Name the warrant for that step, or mark it conjecture — not a finding." |
| **A whole problem stated at once** | **Decompose:** "What must be true *first* for this to hold? Build the dependency chain backward to something verifiable." |
| **A stated lean** — "I lean / the answer is / Option A is best" | **Reductio (light):** "Assume that's wrong. Steelman the kill. Does it self-destruct (you're confirmed) or survive (you've only asserted it)?" |

That last row is the instinct-builder: a *light* reductio fires on every stated lean, so the habit becomes reflexive over time — while the **gate** remains the deep, mandatory version. Inline builds the reflex; the gate is the ritual.

**Inline off-switch:** if the term is shared and concrete, the fact is genuinely established, or the inference is a single granted step — **do not fire.** Silence is part of the discipline. A trigger that fires on everything trains the user to ignore it.

---

## Operating Rules

### Tone
- Adversarial at the gate, not agreeable. The gate's job is to find the finding that sinks the artifact — "looks good" is a failure of the gate, not a success of the artifact.
- Inline: sharp and brief. One question, not a lecture. Name the habit only when it illuminates.
- Honest about the ceiling: always state that same-model reductio shares blind spots and name when an external model (or the live room) is the real test.

### Anti-Patterns
Do NOT:
- Run the gate on a position that doesn't exist yet (it's QEF — route to brainstorm).
- Critique in the author's own voice at the gate — adopt the hostile persona / fresh frame, or you reproduce the blind spots.
- Let a hedged lean ("decisions held open") escape the reductio — held-open is not stress-tested.
- Accept an option set without proving it exhaustive.
- Treat an inherited meeting "fact" as a common notion — it's a postulate; tag it as attackable.
- Fire inline triggers on every noun or every step — that's the over-application trap that gets the tool switched off.
- Present the in-house reductio as a substitute for a genuinely external model on high-stakes work.
- Pad the verdict with findings to look thorough — report the leaks that matter.

### The six habits, one-line each (quick reference)
1. **Define your terms** — pin meaning before building on a word.
2. **State your assumptions** in plain sight — so they can be challenged (tag definition/postulate/common-notion).
3. **Construct, don't assert** — build the conclusion; don't declare it.
4. **Cite the rule at every step** — name the warrant or mark it conjecture.
5. **Decompose** — break the complex into sub-problems simple enough to verify, then build forward.
6. **Reductio ad absurdum** — when you can't prove it directly, assume the opposite and follow it until it self-destructs.

---

## Example Invocations

```text
/euclid Harden docs/architecture-options.md before it goes to the design-review board.
```
Expected: GATE mode — extract the leans, run the hostile-persona reductio on each, close the option cases (hunt the missing one), audit the inherited assumptions, sweep for uncited strategic leaps, offer external-model escalation, produce the verdict.

```text
/euclid I lean toward building this as a single service. Try to kill it.
```
Expected: GATE on a single lean — assume it's wrong, build the strongest case for the opposite ("split it" / "don't build it"), report whether the lean survives.

```text
/euclid inline — grill me on this as I think, fire the triggers.
```
Expected: INLINE mode (or suggest `/grill-me-euclid` if the problem is heavy enough to warrant a full grill).

## Default Opening Move

1. Determine mode: **GATE** unless the user explicitly asks for inline.
2. **GATE:** confirm a formed position exists (Step 0). If yes, extract the skeleton and run Steps 1–7. If no, route to `/brainstorm` and say why.
3. **INLINE:** confirm the problem is being actively reasoned, then fire triggers lightly as cues appear. If the problem is heavy and the user wants the habits woven through a full interrogation, suggest `/grill-me-euclid`.

## Credits

Built from the critical-thinking habits in Euclid's *Elements* — the discipline of defining terms, stating postulates in plain sight, constructing rather than asserting, citing the rule at every step, decomposing into dependency chains, and disproving by contradiction. The "which habit when" router (six habits → three jobs → two firing points) and the adversarial gate are this skill's contribution. Composes with `/brainstorm`, `/grill-me`, and any adversarial-review or external-model challenge skill you have installed.

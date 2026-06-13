# Euclid

> Harden an argument before it leaves your hands — using the six critical-thinking
> habits Euclid's *Elements* trains, with the destructive test run on **your own**
> favored answer first.

**Type:** standalone (no shared structure, no config). Pairs with `brainstorm`, `grill-me`, and `grill-me-euclid`.

## The idea

Euclid's *Elements* shaped Lincoln, Hobbes, Einstein, and Russell — none of whom became geometers. What they took from it was a *form* of reasoning: define your terms, state your assumptions in plain sight, build conclusions one cited step at a time, and when you can't prove something directly, assume the opposite and follow it until it self-destructs.

This skill turns those habits into an executable discipline. The six habits group into **three jobs**:

- **EXPOSE** — define terms · state postulates *(the foundation; cheap to fix early)*
- **BUILD** — decompose into dependency chains · construct-and-cite *(the bridge)*
- **TEST** — reductio ad absurdum · proof by cases *(under fire; needs a formed target)*

The router: **diagnose which job the moment needs, and the two habits inside it are obvious.** Three things to hold in your head, not six.

## What it does

`/euclid` has two modes:

- **GATE (default)** — Point it at a *formed* position (a brief, a plan, a recommendation, a lean) and it runs an adversarial armor-check: a hostile reviewer persona, in a fresh frame, runs a **reductio on your own lean** (assume it's wrong; does the opposite collapse, or stand up?), **closes the cases** on every option set (is the list actually exhaustive, or is there a missing option?), **audits the postulates** (which inherited "facts" are contestable, and what dies if they're false?), and **sweeps for uncited leaps** ("therefore," "more defensible" — name the warrant or mark it conjecture). For high-stakes work it routes to a genuinely external model. The output is a verdict: which leans **survived** (demonstrated) vs **fell** (merely asserted), and what to fix before the artifact meets its audience.

- **INLINE** — While you reason, five linguistic triggers fire the right habit at the right moment (a loaded noun → *define*; an inherited fact → *state the postulate*; a "therefore" → *cite the rule*; a whole problem → *decompose*; a stated lean → *light reductio*). Kept deliberately light, with an explicit off-switch, so it sharpens rather than nags.

The habit most people skip — and the one the gate exists to force — is **running the reductio on the answer you've already fallen for.** You don't attack your own favored option; a hostile room does, live. Better to have done it first.

## When to use

- You have a formed position and want it hardened before a skeptical audience sees it.
- You want your **own lean attacked** — "I think it's X; try to kill it."
- You're not sure which thinking habit a moment calls for (use the router).

## When not to use

- You don't have a position yet, you're still exploring → use `/brainstorm` (constructing a solution is *quod erat faciendum*; this skill is for *quod erat demonstrandum* — proving a formed position holds).
- You want a relentless branch-by-branch plan interview → use `/grill-me`.
- You have a heavy problem and want the habits firing *inline as you think*, ending in the gate → use `/grill-me-euclid`.

## Install

```bash
cp -r skills/euclid ~/.claude/skills/
```

## Use

```
/euclid Harden docs/architecture-options.md before it goes to the review board.
```

```
/euclid I lean toward building this as a single service. Try to kill it.
```

## Optional companions

- An **adversarial-review** skill (multiple hostile personas) composes into the gate's Step 2 for a richer multi-persona pass.
- An **external-model challenge** skill (OpenAI Codex / Gemini CLI in adversarial mode) is the high-stakes escalation tier — genuinely different blind spots than a same-model self-review. The gate is honest about this ceiling and offers the escalation; it degrades gracefully if no external model is available.

## Credits

Built from the critical-thinking habits in Euclid's *Elements*. The "which habit when" router (six habits → three jobs → two firing points), the QEF/QED skill-selection switch, the definition/postulate/common-notion trichotomy, and the adversarial gate are this skill's contribution.

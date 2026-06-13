# Grill Me — Euclid

> A heavier `/grill-me` for problems weighty enough to warrant deeper thinking:
> the relentless one-question interview, but with Euclid's six critical-thinking
> habits driving the questions — and it closes by attacking your own conclusion.

**Type:** standalone. **Requires** the `euclid` skill installed (it provides the engine and the gate).

## What it does

`/grill-me-euclid` is the Euclidean edition of `/grill-me`. It keeps everything that makes `/grill-me` work — one skeptical question at a time, a recommended answer for each, codebase-aware questioning, a living session document, an ordered closing plan — and adds two things:

1. **The questions are Euclidean.** Instead of generic branch-walking, each question is selected by which Euclidean cue is live in the conversation:
   - a **loaded noun** → *define it* before building on it
   - an **inherited "fact"** → *tag it* (definition / postulate / common notion) and ask what dies if it's false
   - a **"therefore"** → *cite the rule*, or mark it conjecture
   - a **whole problem stated at once** → *decompose* it backward to something verifiable
   - a **stated lean** → a *light reductio* (assume it's wrong; does it collapse or stand?)

2. **It closes with the `/euclid` gate.** Before finalizing, it runs the adversarial armor-check on the position the interview produced: a hostile reviewer persona, in a fresh frame, runs the deep reductio on your own lean, closes the option cases, audits the postulate ledger, and (for high-stakes work) offers to escalate to an external model. The session document ends with a verdict: which conclusions **survived** and which **fell** as merely asserted.

Plain `/grill-me` interrogates a plan. This one interrogates *and* **proves** it — in the Euclidean *quod erat demonstrandum* sense — by making sure your own favored answer has been attacked before anyone else attacks it.

## When to use

- The problem is **heavy or high-stakes** — option-rich, hard to reverse, or bound for a skeptical room.
- You want both a thorough interview *and* adversarial proof of the result.
- You want the Euclidean habits to fire **inline as you think**, not just as a final review.

## When not to use

- A routine plan that just needs branch-by-branch resolution → use `/grill-me` (lighter).
- A finished artifact that only needs the armor-check, no fresh interview → use `/euclid` (gate only).
- You don't have a position yet → use `/brainstorm`.

## How it relates to the others

| Skill | What it does | Reach for it when |
|-------|--------------|-------------------|
| `brainstorm` | Construct a solution | You're still generating; no position yet |
| `grill-me` | Interrogate a plan branch-by-branch | Routine plan, shared understanding needed |
| **`grill-me-euclid`** | **Euclidean interview + closing gate** | **Heavy problem; interrogate AND prove** |
| `euclid` | The gate alone (armor-check a formed artifact) | The position is already written; just attack it |

## Install

```bash
cp -r skills/euclid ~/.claude/skills/          # the engine — required
cp -r skills/grill-me-euclid ~/.claude/skills/ # this skill
```

## Use

```
/grill-me-euclid We're deciding whether to make this a shared multi-tenant service or one service per customer.
```

Answer each question — or say "go with your recommended answer" — and when the interview converges, the gate runs automatically and writes its verdict into the session document.

## Credits

The interview mechanics are inherited from `/grill-me` (itself inspired by Matt Pocock's `grill-me`). The Euclidean question-selection layer and the closing adversarial gate come from the `euclid` skill in this repo.

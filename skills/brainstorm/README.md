# Brainstorm

> A Socratic thinking partner for deeply understanding a problem and finding the
> *simplest* resolution — grounded in mental models from "The Great Mental
> Models" by Shane Parrish.

**Type:** standalone (no shared structure, no config). Pairs naturally with
`/grill-me`.

## What it does

`/brainstorm` is for thinking, not building. Three phases:

1. **Domain understanding** — Socratic questioning, one question at a time, until
   the *real* problem surfaces (separated from the solution you walked in with).
2. **Solution research** — explores the space using high-quality sources, then
   proposes options **ranked simplest-first**. "Do nothing" and "accept the
   tradeoff" are valid outcomes.
3. **Documentation** — writes a high-signal exploration doc: problem statement,
   premises tested, mental models that actually illuminated, solutions, and a
   recommended approach with its key risk.

It has a built-in **escape hatch** — say "skip ahead" any time and it summarizes
and moves on.

## When to use

- You have a problem you want to think through before committing to a direction.
- You suspect you're about to over-engineer and want the simplest adequate fix.
- You want a thinking partner that pushes back instead of agreeing.

## When not to use

- You've already chosen a plan and want it stress-tested → use `/grill-me`.
- You're reviewing an existing system's architecture → use `/system-architecture-review`.
- Medical, legal, or mental-health problems → it will decline and point you to a professional.

## Install

```bash
cp -r skills/brainstorm ~/.claude/skills/
```

It uses `WebFetch` for solution research when available; if web access is
blocked it falls back to internal knowledge and says so.

## Use

```
/brainstorm I have notes scattered across five apps and can never find anything when I need it.
```

It opens with a single clarifying question — not a solution — and works from
there.

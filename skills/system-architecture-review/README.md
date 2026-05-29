# System Architecture Review

> A rigorous, principled review of an *existing* system's architecture — Socratic
> domain understanding, a review grounded in the field's authoritative texts, and
> an adversarial cross-model challenge to catch blind spots.

**Type:** standalone. Optionally uses a second AI model (e.g. OpenAI Codex,
Gemini) or a human peer for the adversarial pass — degrades gracefully if none
is available.

## What it does

Three phases (plus a document-ingestion Phase 0):

0. **Ingest** — reads a folder of architecture docs (ADRs, diagrams, specs,
   incident reports) and summarizes what it understands and what's missing.
1. **Domain understanding** — Socratic questioning to learn *why* the
   architecture is the way it is: constraints, tradeoffs, quality-attribute
   priorities, failure modes.
2. **Architecture review** — a principled evaluation against architectural
   characteristics, pattern fitness, modularity (cohesion/coupling/connascence),
   data architecture, communication patterns, and "hard parts" tradeoffs. Uses
   ATAM-style utility trees and scenario analysis. Produces a full report with
   severity-rated findings, sensitivity/tradeoff points, and an assumptions log.
3. **Adversarial review** — an **independent reviewer** does a *blind* assessment
   (it never sees Phase 2's findings) drawing only on reputable sources, then the
   two are compared and synthesized. This breaks the single-model monoculture.

### Grounded in

- *Fundamentals of Software Architecture (2nd ed.)* — Richards & Ford
- *Software Architecture: The Hard Parts* — Ford, Richards, Sadalage, Dehghani
- *The Great Mental Models* — Shane Parrish
- ATAM (SEI/CMU), Rozanski & Woods viewpoints, Paul & Elder Socratic standards

## The adversarial pass — reviewer options

The skill wants a *different* model or person than the one that did Phase 2:

| Option | How |
|--------|-----|
| Second AI via CLI/skill | OpenAI Codex (`codex` CLI or `/codex` skill), Gemini CLI, etc. |
| Second AI via API | call out via Bash to whatever you have configured |
| Human peer | the skill generates the blind-review prompt for you to hand off |
| None available | falls back to a self-adversarial re-review, clearly marked as lower-confidence |

You don't need any specific tool installed — it adapts.

## When to use

- You have an existing system and want a thorough, principled architecture review.
- You want strengths *and* improvement areas, not just a list of complaints.
- You want a second independent perspective to challenge the findings.

## When not to use

- Code-level / PR review → use a code-review workflow.
- Stress-testing a plan you've already chosen → use `/grill-me`.
- Open-ended exploration with no system yet → use `/brainstorm`.

## Install

```bash
cp -r skills/system-architecture-review ~/.claude/skills/
```

## Use

```
/system-architecture-review ./docs/architecture/
```

It reads the folder, summarizes, asks domain questions one at a time, then
produces `architecture-review-[system-name].md` in your working directory and
runs the adversarial pass before finalizing.

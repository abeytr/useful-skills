# Grill Me

> Stress-test a plan, design, or decision until every branch, dependency, and
> tradeoff is explicit and resolved — one question at a time.

**Type:** standalone (no shared structure, no config, no dependencies).

## What it does

`/grill-me` interviews you relentlessly about a plan you're about to execute.
It's not a brainstorm and not a cheerleader — it's a skeptical reviewer that
walks the decision tree branch by branch:

- **One question at a time**, highest-leverage first.
- **Every question comes with a recommended answer** — so you can move fast by
  saying "go with your recommendation."
- **Investigates the codebase** instead of asking, when the answer is already
  there.
- **Challenges incoherent answers** rather than letting them pass.
- **Ends with an ordered plan** — decisions made, decisions deferred, risks
  named, next steps.

It's the skill that produced the design conversation behind this very repo.

## When to use

- You have a plan and want it pressure-tested before you build.
- You need hidden assumptions surfaced and dependencies ordered.
- You're choosing between approaches and want the tradeoffs forced into the open.

## When not to use

- You don't yet understand the problem → use `/brainstorm`.
- You're reviewing an existing system's architecture → use `/system-architecture-review`.

## Install

```bash
cp -r skills/grill-me ~/.claude/skills/
```

## Use

```
/grill-me We want to split the auth service out of the monolith without breaking existing clients.
```

Then answer each question — or say "go with your recommended answer" to accept
the suggestion and move to the next branch.

---
name: "grill-me"
description: "Interview the user relentlessly about a plan, design, architecture, or implementation until shared understanding is reached. Use when the user says grill me, wants to stress-test a plan, wants every branch of the decision tree explored, or needs dependencies and tradeoffs resolved one question at a time. For each question, provide a recommended answer. If a question can be answered from the codebase, inspect the codebase instead of asking."
argument-hint: "Describe the plan, design, or decision you want stress-tested"
user-invocable: true
disable-model-invocation: false
---

# Grill Me

## Purpose

Use this skill to pressure-test a plan until the major design branches, dependencies, assumptions, and tradeoffs are explicit and resolved. The goal is not encouragement. The goal is a durable shared understanding.

## When to Use

- The user says "grill me"
- The user wants a plan or design stress-tested
- The user wants an architecture or implementation interrogated branch-by-branch
- The user needs hidden assumptions surfaced before execution
- The user wants dependencies resolved in a strict order

## Core Behavior

You are rigorous, skeptical, and sequential.

- Ask exactly one substantive question at a time.
- For every question, include a recommended answer.
- Prefer narrowing questions over open-ended brainstorming.
- Walk the decision tree one branch at a time.
- Resolve prerequisites before downstream choices.
- If the answer can be derived from the codebase, inspect the codebase instead of asking.
- Keep going until the plan is coherent, conflicts are resolved, and the remaining risks are explicit.
- Do not dump a questionnaire. This is an interview, not a form.

## Operating Rules

### 1. Build the Decision Tree First

Before asking the next question, infer the current decision tree:

- Objective: what outcome is required?
- Constraints: time, compatibility, performance, compliance, team norms
- Existing state: what already exists in code, infra, or process?
- Major branches: architecture, interfaces, data model, rollout, testing, ownership
- Dependencies: which decisions block others?
- Risks: what can fail, regress, or become expensive later?

Keep this tree implicit unless the user asks to see it. Use it to decide the next highest-value question.

### 2. Ask Highest-Leverage Questions First

Choose the next question using this order:

1. Missing objective or success criteria
2. Hard constraints or non-negotiables
3. Existing system realities discoverable from code or docs
4. Irreversible architectural decisions
5. Interface and contract decisions
6. Data and state management decisions
7. Operational concerns: rollout, migration, observability, failure handling
8. Testing and verification
9. Ownership, maintenance, and follow-up work

### 3. Prefer Investigation Over Questions

If the workspace exists and the answer is discoverable, investigate first.

Examples:
- If the question is about an API contract, read the handler, schema, or client.
- If the question is about current data flow, inspect the relevant modules.
- If the question is about an existing migration or deployment pattern, search the repo.

Then ask a sharper question based on findings, or state the derived answer and move to the next unresolved branch.

### 4. Format of Each Turn

Each interview turn should contain only these parts:

**Question**
A single concrete question that advances the plan.

**Why this matters**
One or two sentences on what this decision unlocks or constrains.

**Recommended answer**
Your best current recommendation, stated plainly.

**If you answer X, next I will ask**
Name the next branch that follows from this answer.

Keep the tone direct. Do not pad.

### 5. Branch Resolution Rules

When the user answers:

- Accept the answer if it is coherent with prior decisions.
- Challenge it if it conflicts with constraints, code reality, or stated goals.
- If multiple branches remain viable, recommend one and explain the tradeoff briefly.
- If the answer creates a new dependency, resolve that dependency before continuing downstream.
- If the user is vague, force specificity with the next question.

If the user does not know or will not decide:

- For irreversible or high-cost decisions, stop and force an explicit choice.
- For reversible decisions, recommend a default, label it as an assumption, and continue.
- Revisit any assumption before concluding the interview if later answers make it unstable.

### 6. Completion Criteria

Stop only when all of the following are true:

- The objective is explicit.
- Major architectural and implementation branches are resolved or intentionally deferred.
- Dependencies are ordered.
- Success criteria are testable.
- Key risks and unknowns are named.
- There is a clear next action or implementation sequence.

At completion, provide:

- A concise summary of the chosen plan
- The decisions that were made
- The decisions intentionally deferred
- The main risks
- The immediate next steps

## Interview Patterns

### Pattern: New Feature or System Design

Prioritize:
1. User outcome
2. Constraints
3. Existing system fit
4. API and data model
5. Failure modes
6. Rollout and verification

### Pattern: Refactor Plan

Prioritize:
1. Problem with current design
2. What must remain behaviorally stable
3. Migration boundary
4. Compatibility risks
5. Test coverage and rollback

### Pattern: Performance or Reliability Plan

Prioritize:
1. Current bottleneck or failure mode
2. Measurement method
3. Acceptable target
4. Candidate interventions
5. Operational risk
6. Verification after change

### Pattern: Codebase-Aware Grilling

When a workspace is available:
1. Inspect the relevant code first
2. State the discovered constraint or pattern
3. Ask only what the code cannot answer
4. Use file evidence to challenge weak assumptions

## Anti-Patterns

Do not do these things:

- Ask ten questions at once
- Ask questions whose answers are already in the codebase
- Offer multiple recommended answers without committing to one
- Jump to implementation details before resolving objectives and constraints
- Let contradictory answers pass without challenge
- End with vague agreement instead of an ordered plan

## Example Invocation

```text
/grill-me We want to split the monolith auth service into a standalone service without breaking existing clients.
```

Expected behavior:
- Inspect the current auth boundaries if code is available
- Ask the single highest-leverage unresolved question
- Recommend an answer
- Continue branch-by-branch until rollout, migration, and verification are defined

## Default Opening Move

If the user provides a plan with enough context, begin with the highest-leverage unresolved question.

If the plan is underspecified, begin here:

**Question**
What exact outcome must this plan achieve, and what would count as failure even if the implementation is technically correct?

**Why this matters**
This anchors every downstream tradeoff and prevents local optimizations that miss the real goal.

**Recommended answer**
State one primary outcome, two or three measurable success criteria, and the main failure conditions.

**If you answer X, next I will ask**
I will ask which constraints are fixed and which are negotiable.

---
name: "grill-me"
description: "Interview the user relentlessly about a plan, design, architecture, or implementation until shared understanding is reached. Use when the user says grill me, wants to stress-test a plan, wants every branch of the decision tree explored, or needs dependencies and tradeoffs resolved one question at a time. For each question, provide a recommended answer. If a question can be answered from the codebase, inspect the codebase instead of asking. Maintains a living session document that records every question, the user's answer in their own words, and periodic checkpoints, then closes with a summary of decisions, rationale, and a high-level plan."
argument-hint: "Describe the plan, design, or decision you want stress-tested"
user-invocable: true
disable-model-invocation: false
---

# Grill Me

> Inspired by Matt Pocock's `grill-me` skill
> (https://github.com/mattpocock/skills/blob/main/skills/productivity/grill-me/SKILL.md).
> The relentless one-question-at-a-time interview, recommended answers, and
> codebase-aware questioning come from his version; this implementation adds an
> explicit decision-tree model, a fixed per-turn format, leverage-based question
> ordering, branch-resolution rules, formal completion criteria, and a living
> session document with checkpoints and a closing decision summary.

## Purpose

Use this skill to pressure-test a plan until the major design branches, dependencies, assumptions, and tradeoffs are explicit and resolved. The goal is not encouragement. The goal is a durable shared understanding.

Every grilling session is recorded in a **living session document** so that both you and the user can later recall exactly what was discussed, what the user decided, and why. The document captures the user's answers in their own words (not paraphrased away), checkpoints progress at regular intervals, and closes with a clean summary of decision points, rationale, and a high-level plan.

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
- Maintain a living session document from the first turn. Log every question, the user's answer in their own words, and your reading of it. Checkpoint at regular intervals. Close with a summary.

## Operating Rules

### 0. Open and Maintain the Session Document

Before asking the first question, create a session document and keep it current for the entire interview. This is the durable record that lets both you and the user recall what was discussed, what was decided, and why — long after the conversation scrolls away.

**Where it lives**

- Create `grill-sessions/` in the current working directory if it does not exist.
- File name: `grill-sessions/YYYY-MM-DD-<topic-slug>.md` — use today's date from context.
- The `<topic-slug>` is required and must name the actual subject so the user can tell sessions apart at a glance in a file listing. Make it specific and human-readable: 3–6 kebab-case words drawn from the plan being grilled (e.g. `split-auth-service`, `checkout-latency-fix`, `migrate-billing-to-stripe`). Never use a generic placeholder like `session`, `plan`, `grill`, or `topic`.
- If the topic is not yet clear at creation time, ask the user for a one-line subject (or infer it from their opening message) before naming the file. Do not fall back to a generic name.
- If a workspace has an obvious docs or notes location the user prefers, use that instead — but keep the same dated, topic-named filename.
- Tell the user the full path once, when you create it.
- If the topic materially shifts mid-session, rename the file to match and tell the user the new path.

**Initial scaffold** (write this on the first turn):

```markdown
# Grill Session: <Topic>

- **Date:** <YYYY-MM-DD>
- **Status:** In progress
- **Subject:** <one-line description of the plan/design being stressed>

## Objective
_To be established in the first turns._

## Decision Log
<!-- One entry per resolved question. Append as you go. -->

## Open Threads
<!-- Questions asked but not yet answered, and deferred decisions. -->

## Assumptions
<!-- Defaults adopted because the user deferred a reversible decision. -->

## Checkpoints
<!-- Periodic running summaries. -->
```

**After every answered question**, append a Decision Log entry. Capture the user's answer verbatim or near-verbatim — do not sanitize it into your own phrasing, because the point is to recall what *they* said. Use this shape:

```markdown
### Q<n>: <the question in a few words>
- **Asked:** <the actual question>
- **Recommended (mine):** <the recommendation you gave>
- **User answered (their words):** "<quote or close paraphrase of what the user said>"
- **My reading:** <how you interpreted it / what it resolves>
- **Status:** Resolved | Deferred | Assumption | Revisit
- **Depends on / unblocks:** <links to other Q numbers if relevant>
```

Keep the user's words and your interpretation in separate fields so a later reader can tell them apart. If the user corrects an earlier answer, do not overwrite it — append a note and mark the original entry `Revisit → superseded by Q<n>`.

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

After the user answers, append the Decision Log entry (see Rule 0) before moving to the next question. The interview turn shown to the user stays clean — the logging happens in the document.

### 5. Checkpoint Discipline

Checkpoints make the session resumable and give the user a periodic chance to correct course.

Write a checkpoint to the document's **Checkpoints** section when any of these is true:

- Every 4–5 resolved questions.
- A major branch closes (e.g. objective locked, architecture chosen, rollout defined).
- The user says "checkpoint", "where are we", or "recap".
- Before ending the session.

Each checkpoint is short:

```markdown
### Checkpoint <n> — after Q<last>
- **Settled so far:** <2–4 bullets of what is now decided>
- **Still open:** <what remains>
- **Next branch:** <the question coming up>
```

When you write a checkpoint, surface a one-line pointer to the user in chat ("Checkpoint saved — N decisions logged, next up: X") so they know the record exists and can interject. Do not paste the whole document into chat unless asked.

### 6. Branch Resolution Rules

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

### 7. Completion Criteria

Stop only when all of the following are true:

- The objective is explicit.
- Major architectural and implementation branches are resolved or intentionally deferred.
- Dependencies are ordered.
- Success criteria are testable.
- Key risks and unknowns are named.
- There is a clear next action or implementation sequence.

At completion, do two things.

**1. Finalize the session document.** Flip `Status:` to `Complete`, ensure the Objective is filled in, and append a `## Summary` section with these subsections:

```markdown
## Summary

### Decision Points
<!-- Each material decision, one line: what was decided. -->

### Rationale
<!-- For each decision, why it was chosen over the alternatives — drawn from the Decision Log. -->

### Deferred / Assumptions
<!-- Decisions intentionally postponed and defaults adopted, with what would force a revisit. -->

### Risks
<!-- The main risks and unknowns that survive the interview. -->

### High-Level Plan
<!-- Ordered implementation sequence. Dependencies respected. The first concrete next action. -->
```

The Summary must be derivable from the Decision Log above it — it is a synthesis, not new content. Anyone reading only the Summary should understand what was decided and what to do next; anyone reading the Decision Log should be able to see the user's own words behind each decision.

**2. Report in chat.** Give the user the same five-part summary (decision points, rationale, deferred, risks, high-level plan) and the path to the finalized document.

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
- Run the whole interview without writing anything down, then try to reconstruct it from memory at the end
- Rewrite the user's answers into your own polished phrasing in the Decision Log — preserve their words
- Paste the entire session document into chat on every turn instead of a one-line pointer
- Overwrite a changed answer instead of marking the original `Revisit` and appending the new one

## Example Invocation

```text
/grill-me We want to split the monolith auth service into a standalone service without breaking existing clients.
```

Expected behavior:
- Create the session document and tell the user where it lives
- Inspect the current auth boundaries if code is available
- Ask the single highest-leverage unresolved question
- Recommend an answer
- Log each answer in the user's own words and checkpoint periodically
- Continue branch-by-branch until rollout, migration, and verification are defined
- Finalize the document with a summary of decisions, rationale, and a high-level plan

## Default Opening Move

First, create the session document (Rule 0) and tell the user its path in one line. Then proceed to the first question.

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

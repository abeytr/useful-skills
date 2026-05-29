---
name: "system-architecture-review"
description: |
  Architecture review partner for existing systems. Three phases:
  (1) Domain understanding via Socratic questioning informed by mental models from
  "The Great Mental Models" by Shane Parrish,
  (2) Architecture review grounded in "Fundamentals of Software Architecture 2nd Ed"
  by Richards & Ford and "Software Architecture: The Hard Parts" by Ford, Richards et al.,
  (3) Adversarial cross-model review by an independent reviewer (a second AI model
  such as OpenAI Codex or Gemini, or a human peer) using reputable external sources.
  Produces a comprehensive report with strengths, improvement areas, and validated findings.
  Use when the user says "review architecture", "architecture review", or provides
  system documentation for architectural analysis.
argument-hint: "Path to folder containing system architecture documents"
user-invocable: true
disable-model-invocation: false
allowed-tools:
  - Read
  - Grep
  - Glob
  - Write
  - Bash
  - Edit
  - Agent
  - WebFetch
  - Skill
---

# System Architecture Review

## Purpose

Partner with the user to deeply understand a system's architecture through its documentation, probe the domain via Socratic questioning, then perform a rigorous architecture review grounded in established principles. Conclude with an adversarial cross-model review by an independent reviewer (a second AI model or a human peer) to stress-test findings against external high-quality sources.

## When to Use

- The user says "architecture review" or "/system-architecture-review"
- The user provides a folder of system architecture documents for review
- The user wants a thorough, principled evaluation of an existing system design
- The user wants to identify architectural strengths and improvement areas

## When NOT to Use

- Code-level review (diffs/PRs) → use a code-review workflow
- Debugging or investigating a specific failure → use a debugging workflow
- Stress-testing a plan or decision you've already made → use `/grill-me`
- Open-ended problem exploration (no system to review yet) → use `/brainstorm`

---

## Philosophy

This skill draws from four intellectual traditions:

1. **Socratic Method** — Understanding emerges through disciplined questioning. Never assume the documentation tells the full story. Probe until the real constraints, tradeoffs, and motivations surface.
2. **Mental Models (Shane Parrish, "The Great Mental Models")** — Apply structured thinking tools to cut through architectural complexity and reveal hidden dynamics, coupling, and failure modes.
3. **Software Architecture Principles (Richards & Ford)** — Evaluate against established architectural characteristics, patterns, and tradeoffs from the field's authoritative texts.
4. **Adversarial Validation** — An independent cross-model review ensures no blind spots, using high-quality external sources beyond the primary reference texts. The reviewer is a *different* model or person than the one that produced the Phase 2 findings.

---

## Mental Models Toolkit

Apply these models during domain questioning and architecture analysis. Name the model when it genuinely illuminates — not as decoration on every turn.

**Guard against false analogy**: Physics and biology models are metaphors when applied to software systems. Use them to generate hypotheses and frame questions, not to prove conclusions.

### Volume 1: General Thinking Concepts

- **The Map Is Not the Territory** — The architecture diagrams and documents are not the system itself. What has been left out? What runtime behaviors differ from the documented design?
- **Circle of Competence** — Where does the team have deep expertise? Where are blind spots? What parts of the system extend beyond the team's demonstrated competence?
- **First Principles Thinking** — Strip the architecture to its fundamental requirements. What remains when you remove all inherited assumptions and "we've always done it this way"?
- **Thought Experiment** — "What would happen if this component failed?" "What if traffic increased 10x?" Explore consequences without cost.
- **Second-Order Thinking** — What are the consequences of the architectural decisions' consequences? What does the chosen pattern break elsewhere?
- **Probabilistic Thinking** — How likely are the failure scenarios? What does the base rate say about this pattern's success in similar contexts?
- **Inversion** — Instead of "how do we make this system reliable?", ask "what would guarantee this system fails?" What single points of failure exist?
- **Occam's Razor** — Among competing architectural approaches, prefer the simplest that meets requirements. Is complexity justified by actual (not hypothetical) needs?
- **Hanlon's Razor** — Don't attribute to incompetence what can be explained by constraints, history, or organizational dynamics at the time of design.

### Volume 2: Physics and Chemistry

- **Relativity** — The architecture looks different from the perspective of different stakeholders (ops, developers, users, business). How does each group experience it?
- **Reciprocity** — Every architectural decision creates reactions. What pressure does this design put on adjacent teams and systems?
- **Thermodynamics / Entropy** — Systems tend toward disorder. What active maintenance does this architecture require? What is decaying?
- **Inertia** — Existing systems resist change. What momentum exists that makes migration costly? What path dependencies are locked in?
- **Friction and Viscosity** — What slows down development, deployment, or change? Where is the resistance to evolution?
- **Catalysts** — What small architectural change could accelerate improvement across the entire system?

### Volume 3: Biology and Systems

- **Evolution / Natural Selection** — What earlier architectures existed? What survived and why? What was selected against?
- **Ecosystem Thinking** — The system exists in a larger ecosystem. What are the adjacent systems, feedback loops, and dependencies?
- **Niches** — Does the architecture serve a specific context well before attempting to generalize?
- **Red Queen Effect** — Is the team running just to keep the system operational? What breaks that cycle?
- **Adaptation vs. Extinction** — Can the current architecture adapt to new requirements, or must parts be replaced entirely?

### Volume 4: Numeracy and Mathematics

- **Scale** — Does the architecture behave differently at different scales? What works now that breaks at 10x or 100x?
- **Margin of Safety** — How much headroom exists? What happens when conditions are worse than expected?
- **Compounding** — Are there small architectural improvements that would compound? Is technical debt compounding?
- **Reversibility** — Which architectural decisions are reversible and which are not? Are irreversible decisions well-justified?
- **Distributions** — Is the system designed for averages or extremes? What do the tail cases look like?

---

## Socratic Questioning Framework

Use these question types during Phase 1, cycling through them as the conversation demands. Each question should target at least one intellectual standard (per Paul & Elder, "The Art of Socratic Questioning", 2007):

**Intellectual Standards to stress-test answers against:**
- Clarity, Accuracy, Precision, Relevance, Depth, Breadth, Logic, Fairness

**Question Types:**

1. **Clarification Questions** — "What do you mean by...?" "Can you give me a concrete example of how this component behaves?" "What does that look like in production?" *(Standards: clarity, precision)*
2. **Assumption-Probing Questions** — "What are you assuming about the load profile?" "What if the opposite were true?" "Why was this pattern chosen over alternatives?" *(Standards: accuracy, logic)*
3. **Evidence Questions** — "What evidence supports this scaling assumption?" "How do you know this boundary is correct?" "What data drove this decision?" *(Standards: accuracy, depth)*
4. **Perspective Questions** — "How does the ops team experience this?" "What would a new developer struggle with?" "How does the end user perceive this?" *(Standards: breadth, fairness)*
5. **Consequence Questions** — "What follows from this coupling?" "If that service fails, what cascades?" "What are the second-order effects of this choice?" *(Standards: depth, logic)*
6. **Meta Questions** — "Is this the right boundary to draw?" "Are we solving the right problem at this layer?" "What question about this system are we not asking?" *(Standards: relevance, breadth)*

### Question Log

For each question-answer exchange during Phase 1, internally track:
- **Target**: assumption, concept, or architectural element being probed
- **Evidence sought**: what would confirm or refute
- **Implication tested**: what follows if the answer is X vs. Y
- **Alternative viewpoint**: what someone who disagrees would say
- **Intellectual standard stressed**: which standard this question tests

This log feeds into the report's "Premises Explored" section and ensures questioning has genuine Socratic rigor rather than being merely sequential.

---

## Stakeholder Register and Viewpoints

Before beginning the review, identify and document stakeholders and required architectural views (per Rozanski & Woods, "Software Systems Architecture", 2011).

### Stakeholder Register

Identify at minimum:
- **Developers** — Who builds and maintains this system?
- **Operations/SRE** — Who runs it in production?
- **Business/Product** — Who defines requirements and priorities?
- **Users** — Who experiences the system's behavior?
- **Adjacent teams** — Who depends on or is depended upon?

For each stakeholder, note their primary concerns and what architectural properties matter most to them.

### Required Viewpoints (minimum set)

The review must address these views, whether from documentation or inferred from questioning:

1. **Context View** — System boundaries, external interfaces, actors
2. **Functional/Module View** — Internal decomposition, responsibilities, dependencies
3. **Component-and-Connector (Runtime) View** — Runtime elements, communication, concurrency
4. **Deployment/Allocation View** — Infrastructure, deployment topology, resource allocation
5. **Information/Data View** — Data ownership, flows, storage, lifecycle

If a view cannot be constructed from available information, note it as a gap with impact assessment.

---

## Architecture Review Dimensions

Grounded in **"Fundamentals of Software Architecture" (2nd Edition)** by Mark Richards and Neal Ford, and **"Software Architecture: The Hard Parts"** by Neal Ford, Mark Richards, Pramod Sadalage, and Zhamak Dehghani.

### Architectural Characteristics (from Fundamentals of Software Architecture)

Evaluate the system against these quality attributes, noting which are explicitly prioritized and which are neglected:

**Operational Characteristics:**
- Availability — uptime guarantees, redundancy, failover
- Continuity — disaster recovery, business continuity
- Performance — latency, throughput, capacity
- Recoverability — backup, restore, mean time to recovery
- Reliability / Safety — failure rate, data integrity
- Robustness — handling edge cases, error conditions, boundary conditions
- Scalability — ability to handle growth (vertical and horizontal)

**Structural Characteristics:**
- Configurability — ease of changing system behavior via configuration
- Extensibility — ability to add new functionality
- Installability — ease of deployment to required environments
- Leverageability / Reuse — ability to leverage common components
- Localization — support for multiple languages, data formats
- Maintainability — ease of modification and correction
- Portability — ability to run on multiple platforms
- Supportability — ability to diagnose and resolve issues
- Upgradeability — ability to upgrade to newer versions

**Cross-Cutting Characteristics:**
- Accessibility — access for all users including disabilities
- Archivability — data retention and purging
- Authentication — identity verification
- Authorization — access control
- Legal — regulatory compliance
- Privacy — data protection
- Security — resistance to attacks
- Usability — user experience and ergonomics

### Architecture Patterns Assessment (from Fundamentals)

Identify which pattern(s) the system employs and evaluate fitness:

- Layered Architecture
- Pipeline Architecture
- Microkernel Architecture
- Service-Based Architecture
- Event-Driven Architecture
- Space-Based Architecture
- Orchestration-Driven Service-Oriented Architecture
- Microservices Architecture

For each identified pattern, assess:
- Is the pattern appropriate for the problem domain?
- Are the pattern's inherent tradeoffs understood and acceptable?
- Is the implementation consistent with the pattern's principles?
- Are anti-patterns present (e.g., distributed monolith, nano-services)?

### Modularity Assessment (from Fundamentals)

- **Cohesion** — Do modules/services have clear, single responsibilities?
- **Coupling** — What is the degree of interdependence? (afferent/efferent coupling)
- **Connascence** — What types of coupling exist? (name, type, meaning, position, algorithm, execution, timing, values, identity)
- **Component boundaries** — Are boundaries drawn at natural fault lines?

### Hard Parts Analysis (from Software Architecture: The Hard Parts)

**Service Granularity:**
- Are services too coarse (monolithic tendencies) or too fine (nano-service overhead)?
- Granularity disintegrators: service scope and function, code volatility, scalability/throughput, fault tolerance, security, extensibility
- Granularity integrators: database transactions, workflow/choreography, shared code, data relationships

**Data Decomposition:**
- How is data ownership distributed across services?
- Are there shared databases creating hidden coupling?
- Data decomposition drivers: change control, connection management, scalability, fault tolerance, architectural quanta

**Distributed Transactions and Sagas:**
- How are cross-service transactions handled?
- Saga patterns: orchestration vs. choreography
- Compensating transactions for failure scenarios

**Reuse Patterns:**
- Code replication vs. shared library vs. shared service vs. sidecar/service mesh
- Platform-level reuse vs. application-level reuse

**Communication Patterns:**
- Synchronous vs. asynchronous
- Orchestration vs. choreography
- Request-reply vs. event-driven
- Data contracts and versioning

**Architectural Quantum:**
- Independently deployable artifacts with high functional cohesion and synchronous connascence
- Static coupling (compile-time dependencies)
- Dynamic coupling (runtime communication)

**Trade-off Analysis:**
- Every architecture decision is a trade-off
- Are trade-offs explicit and documented?
- Are the "least worst" options chosen with clear reasoning?

### Fitness Functions

- Are there architectural fitness functions (automated or manual) that verify the architecture maintains its characteristics over time?
- Deployment pipeline integration
- Governance mechanisms

### Utility Tree and Scenario-Based Analysis (from ATAM)

After identifying prioritized quality attributes in Phase 1, construct a utility tree:

1. **Root**: System utility (overall architectural goodness)
2. **Level 1**: Quality attributes (performance, availability, modifiability, security, etc.)
3. **Level 2**: Attribute refinements (e.g., under performance: latency, throughput, resource utilization)
4. **Level 3**: Concrete scenarios with stimulus, response, and response measure

Prioritize 6-10 architecturally significant scenarios by (High/Medium/Low):
- **Business importance** — How critical is this to stakeholders?
- **Technical risk** — How difficult is this to achieve given the architecture?

Focus the review on scenarios rated (H,H) or (H,M). For each priority scenario, identify:
- **Sensitivity points** — Architectural decisions where a small change significantly affects the scenario
- **Tradeoff points** — Decisions that affect multiple quality attributes in opposite directions
- **Risk themes** — Patterns of risk that emerge across multiple scenarios
- **Non-risks** — Decisions that are well-supported and unlikely to cause problems

This ensures findings are traceable to concrete, testable scenarios rather than abstract checklist judgments.

### Security and Trust Analysis

Beyond the checklist treatment of security as a quality attribute, perform:

1. **Trust boundary identification** — Where do trust levels change? (network boundaries, service boundaries, user-system boundaries)
2. **Data classification** — What data sensitivity levels exist? Where does sensitive data flow?
3. **Entry points and attack surfaces** — What are the system's external interfaces?
4. **Abuse cases** — For each key scenario, what could an attacker do?
5. **3-5 concrete security/privacy scenarios** — Specific threat scenarios rated by likelihood and impact

---

## Escape Hatch

At ANY point, if the user says "skip ahead", "let's move on", "I've explained enough", or signals impatience:

- Respect it immediately. Summarize what you understand so far.
- State what you're uncertain about (one sentence).
- Proceed to the next phase with what you have.

Never force a user to endure more questioning than they want. The skill serves the user, not the methodology.

---

## Iterative Back-Edges

Phases are not strictly waterfall. The following back-edges are expected and encouraged:

- **Phase 2 → Phase 1**: If the review reveals that a key quality attribute or constraint was not explored, pause the review and ask the clarifying question before continuing. State: "I need to revisit a domain question before I can assess this properly."
- **Phase 2 → Phase 0**: If the review requires artifacts not yet read (deployment configs, incident reports, monitoring data), request them from the user.
- **Phase 3 → Phase 2**: If the independent reviewer identifies a dimension the review missed entirely, add it to the Phase 2 analysis before finalizing.

When exercising a back-edge:
1. State which phase you're returning to and why (one sentence)
2. Ask the specific question or request the specific artifact
3. Resume the current phase with the new information
4. Note confidence level: findings without runtime/evidence validation carry lower confidence

---

## Phase 0: Document Ingestion

### Objective

Read and internalize all system documentation provided by the user.

### Protocol

1. When the user provides a folder path, recursively discover all relevant documents (markdown, text, diagrams, code, configuration files, ADRs, etc.)
2. **Risk-based triage**: Prioritize reading in this order:
   - **Priority 1** (always read fully): Business/domain context, architecture decision records (ADRs), system overview, interface specifications, deployment topology, data models, incident/post-mortem reports
   - **Priority 2** (read fully if available): Non-functional requirements/SLAs, security documentation, monitoring/observability setup, team structure documentation
   - **Priority 3** (sample as needed): Implementation details, configuration files, test documentation, operational runbooks
3. Build an internal model of:
   - System components and their responsibilities
   - Data flows and communication patterns
   - Technology stack and infrastructure
   - Stated design decisions and constraints
   - Team structure and organizational context (if available)
   - Non-functional requirements and SLAs (if documented)
4. After reading, present a brief summary (5-10 bullet points) of what you've understood from the documents.
5. Explicitly note what is NOT covered in the documentation — gaps, missing perspectives, undocumented assumptions.

### Phase Transition

After presenting the document summary, state:

> "I've read through the documentation. Here's my initial understanding: [summary]. I notice the following gaps: [gaps]. Before I begin the architecture review, I'd like to ask some questions to deepen my understanding of the domain and the decisions behind this design. Ready to proceed?"

STOP. Wait for confirmation.

---

## Phase 1: Domain Understanding (Socratic Exploration)

### Objective

Build deep domain understanding through Socratic questioning, informed by mental models. Understand not just WHAT the architecture is, but WHY it is this way, what constraints shaped it, and what problems it solves.

### Opening Move

Ask ONE question that targets the most significant gap identified during document ingestion:

> "From the documentation, I can see [observation]. What I'd like to understand first is: [question targeting the WHY behind a key architectural decision]."

*(Apply the most relevant mental model — e.g., "The Map Is Not the Territory" if the docs seem to omit important runtime behavior, or "First Principles" if the architecture seems to inherit patterns without justification.)*

### Questioning Protocol

1. Ask exactly ONE question at a time.
2. Cite the mental model only when it genuinely sharpens the question — not on every turn.
3. Explain briefly why this question matters for the review (one sentence).
4. Wait for the user's answer before proceeding.
5. Do NOT dump multiple questions. This is a dialogue, not a survey.
6. Track architectural decisions and their rationale as they emerge.
7. Note tensions between stated goals and observed patterns.

### Areas to Probe

- **Domain drivers** — What business problems does this solve? What are the key domain events?
- **Quality attribute priorities** — Which architectural characteristics matter most? Which were explicitly traded away?
- **Constraint origins** — Are constraints technical, organizational, regulatory, or historical?
- **Evolution history** — How did the architecture arrive at its current state? What was tried and abandoned?
- **Team topology** — How does team structure map to system structure? (Conway's Law)
- **Failure modes** — What has broken in production? What keeps the team up at night?
- **Growth vectors** — Where is change expected? What new requirements are emerging?
- **Data sensitivity** — What data is most critical? What are the consistency requirements?

### Exit Criteria

Phase 1 is complete when you can answer:

1. **What problem does this system solve, and for whom?**
2. **What are the top 3-5 architectural characteristics that matter most?**
3. **What constraints shaped the current design?**
4. **What has changed or is changing that puts pressure on the architecture?**
5. **What are the known pain points and failure modes?**

**Minimum**: 4 question-answer exchanges.
**Typical**: 6-10 exchanges.
**Maximum**: If you've asked 12 questions and still lack critical answers, summarize what you know, state what's missing, and ask if the user wants to continue or proceed with incomplete understanding.

### Phase Transition

When exit criteria are met, summarize your domain understanding:

> "I now have a solid understanding of the domain and the forces shaping this architecture. Let me reflect it back: [structured summary covering the 5 exit criteria]. Before I proceed with the detailed architecture review, is there anything I've missed or gotten wrong?"

STOP. Wait for confirmation before proceeding to Phase 2.

---

## Phase 2: Architecture Review

### Objective

Perform a thorough architecture review grounded in the principles from "Fundamentals of Software Architecture" and "Software Architecture: The Hard Parts". Identify strengths, weaknesses, risks, and improvement opportunities.

### Review Protocol

0. **Architecture recovery/validation** — Before drawing conclusions, compare documented architecture against available runtime evidence:
   - Deployment topology (actual infrastructure vs. documented)
   - Interface contracts (API specs, message schemas vs. documented flows)
   - Incident history (what has actually broken vs. stated reliability)
   - Monitoring/observability (what is measured vs. stated priorities)
   - If runtime evidence is unavailable, note which conclusions carry lower confidence as a result.

1. **Assess against stated priorities** — Does the architecture actually deliver on the characteristics the team prioritizes?

2. **Pattern identification and fitness** — What architectural pattern(s) are in use? Are they appropriate for the domain and constraints?

3. **Modularity analysis** — Evaluate cohesion, coupling, and connascence across component/service boundaries.

4. **Data architecture** — How is data owned, shared, and synchronized? Are there hidden couplings through shared databases?

5. **Communication and integration** — How do components/services communicate? Are patterns consistent and appropriate?

6. **Evolutionary architecture** — Can the system adapt to likely future requirements? Are there fitness functions?

7. **Hard parts assessment** — Where are the genuinely difficult tradeoffs? Are they acknowledged and well-reasoned?

8. **Anti-pattern detection** — Identify known architectural anti-patterns (distributed monolith, god services, anemic services, hub-and-spoke bottlenecks, etc.)

### Mental Model Application During Review

Apply mental models to deepen analysis:

- **Inversion**: "What would guarantee this architecture fails?" → identifies critical risks
- **Second-Order Thinking**: "What do these coupling choices break downstream?" → reveals hidden costs
- **Scale**: "Does this work at 10x load? 100x?" → exposes scaling limits
- **Entropy**: "What is actively decaying?" → identifies maintenance burdens
- **Ecosystem Thinking**: "What feedback loops exist?" → reveals systemic dynamics
- **Margin of Safety**: "Where is there no headroom?" → finds brittleness

### Scoring Rubric

For each dimension reviewed, classify using both a rating and supporting attributes:

**Ratings:**
- **Strength** — Well-designed, appropriate for context, demonstrates good architectural thinking
- **Adequate** — Meets current needs, no immediate concern, but not exceptional
- **Concern** — Shows signs of strain, technical debt accumulating, or misalignment with stated goals
- **Critical** — Active risk to system reliability, team velocity, or business goals; requires attention

**For Concern and Critical findings, also assess:**
- **Impact**: What quality attribute scenarios are affected? (link to utility tree)
- **Likelihood**: How probable is the negative outcome? (High/Medium/Low)
- **Blast radius**: How much of the system is affected if this goes wrong?
- **Reversibility**: How costly is it to change this decision later? (Easy/Moderate/Expensive/Irreversible)
- **Confidence**: How much evidence supports this finding? (Verified/Inferred/Speculative)

Findings rated "Speculative" confidence should be presented in an appendix, not the main findings.

### Report Generation

After completing the review, generate a comprehensive report. Write it to the working directory as `architecture-review-[system-name].md`.

#### Report Structure

```markdown
# Architecture Review: [System Name]

**Date**: [YYYY-MM-DD]
**Reviewer**: [AI-assisted architecture review]
**Documents Reviewed**: [list of source documents]
**Domain**: [domain description]

---

## Executive Summary

[3-5 sentences capturing the overall assessment, key strengths, and most critical improvement areas]

## System Understanding

### Domain Context
[Brief description of what the system does, for whom, and why]

### Architecture Overview
[High-level description of the architecture as understood — pattern, components, data flows, infrastructure]

### Key Architectural Decisions
[Significant decisions identified and their rationale]

### Prioritized Quality Attributes
[The characteristics that matter most, as understood from Phase 1]

---

## Strengths

### [Strength 1: Title]
**Dimension**: [which review dimension]
**Observation**: [what was observed]
**Why this matters**: [why this is good architecture]
**Principle**: [relevant principle from Richards/Ford if applicable]

[Repeat for each strength]

---

## Areas for Improvement

### [Area 1: Title]
**Severity**: Concern | Critical
**Dimension**: [which review dimension]
**Observation**: [what was observed]
**Risk**: [what could go wrong or is going wrong]
**Principle**: [relevant principle from Richards/Ford]
**Recommendation**: [specific, actionable suggestion]
**Trade-off**: [what the recommendation costs]

[Repeat for each area, ordered by severity then impact]

---

## Detailed Analysis

### Architectural Characteristics Assessment

| Characteristic | Rating | Notes |
|---------------|--------|-------|
| [each relevant characteristic] | Strength/Adequate/Concern/Critical | [brief note] |

### Pattern Fitness

[Analysis of whether the chosen pattern(s) suit the domain]

### Modularity

[Cohesion, coupling, and boundary analysis]

### Data Architecture

[Data ownership, consistency, and coupling analysis]

### Communication Patterns

[Integration and communication assessment]

### Evolutionary Readiness

[Ability to adapt to anticipated changes]

---

## Mental Models Applied

[Only models that genuinely illuminated something]

- **[Model Name]**: [What it revealed about the architecture]

---

## Risk Themes, Sensitivity Points, and Tradeoffs

### Sensitivity Points
[Architectural decisions where small changes significantly affect quality attributes]

| Decision | Affected Scenario | Sensitivity |
|----------|------------------|-------------|
| ... | ... | ... |

### Tradeoff Points
[Decisions that affect multiple quality attributes in opposite directions]

| Decision | Positive Effect | Negative Effect |
|----------|----------------|-----------------|
| ... | ... | ... |

### Risk Themes
[Patterns of risk that emerge across multiple findings]

### Non-Risks
[Decisions that are well-supported and unlikely to cause problems — explicitly noted to avoid revisiting]

### Assumptions and Unknowns
[What the review assumes to be true but could not verify]

| Assumption | Impact if Wrong | Confidence |
|-----------|----------------|------------|
| ... | ... | High/Medium/Low |

---

## Architectural Decision Records

| Decision | Drivers | Options Considered | Chosen | Consequences | Revisit Trigger |
|----------|---------|-------------------|--------|--------------|-----------------|
| ... | ... | ... | ... | ... | ... |

---

## Open Questions

[Unresolved items that warrant further investigation]

## References

- Richards, M. & Ford, N. (2024). Fundamentals of Software Architecture (2nd ed.). O'Reilly Media.
- Ford, N., Richards, M., Sadalage, P., & Dehghani, Z. (2021). Software Architecture: The Hard Parts. O'Reilly Media.
- Parrish, S. (2019-2024). The Great Mental Models (Vols. 1-4). Latticework Publishing.
- Bass, L., Clements, P., & Kazman, R. (2021). Software Architecture in Practice (4th ed.). Addison-Wesley.
- Rozanski, N. & Woods, E. (2011). Software Systems Architecture (2nd ed.). Addison-Wesley.
- Kazman, R. et al. Architecture Tradeoff Analysis Method (ATAM). SEI/CMU.
- Paul, R. & Elder, L. (2007). The Art of Socratic Questioning. Foundation for Critical Thinking.
- [Any additional sources cited during review]
```

### Phase Transition

After writing the report, present a summary to the user:

> "I've completed the architecture review and written the full report to `[filename]`. Here's the executive summary: [summary]. Key strengths: [top 2-3]. Critical areas: [top 2-3]. I'll now run an adversarial review with an independent reviewer (a second AI model such as Codex or Gemini, or a human peer) to stress-test these findings against external sources. Proceed?"

STOP. Wait for confirmation before Phase 3.

---

## Phase 3: Adversarial Review (Independent Cross-Model)

### Objective

Have an **independent reviewer** perform a blind adversarial review of the architecture, explicitly drawing on high-quality external sources beyond the primary reference texts. The reviewer attempts to find weaknesses, missed patterns, alternative approaches, and contradictions in the Phase 2 findings.

### Choosing the Reviewer

The point is independence: a *different* model or person than the one that produced Phase 2. Use whatever is available, in rough order of preference:

- **A second AI model via CLI/tool** — e.g. OpenAI Codex (`codex` CLI or the `/codex` skill if installed), Gemini CLI, or any other model you can invoke. If you have a skill named `codex`, invoke it with the Skill tool.
- **A second model via its API** — call out via Bash to whatever CLI/API the user has configured.
- **A human peer reviewer** — if no second model is available, generate the blind-review prompt below and hand it to the user to run themselves or pass to a colleague.

**If no independent reviewer is available at all:** tell the user, and offer a degraded alternative — re-run the review yourself from an explicitly adversarial stance ("assume Phase 2 was too generous; try to refute each strength and find what was missed"). Mark these findings as self-adversarial, not independent, and note the lower confidence.

### Protocol

The adversarial review uses a **two-pass approach** to avoid anchoring bias:

**Pass 1: Blind Review (independent assessment)**

1. **Prepare a neutral context file** — Write ONLY the architecture description (from Phase 0 + Phase 1) to a file the reviewer can access. Do NOT include Phase 2 findings, ratings, or recommendations. This ensures the reviewer forms independent judgments.

2. **Invoke the reviewer** — Hand it the blind-review prompt below, which:
   - Provides the system architecture summary (domain, components, data flows, decisions, constraints)
   - Instructs an independent architecture evaluation
   - Explicitly requires reputable high-quality sources:
     - Excellent books in the specific domain (e.g., "Designing Data-Intensive Applications" by Kleppmann, "Building Evolutionary Architectures" by Ford/Parsons/Kua, "Domain-Driven Design" by Evans, "Release It!" by Nygard, "Building Microservices" by Newman, etc.)
     - Research articles and conference papers (e.g., ACM, IEEE, InfoQ)
     - Materials published by well-known experts (Martin Fowler, Gregor Hohpe, Pat Helland, Werner Vogels, etc.)
   - Asks for: strengths, risks, anti-patterns, improvement opportunities, and alternative approaches
   - Requests a specific source cited for each finding

**Pass 2: Comparison and Challenge**

3. **Compare independently** — After receiving the reviewer's blind assessment:
   - Map the reviewer's findings against Phase 2 findings
   - Identify what the reviewer found that Phase 2 missed (new findings)
   - Identify what Phase 2 found that the reviewer missed (validation of depth)
   - Identify disagreements in severity or assessment
   - For each reviewer finding, assess: source type, exact claim, whether direct evidence or inference

4. **Validate and synthesize** — For each finding:
   - Evaluate validity against the actual system documentation
   - Check cited sources for plausibility (flag potential hallucinated citations)
   - Determine if new findings genuinely add value or are generic advice
   - Note confidence level: verified (traceable to evidence) vs. inferred (plausible but unverified)

5. **Update the report** — Incorporate validated findings into the final report:
   - Add a new section "## Adversarial Review Findings"
   - Include findings that add genuine value
   - Note disagreements and provide a synthesized perspective with reasoning
   - Add any new sources cited by the reviewer to the references
   - Mark each finding's provenance: `[reviewer-verified]` or `[reviewer-inferred]`

### Blind-Review Prompt

Give this to the independent reviewer (drop the leading `/codex` if you're not using the codex skill — the body works for any model or human reviewer):

```
You are performing an independent architecture evaluation. Your job is to
assess this system architecture and identify strengths, risks, anti-patterns,
and improvement opportunities. Form your own judgments — do not reference any
prior review.

SYSTEM ARCHITECTURE:
[paste architecture summary from Phase 0 + Phase 1 domain understanding ONLY]
[include: domain context, components, data flows, communication patterns,
 technology stack, constraints, quality attribute priorities, known pain points]
[DO NOT include: Phase 2 findings, ratings, or recommendations]

INSTRUCTIONS:
1. Draw EXCLUSIVELY on reputable, high-quality sources:
   - Authoritative books (Kleppmann, Evans, Newman, Nygard, Hohpe, Vernon, etc.)
   - Research papers (ACM, IEEE, peer-reviewed)
   - Expert publications (Fowler, Helland, Vogels, Dehghani, etc.)
   - Conference talks from reputable venues (QCon, Strange Loop, GOTO, etc.)
2. For each finding, cite the specific source (author, title, year)
3. Identify:
   - Architectural strengths (what is well-designed)
   - Risks and potential failure modes
   - Anti-patterns present in the design
   - Alternative architectural patterns worth considering
   - Missing industry best practices
   - Security/privacy concerns based on trust boundaries
4. For each finding, rate severity: CRITICAL / WARNING / NOTE
5. Do NOT use blog posts, Medium articles, or unverified sources
6. Be specific and actionable — vague concerns are worthless
```

### Final Report Update

After the adversarial review, append to the report:

```markdown
---

## Adversarial Review Findings

**Review method**: Independent adversarial review by [reviewer — e.g. OpenAI Codex / Gemini / human peer], drawing on external high-quality sources.

### New Findings

[Findings from the reviewer that add genuine value, not already in the Phase 2 report]

### Severity Adjustments

[Where the reviewer disagreed with Phase 2 severity ratings, with reasoning]

### Additional Sources Cited

[New references from the reviewer's analysis]

---

## Synthesis and Final Assessment

[Integrated view combining Phase 2 and Phase 3 findings. Where the primary review
and the independent reviewer disagree, present both perspectives with reasoning.
Final recommendation prioritization.]
```

### Delivery

After completing the final report:

> "The adversarial review is complete. I've updated the report at `[filename]` with the independent reviewer's findings and a synthesized final assessment. Key additions from the adversarial review: [top 2-3 new findings]. Here's the final executive summary: [updated summary]."

---

## Operating Rules

### Tone

- Rigorous but respectful. This is a review, not a prosecution.
- Direct about findings. Don't soften critical issues with excessive hedging.
- Acknowledge good decisions — architecture is about tradeoffs, and good tradeoffs deserve recognition.
- Honest about uncertainty. "I don't have enough information to assess this" is valid.

### Anti-Patterns

Do NOT:
- Ask multiple questions at once during Phase 1
- Skip document ingestion and jump to questioning
- Review without understanding the domain context
- Apply architectural principles dogmatically without considering constraints
- Present findings without actionable recommendations
- Treat all issues as critical — severity must be calibrated
- Ignore organizational context (Conway's Law, team capabilities, time pressure)
- Force mental model citations when they don't add insight
- Skip the adversarial review when an independent reviewer is available
- Accept the reviewer's findings uncritically — synthesize with your own analysis
- Use low-quality sources (blogs, listicles, unverified content)

### Pacing

- Phase 0: Thorough document read, concise summary
- Phase 1: 4-12 exchanges depending on domain complexity (respect the escape hatch)
- Phase 2: Detailed review, comprehensive report
- Phase 3: independent adversarial challenge, synthesized update

### Source Quality Standards

Throughout all phases, maintain high source quality:

**Acceptable sources:**
- Peer-reviewed academic publications
- Books by recognized domain experts (cite title, author, year)
- Published case studies with measurable outcomes
- Official documentation and specifications
- Conference presentations from reputable venues
- Practitioners with demonstrated, verifiable expertise

**Unacceptable sources:**
- Blog spam, listicles, content marketing
- Unverified claims or anecdotes
- Social media posts
- Sources without identifiable authors
- Marketing materials from vendors

---

## Example Invocation

```text
/system-architecture-review /path/to/architecture-docs

# or

/system-architecture-review
> Here's the folder with our system docs: ./docs/architecture/
```

Expected behavior:
1. Read all documents in the provided folder
2. Present summary and identify gaps
3. Ask Socratic questions to understand domain deeply (one at a time)
4. Perform comprehensive architecture review per Richards/Ford principles
5. Generate detailed report with strengths and improvement areas
6. Run adversarial review with an independent reviewer (a second AI model or a human peer) using high-quality external sources
7. Synthesize findings and deliver final report

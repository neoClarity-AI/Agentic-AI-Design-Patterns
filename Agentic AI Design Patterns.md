# Agentic AI Design Patterns

## Table of Contents

**[Design Principles](#design-principles)**

**[How to Read These Patterns](#how-to-read-these-patterns)**

**[Architecture Patterns](#architecture-patterns)**

- [Agentic Operating System Pattern](#agentic-operating-system-pattern)
- [AOS Factory Pattern](#aos-factory-pattern)
- [Agent Factory Pattern](#agent-factory-pattern)

**[Governance Patterns](#governance-patterns)**
- [Governance-First Pattern](#governance-first-pattern)
- [Trustworthy Autonomy Pattern](#trustworthy-autonomy-pattern)
- [Memory Isolation Pattern](#memory-isolation-pattern)
- [Self-Improvement Pattern](#self-improvement-pattern)
- [Self-Validation Pattern](#self-validation-pattern)
- [Contract Pattern](#contract-pattern)
- [Circuit Breaker Pattern](#circuit-breaker-pattern)

**[Capability Patterns](#capability-patterns)**
- [Chief of Staff Pattern](#chief-of-staff-pattern)
- [Router Pattern](#router-pattern)
- [Loop Pattern](#loop-pattern)
- [Subagent Pattern](#subagent-pattern)

**[Productivity Patterns](#productivity-patterns)**
- [Assistant Pattern](#assistant-pattern)
- [Researcher Pattern](#researcher-pattern)
- [Collaborator Pattern](#collaborator-pattern)
- [Organizer Pattern](#organizer-pattern)
- [Administrator Pattern](#administrator-pattern)
- [Instructor Pattern](#instructor-pattern)

## Design Principles

The patterns in this catalog are shaped by a consistent set of design principles, drawn from the design specification in the [Open AOS Factory](https://github.com/neoClarity-AI/Open-AOS-Factory) reference implementation. Some are stated explicitly in the specification's own principles section. Others are embodied throughout the design without being named. Together they explain *why* the architecture, governance, capability, and productivity patterns take the forms they do.

**Spec-Driven Single Source of Truth.** The design specification is canonical, and the factory, builders, plugin, and documentation are all renderings of it. Regenerating from the spec reproduces the same system, every generated file is stamped with the spec version it came from, and an instance's own evolving version is tracked separately from that provenance, so the whole system stays auditable and reproducible.

**Governance Before Productivity.** Safety, memory, coordination, and quality review are foundational rather than additive: the required governance agents exist before any productive agent, so a fast, capable, ungoverned system is never a possible end state.

**Single Responsibility and Separation of Concerns.** Each agent owns one domain with explicit non-responsibilities and each file has one purpose recoverable from its path, and the design keeps roles cleanly divided: builders apart from the agents they build, coordinators apart from specialized workers.

**Non-Destructive, Approval-Gated Safety.** The system prefers actions that cannot lose work, creating or appending rather than overwriting, deleting, or moving. Anything consequential is gated behind the user typing `Proceed`. That approval covers only the exact action described and never becomes a standing grant. Tool access is denied until explicitly granted. Logs are append-only, corrected by new entries rather than rewritten.

**Standardized, Extensible Schemas.** Uniform schemas and controlled vocabularies, with conventions like kebab-case slugs and ISO dates, keep generated files legible and let new agents and capabilities be added without inventing new formats or restating shared rules, since configs reference global permissions and the tool-access matrix instead of duplicating them.

**Controlled Evolution.** An instance is a living system, so every file is either a regenerable definition file owned by the factory or an accumulating data file owned by operating agents, and no operating agent edits a definition file. Updates run through dry-run previews that warn on version mismatch, block only on explicitly broken compatibility, and never silently overwrite.

**Multi-Instance Routing and Memory Isolation.** One workspace can host many instances as siblings, with a router that resolves exactly one active target per request and never blends one instance's memory into another.

**Self-Documenting and Self-Improving.** Every instance generates a plain-language guide and improves itself on daily-through-quarterly rhythms, making documentation and maintenance built-in behaviors rather than manual afterthoughts.

**Conversational and No-Code.** The primary interface is conversational and built for the non-technical owner: setup is an interview, agents are invoked by intent-matched trigger phrases, and the only exact-string command in the entire system is `Proceed`.

**Portability.** The system is plain markdown plus a thin routing layer, so it ports beyond its target platform and adapts to other models. Platform-specific mechanisms are avoided wherever a portable one exists.

## How to Read These Patterns

Each pattern is documented with a template adapted from the "Gang of Four" [Design Patterns](https://en.wikipedia.org/wiki/Design_Patterns) book, in this field order: Intent; Classification; Motivation (a brief narrative scenario); Applicability; Structure; Participants; Collaborations; Consequences; Implementation; Known Uses; Related Patterns.

This template deviates from the canonical GoF format in a few deliberate ways. *Also Known As* and *Sample Code* are omitted, because these patterns are conceptual rather than code-level. Motivation is written as a short narrative that states the problem and how the pattern resolves it, in keeping with the original GoF scenario style.

Two optional fields appear only where useful. *Variants* lists the sub-types of a pattern and is an intentional extension to the GoF template (used by the Loop and Circuit Breaker patterns). It appears after Applicability. *Reference Implementation* names the concrete agent or agents that realize a Productivity pattern within an Agentic Operating System. It appears just before Known Uses.

## Architecture Patterns

Architecture Patterns define the overall structure and organization of an Agentic AI system. They describe how agents, governance mechanisms, memory systems, workflows, and coordination structures are assembled into a coherent whole. These patterns operate at the system level and provide the foundation upon which governance, capability, and interaction patterns are built. This category also includes the factory patterns that construct individual agents and complete systems.

### Agentic Operating System Pattern

**Intent:**  
Create a cohesive system of specialized agents that work together through shared governance, memory, workflows, and coordination mechanisms to achieve complex goals.

**Classification:**  
Architecture Pattern

**Motivation:**  
Individual agents can solve narrow problems, but many real-world objectives require multiple capabilities operating in concert. As the number of agents grows, coordination becomes increasingly difficult: when agents function as isolated tools, they duplicate work, lose shared context, conflict with one another, and operate without consistent oversight. An Agentic Operating System (AOS) addresses this by providing a structured environment (shared governance, memory, workflows, and coordination) in which specialized agents collaborate as an integrated system rather than as disconnected tools, giving complex objectives a coherent home.

**Applicability:**  
Use this pattern when an objective needs several specialized capabilities working in concert under shared oversight, rather than a single agent or a loose collection of disconnected tools.

**Participants:**  
The operating-system framework, specialized agents with clearly defined responsibilities, the governance layer, the memory layer, the workflow layer, and the user. 

**Collaborations:**  
Specialized agents operate within the shared governance, memory, and workflow layers. Coordination protocols and escalation paths route work between them, while governance constrains every action. 

**Consequences:**  
Gives complex objectives a coherent home and reduces duplicated work, lost context, and ungoverned action. The cost is the overhead of building and maintaining the shared layers, plus a risk of bottlenecks at the coordination layer. 

**Implementation:**  
Establish the operating system as shared governance mechanisms, structured memory systems, standardized workflows, coordination protocols, and defined permissions and escalation paths. Then add specialized agents with clearly defined responsibilities on top of that foundation. 

**Known Uses:**  
The Agentic Operating System assembled by the [Open AOS Factory](https://github.com/neoClarity-AI/Open-AOS-Factory), comprising governance agents (Chief of Staff, Memory, Security, Review) and optional productive agents.

**Related Patterns:**  
[Governance-First Pattern](#governance-first-pattern), [AOS Factory Pattern](#aos-factory-pattern), [Chief of Staff Pattern](#chief-of-staff-pattern), [Memory Isolation Pattern](#memory-isolation-pattern), [Contract Pattern](#contract-pattern) (contracts as standard interfaces between agents, workflows, tools, memory, permissions, and outputs).

**Structure:**

```text
User
  |
Chief of Staff
  |
+---------------------------+
| Agentic Operating System  |
+---------------------------+
| Governance Layer          |
| Memory Layer              |
| Workflow Layer            |
| Agent Layer               |
+---------------------------+
      |
+-----+-----+-----+-----+
|           |           |
Research  Writing   Task Mgmt
Agent     Agent     Agent
```

---

### AOS Factory Pattern

**Intent:**  
Construct complete agentic operating systems from modular agents.

**Classification:**  
Architecture Pattern

**Motivation:**  
Assembling agentic systems agent-by-agent, ad hoc, produces inconsistent design, fragmented governance, and integration gaps. A factory process avoids this by constructing complete operating systems from standardized, modular agents and a defined assembly process, guaranteeing coherent design and uniform governance across the whole system.

**Applicability:**  
Use this pattern when you need to stand up a complete, governed operating system of agents and want consistent design and uniform governance rather than assembling agents ad hoc.

**Participants:**  
Factory process, multiple modular agents.

**Collaborations:**  
Factory integrates governance and functional agents into a system.

**Consequences:**  
Ensures consistency. May reduce flexibility in one-off agents.

**Implementation:**  
Standardize agent templates and system assembly processes.

**Known Uses:**  
The [Open AOS Factory](https://github.com/neoClarity-AI/Open-AOS-Factory) is a reference implementation of this pattern (the Build AOS builder and per-agent builders).

**Related Patterns:**  
[Agent Factory Pattern](#agent-factory-pattern), [Agentic Operating System Pattern](#agentic-operating-system-pattern), [Governance-First Pattern](#governance-first-pattern).

**Structure:**  
TBD: Diagram of a factory producing a system of agents.

---

### Agent Factory Pattern

**Intent:**  
Create individual agents tailored to specific roles.

**Classification:**  
Architecture Pattern

**Motivation:**  
Not all problems justify a whole operating system. Standing up an entire AOS for a single, narrow need is wasteful, yet hand-building one-off agents is slow and inconsistent. A factory resolves this tension by producing individual agents from role-specific templates and configurations, delivering a tailored, specialized agent quickly while keeping a consistent baseline.

**Applicability:**  
Use this pattern when a single narrow need doesn't justify a whole operating system, but you still want a tailored agent produced quickly from a consistent baseline.

**Participants:**  
Factory process, single agent.

**Collaborations:**  
Factory customizes each agent to its intended task.

**Consequences:**  
Increases flexibility but might lead to inconsistent governance if used alone.

**Implementation:**  
Provide agent templates and role-specific configurations.

**Known Uses:**  
Agent builders for single-purpose bots. The [Open AOS Factory](https://github.com/neoClarity-AI/Open-AOS-Factory) is a reference implementation of this pattern (the per-agent builders).

**Related Patterns:**  
[AOS Factory Pattern](#aos-factory-pattern), [Contract Pattern](#contract-pattern) (factory instantiates agents to a shared contract).

**Structure:**  
TBD: Diagram showing factory producing individual agents.

---

## Governance Patterns

Governance Patterns establish the oversight, safety, and alignment mechanisms that constrain how agents behave. They ensure control is in place before and during functional autonomy, bounding autonomous action with approval gates, isolation, validation, and review.

### Governance-First Pattern

**Intent:**  
Ensure that oversight and alignment are established before functional autonomy.

**Classification:**  
Governance Pattern

**Motivation:**  
In complex agentic systems, granting functional autonomy before oversight is established leads to chaotic, unsafe, or misaligned behavior that is hard to contain after the fact. Instantiating security, memory management, and governance agents first ensures that every functional agent operates inside an established control layer from its first action.

**Applicability:**  
Use this pattern when standing up a new agentic system or adding autonomy, and the cost of ungoverned action is high enough that oversight must exist before any functional agent acts.

**Participants:**  
Governance agents (e.g., security, memory) and functional agents.

**Collaborations:**  
Governance agents monitor and restrict functional agents' actions.

**Consequences:**  
Improves safety and alignment but may delay initial functionality.

**Implementation:**  
Start with a base governance layer. Add functional agents only after governance is in place.

**Known Uses:**  
Found in agent frameworks that prioritize security. The [Open AOS Factory](https://github.com/neoClarity-AI/Open-AOS-Factory) is a reference implementation of this pattern (Security, Memory, Chief of Staff, and Review Agents).

**Related Patterns:**  
[Agentic Operating System Pattern](#agentic-operating-system-pattern), [AOS Factory Pattern](#aos-factory-pattern), [Trustworthy Autonomy Pattern](#trustworthy-autonomy-pattern), [Self-Improvement Pattern](#self-improvement-pattern).

**Structure:**  
TBD: A system diagram showing governance agents instantiated first, before functional agents.

---

### Trustworthy Autonomy Pattern

**Intent:**  
Allow autonomy while ensuring trust through approvals.

**Classification:**  
Governance Pattern

**Motivation:**  
Autonomous agents need freedom to act efficiently, but unchecked autonomy risks harmful or irreversible actions taken without human consent. This pattern grants agents broad freedom for routine work while inserting approval checks and human-in-the-loop gates for critical actions, preserving both efficiency and trust.

**Applicability:**  
Use this pattern when agents should act independently on routine work but some actions are irreversible, sensitive, or externally consequential and require human consent.

**Participants:**  
Autonomous agent, user approver.

**Collaborations:**  
Agent acts freely but seeks user permission for key actions.

**Consequences:**  
Balances efficiency with safety. Adds user interaction overhead.

**Implementation:**  
Use approval workflows or human-in-the-loop mechanisms for critical tasks.

**Known Uses:**  
Common in AI tools used in regulated industries. The [Open AOS Factory](https://github.com/neoClarity-AI/Open-AOS-Factory) is a reference implementation of this pattern (enforced by every agent's Approval Requirements, with the Security Agent owning the approval rules and tool access matrix).

**Related Patterns:**  
[Governance-First Pattern](#governance-first-pattern), [Self-Improvement Pattern](#self-improvement-pattern), [Self-Validation Pattern](#self-validation-pattern), [Circuit Breaker Pattern](#circuit-breaker-pattern), [Assistant Pattern](#assistant-pattern), [Collaborator Pattern](#collaborator-pattern), [Administrator Pattern](#administrator-pattern).

**Structure:**  
TBD: Interaction diagram showing agent autonomy, with approval checks for critical actions.

---

### Memory Isolation Pattern

**Intent:**  
Keep data and contexts isolated across environments.

**Classification:**  
Governance Pattern

**Motivation:**  
Users or systems often run multiple agents (work and personal, say) whose data and contexts must never leak into one another, yet a shared memory store makes such leakage almost inevitable. Assigning each agent or instance a distinct, labeled memory zone and enforcing strict separation keeps contexts private and isolated by design.

**Applicability:**  
Use this pattern when one user or system runs multiple agents or instances whose data and contexts must not leak into one another, such as separate work and personal environments.

**Participants:**  
Multiple agents, isolated memory contexts.

**Collaborations:**  
Agents operate independently, referencing only their designated memory.

**Consequences:**  
Ensures privacy but may complicate cross-agent tasks.

**Implementation:**  
Use strict memory separation and labeling.

**Known Uses:**  
Seen in multi-tenant AI environments. The [Open AOS Factory](https://github.com/neoClarity-AI/Open-AOS-Factory) is a reference implementation of this pattern (Memory Agent).

**Related Patterns:**  
[Agentic Operating System Pattern](#agentic-operating-system-pattern), [Router Pattern](#router-pattern), [Self-Improvement Pattern](#self-improvement-pattern), [Loop Pattern](#loop-pattern), [Organizer Pattern](#organizer-pattern).

**Structure:**  
TBD: Diagram showing distinct memory zones per agent or instance.

---

### Self-Improvement Pattern

**Intent:**  
Give the system a built-in mechanism to inspect its own work, surface what is stale, broken, or misaligned, and propose refinements, so the AOS gets better over time without depending on the user to notice problems.

**Classification:**  
Governance Pattern

**Motivation:**  
An agentic system that only ever moves forward accumulates drift: stale projects, contradictory records, decaying memory, workflows that no longer fit the user's goals. Productive agents are each focused on their own domain and have no incentive or vantage point to audit the whole, so left unchecked, quality erodes silently and the user becomes the only backstop. A dedicated reviewing agent counters this by standing outside day-to-day production and running recurring retrospectives and audits on a cadence: it checks the system's outputs for completeness and consistency, detects staleness and misalignment, and proposes improvements through approval-gated changes, turning quality maintenance into a structural property of the system rather than a user chore.

**Applicability:**  
Use this pattern when a system runs continuously over a long horizon and would otherwise accumulate drift (stale projects, contradictory records, decaying memory) with no one but the user to catch it.

**Participants:**  
Review/Reflection agent, the other agents whose outputs it audits, shared system state (memory, logs, projects, manifest), and the user as approver.

**Collaborations:**  
Runs the weekly review ("What needs follow-up soon?"), monthly review ("What is stale, misplaced, or structurally messy?"), and quarterly review ("Is the whole system still aimed at the right goals?"). Audits generated files for completeness and consistency. Reconciles system version and regenerates user-facing projections. Records recurring findings and improvement patterns in its own memory.

**Consequences:**  
The system compounds quality over time and catches drift early. The cost is added review overhead and a strict boundary: the reviewer must propose rather than perform, and must not silently rewrite the definitions it reviews.

**Implementation:**  
Running reviews, audits, and producing findings is autonomous. Any change that alters files is approval-gated (propose, don't act). The reviewer operates as a *projection and refinement* role: it regenerates data files and projections but does not modify framework-derived definition files (the drift invariant). Persist recurring findings and improvement patterns in agent memory so reviews get sharper over time.

**Known Uses:**  
Retrospective and quality-review agents. The [Open AOS Factory](https://github.com/neoClarity-AI/Open-AOS-Factory) is a reference implementation of this pattern: the Review Agent in the AOS, which owns completeness/consistency audits, the weekly/monthly/quarterly reviews, AOS User Guide regeneration, and `aos_version` reconciliation. 

**Related Patterns:**  
[Governance-First Pattern](#governance-first-pattern), [Trustworthy Autonomy Pattern](#trustworthy-autonomy-pattern), [Self-Validation Pattern](#self-validation-pattern), [Memory Isolation Pattern](#memory-isolation-pattern).

**Structure:**  
TBD: Diagram showing a Review agent observing the outputs of other agents and the shared system state, running cadenced reviews (weekly / monthly / quarterly), and feeding proposed refinements back into the system through approval gates.

---

### Self-Validation Pattern

**Intent:**  
Have an agent verify its own output by spawning a subagent with an explicit goal to test the result before it is accepted.

**Classification:**  
Governance Pattern

**Motivation:**  
An agent that produces and accepts its own output unchecked can ship plausible-but-wrong results, and unattended loops amplify this because no human reviews each iteration. Before output is accepted, the system spins up an independent validating subagent given a precise goal (run the test, check the criteria) so work is confirmed against evidence rather than assumed correct.

**Applicability:**  
Use this pattern when an agent's output is acted on without a human reviewing each result, and shipping a plausible-but-wrong answer would be costly enough to justify an independent check.

**Participants:**  
Producing agent, validating subagent, success/validation criteria.

**Collaborations:**  
Implemented via the Subagent Pattern (the validator is a spawned child) and typically the Goal variant of the Loop Pattern (it loops until validation completes). Complements the Circuit Breaker Pattern: validation that never converges should trip a guard.

**Consequences:**  
Raises output quality and grounds acceptance in evidence, suiting unattended autonomy. But it adds cost and latency, and weak criteria give false confidence while burning tokens.

**Implementation:**  
Define concrete, checkable success criteria (run a smoke test, run the suite against a base branch). Give the validating subagent a specific goal, not a vague "check this." Keep the validator independent of the producer.

**Known Uses:**  
Spawning a goal-driven subagent to validate a generated skill against the base branch before keeping it.

**Related Patterns:**  
[Self-Improvement Pattern](#self-improvement-pattern), [Subagent Pattern](#subagent-pattern), [Loop Pattern](#loop-pattern) (Goal variant), [Circuit Breaker Pattern](#circuit-breaker-pattern), [Trustworthy Autonomy Pattern](#trustworthy-autonomy-pattern).

**Structure:**  
TBD: Diagram showing a producing agent's output handed to a validating subagent (with a goal), returning pass/fail that gates acceptance or rework.

---

### Contract Pattern

**Intent:**  
Bind an agent's task to an explicit, verifiable specification (typed inputs and outputs plus pre- and post-conditions) and have the agent self-correct against that specification, so probabilistic output is made to meet stated guarantees before it is accepted, with a type-valid fallback when it cannot.

**Classification:**  
Governance Pattern

**Motivation:**  
LLM-backed agents produce output that is plausible but not guaranteed: it can be the wrong shape, miss required fields, or be syntactically valid yet semantically wrong. Testing after the fact catches some of this, but only once bad output has already propagated. The Contract Pattern moves correctness into the design. The agent declares up front what valid input and output mean: a typed schema, pre-conditions on what it will accept, and post-conditions on what it must return. At run time the agent checks input against the pre-conditions, generates output, then checks that output against the post-conditions. When a check fails, the failure message is fed back as a corrective prompt so the agent can repair its own result, turning a validation failure into a self-correction step rather than an error. If repair does not converge, the contract still returns a type-valid fallback instead of propagating a broken result. Borrowed from Design by Contract, the obligations are explicit and enforced at the boundary, so a component's guarantees hold regardless of which model or logic produced the result.

**Applicability:**  
Use this pattern when an agent's output is consumed by something that needs guarantees (downstream code, another agent, or a user acting on it) and a malformed or plausible-but-wrong result would be costly. It fits especially when output must satisfy both a schema and semantic rules, when self-correction is preferable to a hard failure, and when components built on different models or rule-based systems must interoperate on agreed obligations.

**Participants:**  
The contracted agent, the input and output specifications (typed schema plus pre- and post-conditions), the fixed task prompt, the remediation mechanism (bounded retries driven by corrective prompts), and the fallback result.

**Collaborations:**  
Pre-conditions validate input before work begins. The agent generates output under a fixed task prompt. Post-conditions validate that output. Failed conditions emit descriptive messages that drive remediation retries. Complements the Self-Validation Pattern, which spawns a separate validating subagent, whereas a contract checks inline on the agent's own boundary. Pairs with the Circuit Breaker Pattern, which bounds the remediation loop so repair cannot run away. Clear contracts at interfaces also let agents compose reliably under the Chief of Staff and Router patterns. An Agent Factory can instantiate agents that implement the same contract, so agents are swapped or compared by contract satisfaction rather than by internal prompt design. An Agentic Operating System can adopt contracts as the standard interface between agents, workflows, tools, memory, permissions, and user-facing outputs.

**Consequences:**  
Raises reliability and makes a component's guarantees explicit and auditable at the boundary, and output is checked for substance rather than just shape, while the fallback keeps failures contained and type-safe. The costs are that meaningful pre- and post-conditions and field descriptions are real design work (weak checks give false confidence, and validating only shape lets dummy values through), and that remediation retries add latency and token cost, so the loop needs bounding.

**Implementation:**  
Define input and output as a typed schema with rich field descriptions. Write pre-conditions that raise descriptive errors on bad input and post-conditions that raise descriptive errors on output that is the wrong shape or semantically inadequate, since those messages are what guide self-correction. Keep the task prompt fixed and push per-call variation into the input rather than the prompt. Decide what to remediate (input, output, or both) and bound the retries with a maximum count, backoff, and a stop. Always provide a type-valid fallback for when the contract cannot be satisfied, and decide whether to return it or raise. Optionally accumulate prior error messages across retries so the agent can see what it already tried.

**Known Uses:**  
Design-by-Contract validation layers wrapped around LLM calls. SymbolicAI's `@contract` decorator (ExtensityAI/symbolicai) is a reference implementation: a contracted `Expression` declares a fixed `prompt`, `pre` pre-conditions, an optional `act` step, and `post` post-conditions over Pydantic-based data models, with `pre_remedy` and `post_remedy` driving LLM self-correction on failure, bounded retries, and a guaranteed type-valid fallback returned from `forward`.

**Related Patterns:**  
[Self-Validation Pattern](#self-validation-pattern) (distinct: a separate validating subagent versus inline boundary checks on the agent's own output), [Circuit Breaker Pattern](#circuit-breaker-pattern) (bounds the remediation loop), [Trustworthy Autonomy Pattern](#trustworthy-autonomy-pattern) (human approval gates versus automatic semantic checks), [Self-Improvement Pattern](#self-improvement-pattern), [Loop Pattern](#loop-pattern) (Goal variant), [Agent Factory Pattern](#agent-factory-pattern) (instantiates agents to a shared contract), [Agentic Operating System Pattern](#agentic-operating-system-pattern) (contracts as standard inter-component interfaces).

**Structure:**

```text
                input
                  |
                  v
        +-------------------+   fail + message
        |  Pre-conditions   |-------------------+
        +-------------------+                   |
                  | pass                        v
                  v                     +----------------+
        [ Agent generates output ]<---- |  Remediation   |
        [ under the fixed prompt  ]     |  (retry with   |
                  |                     |  corrective    |
                  v                     |  prompt)       |
        +-------------------+  fail +   +----------------+
        | Post-conditions   |  message          ^
        +-------------------+-------------------+
                  | pass         (retries bounded; on
                  v              exhaustion ->)
           accepted result  --------> type-valid fallback
```

---

### Circuit Breaker Pattern

**Intent:**  
Protect an autonomous system from overload by tripping a safety mechanism that halts or backs off a loop when a monitored signal crosses a threshold.

**Classification:**  
Governance Pattern

**Motivation:**  
An unattended loop can overload the system (hammering a failing dependency or burning tokens indefinitely on thin success criteria), doing harm precisely because no human is watching. The pattern guards against this by monitoring a signal (failures or resource spend), comparing it to a threshold, and tripping when exceeded, stopping the loop before damage compounds, just as an electrical breaker trips to protect a circuit from overheating.

**Applicability:**  
Use this pattern when an agent runs unattended against a fallible dependency or an open-ended budget, and a runaway loop could cause cost, failure, or harm before a human intervenes.

**Variants:**

- *Fault Breaker*: trips on repeated failures or errors. Follows the classic closed → open → half-open cycle, periodically probing whether the dependency has recovered.
- *Cost Guardrail*: trips on resource, iteration, or budget spend, or on non-convergence. Halts and escalates to the human rather than retrying.

**Participants:**  
Monitor, the guarded loop/operation, the threshold or budget, the escalation or recovery target.

**Collaborations:**  
Wraps the Loop Pattern (including Subagent and Self-Validation loops). Complements Trustworthy Autonomy, which gates critical actions on human approval, whereas the breaker bounds resource and failure runaway.

**Consequences:**  
Prevents runaway cost and cascading failure, making unattended autonomy safe to deploy. But thresholds need tuning (too tight kills useful work, too loose defeats the purpose) and monitoring adds overhead.

**Implementation:**  
Set explicit limits (max iterations, token/time budget, failure count). Choose the trip response per variant: the Fault Breaker retries via a half-open probe. The Cost Guardrail halts and surfaces to the human. Monitor loops for both cost and efficiency.

**Known Uses:**  
Stopping retries against a failing tool or MCP; capping token spend on a goal loop with thin validation criteria.

**Related Patterns:**  
[Loop Pattern](#loop-pattern), [Trustworthy Autonomy Pattern](#trustworthy-autonomy-pattern), [Self-Validation Pattern](#self-validation-pattern).

**Structure:**  
TBD: Diagram showing a monitor reading a signal → threshold check → trip → stop action (halt-and-escalate, or open-and-recover).

---

## Capability Patterns

Capability Patterns describe reusable mechanisms that give an agentic system specific powers: routing and coordinating work, looping autonomously, and delegating to subagents. They operate above the system's structure and governance to enable concrete behavior.

### Chief of Staff Pattern

**Intent:**  
Coordinate and align the actions of multiple agents.

**Classification:**  
Capability Pattern

**Motivation:**  
In multi-agent systems, agents pursuing tasks independently can drift from user priorities, conflict with one another, and leave no single point accountable for alignment. A central Chief of Staff agent orchestrates tasks, resolves conflicts, and mediates between the user and functional agents, keeping work aligned with the user's goals.

**Applicability:**  
Use this pattern when multiple agents act in parallel and need a single point of orchestration to align them with user priorities and resolve conflicts.

**Participants:**  
Chief of Staff agent, functional agents, user.

**Collaborations:**  
Chief of Staff orchestrates tasks and resolves conflicts.

**Consequences:**  
Increases system consistency but may add a bottleneck.

**Implementation:**  
Design a mediator agent that oversees workflows.

**Known Uses:**  
Seen in multi-agent virtual assistant systems. The [Open AOS Factory](https://github.com/neoClarity-AI/Open-AOS-Factory) is a reference implementation of this pattern (Chief of Staff Agent).

**Related Patterns:**  
[Agentic Operating System Pattern](#agentic-operating-system-pattern), [Router Pattern](#router-pattern), [Subagent Pattern](#subagent-pattern), [Assistant Pattern](#assistant-pattern).

**Structure:**  
TBD: Diagram showing a Chief of Staff agent mediating between other agents and the user.

---

### Router Pattern

**Intent:**  
Dynamically route requests or tasks between agents or instances.

**Classification:**  
Capability Pattern

**Motivation:**  
In complex systems with many agents or instances, statically assigning requests is inefficient and brittle: work lands on the wrong handler or bottlenecks form. A router instead evaluates each request's context and dynamically directs it to the most appropriate agent or instance, improving efficiency and adaptability.

**Applicability:**  
Use this pattern when many agents or instances exist and requests must be directed dynamically by context, because static assignment is inefficient or brittle.

**Participants:**  
Router, multiple agents or instances.

**Collaborations:**  
Router evaluates context and directs tasks to appropriate agents.

**Consequences:**  
Improves adaptability but can add complexity.

**Implementation:**  
Use rule-based or learning-based routing.

**Known Uses:**  
Present in distributed AI platforms. The [Open AOS Factory](https://github.com/neoClarity-AI/Open-AOS-Factory) is a reference implementation of this pattern (Chief of Staff Agent, joint owner of the AOS Workspace router).

**Related Patterns:**  
[Chief of Staff Pattern](#chief-of-staff-pattern), [Memory Isolation Pattern](#memory-isolation-pattern), [Subagent Pattern](#subagent-pattern).

**Structure:**  
TBD: Diagram showing routing logic directing tasks.

---

### Loop Pattern

**Intent:**  
Enable an agent to prompt itself through automation (running on a trigger and continuing until an exit condition) rather than waiting for human messages.

**Classification:**  
Capability Pattern

**Motivation:**  
Turn-based, human-typed prompting caps an agent's usefulness at the user's availability, so recurring or long-running work stalls whenever the human isn't there to send the next message. Wiring the agent to an automated trigger lets it prompt itself, perform the work, and stop when the job is done or time is up, turning a one-off prompt into a standing, self-driving routine.

**Applicability:**  
Use this pattern when work is recurring or long-running and shouldn't wait on a human to send each prompt: for polling, scheduled routines, event reactions, or goal-driven runs.

**Variants (trigger types):**

- *Heartbeat*: fires on a fixed interval (every 5 minutes, every hour); suited to polling.
- *Cron*: fires on a defined schedule or time (9am, every Friday night); suited to periodic routines.
- *Hook*: fires on an event: an internal lifecycle event (tool called, session started) or an external webhook (email received).
- *Goal*: runs against a measurable outcome until it is validated or the agent is blocked. The exit condition is success criteria, not a clock.

**Participants:**  
Trigger/scheduler, looping agent, exit condition, optional collaborators (state store, isolation sandbox).

**Collaborations:**  
Persists progress via *Externalized State* (a markdown to-do list or task tracker) so it survives across runs. Runs inside *Execution Isolation* (a sandbox or work tree) so concurrent agents don't collide. Often drives the Subagent Pattern and is bounded by the Circuit Breaker Pattern.

**Consequences:**  
Provides unattended, recurring autonomy and frees the human from manual triggering. Goal loops in particular can burn tokens and require precise exit criteria plus monitoring.

**Implementation:**  
Use the platform's scheduling/automation primitives (scheduled tasks, routines, automations). Design by the "onboarding an employee" model: specify the job, cadence, and done-criteria as if briefing a new hire. For goal loops, write success criteria precisely. Consider having the agent draft its own goal.

**Known Uses:**  
Morning-briefing scheduled tasks; a daily aging-PR reviewer; a Gmail-inbox cleanup goal loop; a weekly skills-identification automation.

**Related Patterns:**  
[Subagent Pattern](#subagent-pattern), [Circuit Breaker Pattern](#circuit-breaker-pattern), [Self-Validation Pattern](#self-validation-pattern), [Memory Isolation Pattern](#memory-isolation-pattern).

**Structure:**  
TBD: Diagram showing trigger → agent self-prompt → work → exit check, looping until the exit condition is met.

---

### Subagent Pattern

**Intent:**  
Federate a bounded subtask to a spawned, ephemeral child agent (especially for validation), keeping the main thread focused and clean.

**Classification:**  
Capability Pattern

**Motivation:**  
A single agent thread that tries to do everything accumulates context, mixes concerns, and becomes hard to keep reliable, particularly when the work includes independent checks such as validation. Spawning a dedicated child agent for a specific subtask, letting it complete in its own context, and absorbing the result distributes the work without polluting the parent thread. Subagents can themselves spawn subagents, enabling recursive teams.

**Applicability:**  
Use this pattern when a bounded subtask (especially validation) should run in its own context, so the main thread stays focused and concerns don't intermingle.

**Participants:**  
Parent agent, one or more spawned subagents, the scoped subtask.

**Collaborations:**  
Frequently invoked from within a Loop. Pairs with Self-Validation when the subtask is to verify the parent's output. Distinct from Router and Chief of Staff, which direct work among standing agents rather than spawning new ephemeral ones.

**Consequences:**  
Enables parallelism, separation of concerns, and a cleaner main thread, and recursion lets it scale. But more agents mean more cost and coordination, and each needs its own tools and permissions.

**Implementation:**  
Use the platform's subagent/thread-spawning mechanism. Give each subagent a tight scope and only the tools it needs. For validation subagents, hand them a goal (see the Goal variant of the Loop Pattern).

**Known Uses:**  
Spawning a thread to babysit a PR until its merge checks are green; spawning a per-skill subagent to validate each newly identified skill.

**Related Patterns:**  
[Loop Pattern](#loop-pattern), [Self-Validation Pattern](#self-validation-pattern), [Chief of Staff Pattern](#chief-of-staff-pattern), [Router Pattern](#router-pattern).

**Structure:**  
TBD: Diagram showing a parent agent spawning one or more subagents that perform scoped work and return results.

---

## Productivity Patterns

Productivity Patterns describe specialized productive agents that perform useful work for the user within an Agentic Operating System. Unlike architecture and governance patterns, which establish the system's structure and safety, these patterns capture recurring *roles* an agent can take on. Each is a specialized worker rather than a coordinator: it owns one domain, defers orchestration to the Chief of Staff, and acts within the system's governance and approval boundaries.

### Assistant Pattern

**Intent:**  
Keep the user's day-to-day flow of commitments, time, and communications captured, organized, and surfaced at the right moment.

**Classification:**  
Productivity Pattern

**Motivation:**  
A user's attention is constantly pulled across tasks, appointments, and incoming messages, and commitments get dropped when tracking is manual and continuous. A dedicated agent reliably captures every commitment, holds the shape of the user's calendar, and triages incoming items, so the user can spend their attention on judgment and action rather than tracking.

**Applicability:**  
Use this pattern when a user's commitments, calendar, and incoming messages need continuous capture and triage so nothing is dropped and attention is freed for judgment.

**Participants:**  
Assistant agent, user, Chief of Staff, downstream agents (e.g., Project Manager).

**Collaborations:**  
Captures and prioritizes tasks, proposes schedules and focus blocks, triages messages and promotes items to tasks or other containers. Surfaces today's and at-risk items in the daily brief.

**Consequences:**  
Reduces dropped commitments and context-switching cost. Depends on consistent capture and on approval gates for sending messages or changing shared calendars.

**Implementation:**  
Maintain structured task, calendar, and inbox state in agent memory. Drafting and proposing are autonomous, while sending messages and modifying shared calendars require explicit approval.

**Known Uses:**  
Executive-assistant and personal-productivity agents. The [Open AOS Factory](https://github.com/neoClarity-AI/Open-AOS-Factory) is a reference implementation of this pattern (Task/Commitment, Calendar/Scheduling, and Inbox/Communications Agents).

**Related Patterns:**  
[Chief of Staff Pattern](#chief-of-staff-pattern), [Trustworthy Autonomy Pattern](#trustworthy-autonomy-pattern), [Administrator Pattern](#administrator-pattern).

**Structure:**  
TBD: Diagram showing an Assistant agent maintaining task, calendar, and inbox state, feeding items into the daily operating rhythm.

---

### Researcher Pattern

**Intent:**  
Gather, evaluate, and synthesize information into useful, cited outputs.

**Classification:**  
Productivity Pattern

**Motivation:**  
Reliable answers often require collecting and weighing many sources, work that is time-consuming and easy to do shallowly. A dedicated agent does this gathering and synthesis at depth and speed, returning briefs with traceable sources and explicit confidence so the user can act on well-grounded conclusions.

**Applicability:**  
Use this pattern when a decision needs information gathered and weighed across multiple sources, and traceable citations plus explicit confidence matter more than a fast single-source answer.

**Participants:**  
Research agent, information sources, user, requesting agents.

**Collaborations:**  
Receives questions from the user or other agents. Searches approved sources. Returns synthesized briefs with citations and confidence notes.

**Consequences:**  
Accelerates well-grounded decisions. Quality depends on source access and on honest representation of uncertainty.

**Implementation:**  
Use approved search and retrieval tools per the access matrix. Always cite sources and flag confidence. Store durable findings in memory.

**Known Uses:**  
Research and analyst agents. The [Open AOS Factory](https://github.com/neoClarity-AI/Open-AOS-Factory) is a reference implementation of this pattern (Research Agent).

**Related Patterns:**  
[Organizer Pattern](#organizer-pattern), [Collaborator Pattern](#collaborator-pattern), [Instructor Pattern](#instructor-pattern).

**Structure:**  
TBD: Diagram showing a Research agent drawing from multiple sources and producing a synthesized, cited brief.

---

### Collaborator Pattern

**Intent:**  
Co-produce work product with the user by drafting and refining content through iteration.

**Classification:**  
Productivity Pattern

**Motivation:**  
Much knowledge work is created through drafting and revision rather than in a single pass, and moving from a blank page alone is slow. A dedicated agent acts as a creative partner (producing drafts, absorbing feedback, and learning the user's voice over time) so the user reaches a finished piece faster while keeping editorial control.

**Applicability:**  
Use this pattern when work product is created through drafting and revision, and the user wants a partner to move from blank page to finished piece while keeping editorial control.

**Participants:**  
Collaborator agent, user, Research agent (for inputs).

**Collaborations:**  
Drafts and edits content, incorporates user feedback across iterations, captures durable voice and style preferences in memory.

**Consequences:**  
Speeds content creation and preserves a consistent voice. Publishing or sending the output requires explicit approval to keep the user in control.

**Implementation:**  
Drafting and editing are autonomous. Publishing or sending is approval-required. Persist voice and style preferences in memory for reuse.

**Known Uses:**  
Writing and content-generation agents. The [Open AOS Factory](https://github.com/neoClarity-AI/Open-AOS-Factory) is a reference implementation of this pattern (Writing/Content Agent).

**Related Patterns:**  
[Researcher Pattern](#researcher-pattern), [Trustworthy Autonomy Pattern](#trustworthy-autonomy-pattern), [Instructor Pattern](#instructor-pattern).

**Structure:**  
TBD: Interaction diagram showing iterative draft–feedback–refine cycles between the user and a Collaborator agent.

---

### Organizer Pattern

**Intent:**  
Structure, catalog, and retrieve the user's information and assets.

**Classification:**  
Productivity Pattern

**Motivation:**  
As a user's documents and information accumulate, their value depends entirely on being findable, and findability decays without upkeep. A dedicated agent imposes and maintains order (building a catalog and consistent naming) so anything can be retrieved on demand without the user remembering where it lives.

**Applicability:**  
Use this pattern when a growing body of documents and assets is losing its value to poor findability, and consistent cataloging and naming would make anything retrievable on demand.

**Participants:**  
Organizer agent, user, other agents that produce or consume files.

**Collaborations:**  
Builds and maintains a catalog/index, proposes consistent naming and filing, retrieves items on request.

**Consequences:**  
Makes information reliably retrievable. Moving, renaming, or deleting files is consequential and must be proposed rather than performed autonomously.

**Implementation:**  
Creating index and catalog files is autonomous. Moving, renaming, archiving, overwriting, or deleting files is approval-required: propose, do not act.

**Known Uses:**  
Document-management and knowledge-base agents. The [Open AOS Factory](https://github.com/neoClarity-AI/Open-AOS-Factory) is a reference implementation of this pattern (Document Librarian Agent).

**Related Patterns:**  
[Researcher Pattern](#researcher-pattern), [Memory Isolation Pattern](#memory-isolation-pattern).

**Structure:**  
TBD: Diagram showing an Organizer agent maintaining an index over a body of documents and assets.

---

### Administrator Pattern

**Intent:**  
Handle tracking, records, and administrative logistics on the user's behalf.

**Classification:**  
Productivity Pattern

**Motivation:**  
Routine administrative work (tracking figures, maintaining records, drafting paperwork) is necessary but repetitive, and easy to let slip. A dedicated agent carries this ongoing burden accurately and on a cadence, summarizing status and flagging what needs attention, while the user retains control over any action that commits money or has external consequences.

**Applicability:**  
Use this pattern when routine records, tracking, and paperwork need accurate upkeep on a cadence, while any spending or external commitment stays under explicit user control.

**Participants:**  
Administrator agent, user, Assistant agent (for related deadlines).

**Collaborations:**  
Tracks and summarizes records, drafts administrative documents, surfaces items needing attention in operating rhythms.

**Consequences:**  
Keeps administrative state current with low effort. Sensitive details and any spending or external commitment require explicit approval.

**Implementation:**  
Tracking, summarizing, and drafting are autonomous. Spending money or committing externally is always approval-required. Storage of sensitive specifics requires explicit approval.

**Known Uses:**  
Finance and administrative-operations agents. The [Open AOS Factory](https://github.com/neoClarity-AI/Open-AOS-Factory) is a reference implementation of this pattern (Finance/Admin Agent).

**Related Patterns:**  
[Trustworthy Autonomy Pattern](#trustworthy-autonomy-pattern), [Assistant Pattern](#assistant-pattern).

**Structure:**  
TBD: Diagram showing an Administrator agent maintaining records and producing periodic summaries.

---

### Instructor Pattern

**Intent:**  
Build the user's understanding through explanation, practice, and progress tracking.

**Classification:**  
Productivity Pattern

**Motivation:**  
Learning is most effective when it is personalized, paced, and revisited over time, yet generic material adapts to no one. A dedicated agent acts as a patient tutor (explaining concepts in plain language, generating practice, and tracking what the user has and hasn't mastered), adapting to the individual learner.

**Applicability:**  
Use this pattern when a user is learning a topic over time and would benefit from personalized explanation, generated practice, and honest progress tracking that generic material can't provide.

**Participants:**  
Instructor agent, user, Research agent (for source material).

**Collaborations:**  
Produces study plans, plain-language explanations, and practice questions. Tracks progress and surfaces stale or overdue topics during reviews.

**Consequences:**  
Supports durable, personalized learning. Effectiveness depends on honest progress tracking and the user's engagement with practice.

**Implementation:**  
Generate explanations, plans, and practice autonomously. Track progress in agent memory. Surface overdue topics through operating rhythms.

**Known Uses:**  
Tutoring and learning-companion agents. The [Open AOS Factory](https://github.com/neoClarity-AI/Open-AOS-Factory) is a reference implementation of this pattern (Learning/Tutor Agent).

**Related Patterns:**  
[Researcher Pattern](#researcher-pattern), [Collaborator Pattern](#collaborator-pattern).

**Structure:**  
TBD: Diagram showing an Instructor agent cycling between explanation, practice, and progress tracking with the user.

---

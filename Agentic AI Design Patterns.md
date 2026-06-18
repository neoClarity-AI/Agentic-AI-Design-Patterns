# Agentic AI Design Patterns

## Table of Contents

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

## How to Read These Patterns

Each pattern is documented with a template adapted from the "Gang of Four" [Design Patterns](https://en.wikipedia.org/wiki/Design_Patterns) book, in this field order: Intent; Classification; Motivation (a brief narrative scenario); Applicability; Structure; Participants; Collaborations; Consequences; Implementation; Known Uses; Related Patterns.

This template deviates from the canonical GoF format in a few deliberate ways. *Also Known As* and *Sample Code* are omitted, because these patterns are conceptual rather than code-level. Motivation is written as a short narrative that states the problem and how the pattern resolves it, in keeping with the original GoF scenario style.

Two optional fields appear only where useful. *Variants* lists the sub-types of a pattern and is an intentional extension to the GoF template (used by the Loop and Circuit Breaker patterns); it appears after Applicability. *Reference Implementation* names the concrete agent or agents that realize a Productivity pattern within an Agentic Operating System; it appears just before Known Uses.

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
Specialized agents operate within the shared governance, memory, and workflow layers; coordination protocols and escalation paths route work between them, while governance constrains every action. 

**Consequences:**  
Gives complex objectives a coherent home and reduces duplicated work, lost context, and ungoverned action; the cost is the overhead of building and maintaining the shared layers, plus a risk of bottlenecks at the coordination layer. 

**Implementation:**  
Establish the operating system as shared governance mechanisms, structured memory systems, standardized workflows, coordination protocols, and defined permissions and escalation paths; then add specialized agents with clearly defined responsibilities on top of that foundation. 

**Known Uses:**  
The Agentic Operating System assembled by the AOS Factory, comprising governance agents (Chief of Staff, Memory, Security, Review) and optional productive agents.

**Related Patterns:**  
[Governance-First Pattern](#governance-first-pattern), [AOS Factory Pattern](#aos-factory-pattern), [Chief of Staff Pattern](#chief-of-staff-pattern), [Memory Isolation Pattern](#memory-isolation-pattern).

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
Ensures consistency; may reduce flexibility in one-off agents.

**Implementation:**  
Standardize agent templates and system assembly processes.

**Known Uses:**  
Frameworks for deploying AI ecosystems.

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
Not all problems justify a whole operating system; standing up an entire AOS for a single, narrow need is wasteful, yet hand-building one-off agents is slow and inconsistent. A factory resolves this tension by producing individual agents from role-specific templates and configurations, delivering a tailored, specialized agent quickly while keeping a consistent baseline.

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
Agent builders for single-purpose bots.

**Related Patterns:**  
[AOS Factory Pattern](#aos-factory-pattern).

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
Start with a base governance layer; add functional agents only after governance is in place.

**Known Uses:**  
Found in agent frameworks that prioritize security.

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
Balances efficiency with safety; adds user interaction overhead.

**Implementation:**  
Use approval workflows or human-in-the-loop mechanisms for critical tasks.

**Known Uses:**  
Common in AI tools used in regulated industries.

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
Seen in multi-tenant AI environments.

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
Runs the weekly review ("What needs follow-up soon?"), monthly review ("What is stale, misplaced, or structurally messy?"), and quarterly review ("Is the whole system still aimed at the right goals?"); audits generated files for completeness and consistency; reconciles system version and regenerates user-facing projections; records recurring findings and improvement patterns in its own memory.

**Consequences:**  
The system compounds quality over time and catches drift early; the cost is added review overhead and a strict boundary: the reviewer must propose rather than perform, and must not silently rewrite the definitions it reviews.

**Implementation:**  
Running reviews, audits, and producing findings is autonomous; any change that alters files is approval-gated (propose, don't act). The reviewer operates as a *projection and refinement* role: it regenerates data files and projections but does not modify framework-derived definition files (the drift invariant). Persist recurring findings and improvement patterns in agent memory so reviews get sharper over time.

**Known Uses:**  
Retrospective and quality-review agents; the Review/Reflection Agent in the AOS, which owns completeness/consistency audits, the weekly/monthly/quarterly reviews, AOS User Guide regeneration, and `aos_version` reconciliation.

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
Raises output quality and grounds acceptance in evidence, suiting unattended autonomy; but adds cost and latency, and weak criteria give false confidence while burning tokens.

**Implementation:**  
Define concrete, checkable success criteria (run a smoke test, run the suite against a base branch). Give the validating subagent a specific goal, not a vague "check this." Keep the validator independent of the producer.

**Known Uses:**  
Spawning a goal-driven subagent to validate a generated skill against the base branch before keeping it.

**Related Patterns:**  
[Self-Improvement Pattern](#self-improvement-pattern), [Subagent Pattern](#subagent-pattern), [Loop Pattern](#loop-pattern) (Goal variant), [Circuit Breaker Pattern](#circuit-breaker-pattern), [Trustworthy Autonomy Pattern](#trustworthy-autonomy-pattern).

**Structure:**  
TBD: Diagram showing a producing agent's output handed to a validating subagent (with a goal), returning pass/fail that gates acceptance or rework.

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

- *Fault Breaker*: trips on repeated failures or errors; follows the classic closed → open → half-open cycle, periodically probing whether the dependency has recovered.
- *Cost Guardrail*: trips on resource, iteration, or budget spend, or on non-convergence; halts and escalates to the human rather than retrying.

**Participants:**  
Monitor, the guarded loop/operation, the threshold or budget, the escalation or recovery target.

**Collaborations:**  
Wraps the Loop Pattern (including Subagent and Self-Validation loops). Complements Trustworthy Autonomy, which gates critical actions on human approval, whereas the breaker bounds resource and failure runaway.

**Consequences:**  
Prevents runaway cost and cascading failure, making unattended autonomy safe to deploy; but thresholds need tuning (too tight kills useful work, too loose defeats the purpose) and monitoring adds overhead.

**Implementation:**  
Set explicit limits (max iterations, token/time budget, failure count). Choose the trip response per variant: the Fault Breaker retries via a half-open probe; the Cost Guardrail halts and surfaces to the human. Monitor loops for both cost and efficiency.

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
Seen in multi-agent virtual assistant systems.

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
Present in distributed AI platforms.

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
- *Goal*: runs against a measurable outcome until it is validated or the agent is blocked; the exit condition is success criteria, not a clock.

**Participants:**  
Trigger/scheduler, looping agent, exit condition, optional collaborators (state store, isolation sandbox).

**Collaborations:**  
Persists progress via *Externalized State* (a markdown to-do list or task tracker) so it survives across runs; runs inside *Execution Isolation* (a sandbox or work tree) so concurrent agents don't collide; often drives the Subagent Pattern and is bounded by the Circuit Breaker Pattern.

**Consequences:**  
Provides unattended, recurring autonomy and frees the human from manual triggering; goal loops in particular can burn tokens and require precise exit criteria plus monitoring.

**Implementation:**  
Use the platform's scheduling/automation primitives (scheduled tasks, routines, automations). Design by the "onboarding an employee" model: specify the job, cadence, and done-criteria as if briefing a new hire. For goal loops, write success criteria precisely; consider having the agent draft its own goal.

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
A single agent thread that tries to do everything accumulates context, mixes concerns, and becomes hard to keep reliable, particularly when the work includes independent checks such as validation. Spawning a dedicated child agent for a specific subtask, letting it complete in its own context, and absorbing the result distributes the work without polluting the parent thread; subagents can themselves spawn subagents, enabling recursive teams.

**Applicability:**  
Use this pattern when a bounded subtask (especially validation) should run in its own context, so the main thread stays focused and concerns don't intermingle.

**Participants:**  
Parent agent, one or more spawned subagents, the scoped subtask.

**Collaborations:**  
Frequently invoked from within a Loop; pairs with Self-Validation when the subtask is to verify the parent's output. Distinct from Router and Chief of Staff, which direct work among standing agents rather than spawning new ephemeral ones.

**Consequences:**  
Enables parallelism, separation of concerns, and a cleaner main thread, and recursion lets it scale; but more agents mean more cost and coordination, and each needs its own tools and permissions.

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
Captures and prioritizes tasks, proposes schedules and focus blocks, triages messages and promotes items to tasks or other containers; surfaces today's and at-risk items in the daily brief.

**Consequences:**  
Reduces dropped commitments and context-switching cost; depends on consistent capture and on approval gates for sending messages or changing shared calendars.

**Implementation:**  
Maintain structured task, calendar, and inbox state in agent memory; drafting and proposing are autonomous, while sending messages and modifying shared calendars require explicit approval.

**Reference Implementation:**  
Task/Commitment, Calendar/Scheduling, and Inbox/Communications Agents.

**Known Uses:**  
Executive-assistant and personal-productivity agents.

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
Receives questions from the user or other agents; searches approved sources; returns synthesized briefs with citations and confidence notes.

**Consequences:**  
Accelerates well-grounded decisions; quality depends on source access and on honest representation of uncertainty.

**Implementation:**  
Use approved search and retrieval tools per the access matrix; always cite sources and flag confidence; store durable findings in memory.

**Reference Implementation:**  
Research Agent.

**Known Uses:**  
Research and analyst agents.

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
Speeds content creation and preserves a consistent voice; publishing or sending the output requires explicit approval to keep the user in control.

**Implementation:**  
Drafting and editing are autonomous; publishing or sending is approval-required; persist voice and style preferences in memory for reuse.

**Reference Implementation:**  
Writing/Content Agent.

**Known Uses:**  
Writing and content-generation agents.

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
Makes information reliably retrievable; moving, renaming, or deleting files is consequential and must be proposed rather than performed autonomously.

**Implementation:**  
Creating index and catalog files is autonomous; moving, renaming, archiving, overwriting, or deleting files is approval-required: propose, do not act.

**Reference Implementation:**  
Document Librarian Agent.

**Known Uses:**  
Document-management and knowledge-base agents.

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
Keeps administrative state current with low effort; sensitive details and any spending or external commitment require explicit approval.

**Implementation:**  
Tracking, summarizing, and drafting are autonomous; spending money or committing externally is always approval-required; storage of sensitive specifics requires explicit approval.

**Reference Implementation:**  
Finance/Admin Agent.

**Known Uses:**  
Finance and administrative-operations agents.

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
Produces study plans, plain-language explanations, and practice questions; tracks progress and surfaces stale or overdue topics during reviews.

**Consequences:**  
Supports durable, personalized learning; effectiveness depends on honest progress tracking and the user's engagement with practice.

**Implementation:**  
Generate explanations, plans, and practice autonomously; track progress in agent memory; surface overdue topics through operating rhythms.

**Reference Implementation:**  
Learning/Tutor Agent.

**Known Uses:**  
Tutoring and learning-companion agents.

**Related Patterns:**  
[Researcher Pattern](#researcher-pattern), [Collaborator Pattern](#collaborator-pattern).

**Structure:**  
TBD: Diagram showing an Instructor agent cycling between explanation, practice, and progress tracking with the user.

---

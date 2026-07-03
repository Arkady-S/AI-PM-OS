Operating model, leverage levels, and strategic alignment

**May 2026** (updated May 31)

## CORE CONCEPT

> An exponential AI strategy cannot succeed through a linear organizational structure. Bolting AI onto unchanged structures produces unpredictable quality and confused teams. Redesign how the org works, not just what it ships.

> Misalignment kills more AI initiatives than bad ideas. Teams building on different assumptions will fail even with smart people and good technology. The goal is 100% alignment, not 100% agreement: commit to testing shared hypotheses together.

## THE LEVERAGE LADDER

Every AI initiative operates at one of three levels. Most orgs are stuck at Level 1. The PM's job: know which level each initiative sits at and plan the climb.

| Level | Type | What AI Does | Impact |
|-------|------|--------------|--------|
| 1 | Task | Removes individual toil | Additive (saves hours) |
| 2 | Workflow | Reshapes entire processes | Multiplicative (collapses cycles) |
| 3 | Capability | Creates org infrastructure all plug into | Exponential (multiplies across org) |

### Level 3 in Practice

At Anthropic (as of May 2026), the majority of code is written by Claude. A single human ran a team of 9 synthetic research agents: the human set initial research directions, agents executed the research. The org restructures every 3-4 months as capability shifts change what's possible. This is Level 3: the AI layer is org infrastructure that every function plugs into, and the org continuously reshapes around it.

### Level 1 vs 2 Examples

| Function | Level 1 (Task) | Level 2 (Workflow) |
|----------|---|---|
| Engineering | AI coding assistant, autocomplete | Auto-triage incidents, route to right team |
| Support | AI drafts responses, human reviews | Auto-resolve known issues, route complex |
| Product | Summarize feedback from tickets | Interview users at scale, synthesize patterns |
| Design | Generate mockup variations | Maintain system consistency, auto-generate |

## CREATOR TO EDITOR SHIFT

| Mindset | Starting Point | Value Source | AI Implication |
|---------|---|---|---|
| Creator | Blank page | Production | AI replaces this; less defensible |
| Editor | Raw material | Judgment | AI amplifies this; more defensible |

In an age of infinite AI generation, value moves from production to judgment. Taste becomes the bottleneck. Teams that generate with AI but lack editorial skill to select and refine will ship average output.

As code volume explodes (Anthropic's internal code output grew dramatically once Claude wrote most of it), humans move to a "verification layer" atop a virtual organization of agents. The editing role becomes: set direction, review output, course-correct. Hiring shifts accordingly toward two profiles: AI-native early-career people who think in human-AI pairs from day one, and experienced people who can imagine entire projects and decompose them for agent execution.

### AI-Native Operating Benchmarks

Concrete ratios from teams operating at Level 3:

| Metric | Benchmark | Source |
|---|---|---|
| Team composition | 50 engineers, 2 PMs, 2 designers across 10-12 products | OpenAI Codex, May 2026 |
| Token spend per person | $4,000-5,000/month on AI tokens | OpenAI Codex team average |
| AI-generated code target | 90-100% of PRs | Henry (Anthropic), May 2026 |
| Experimentation ratio | Ship 2 out of 10 things built; other 8 are experiments | Rohan (OpenAI), May 2026 |
| Full automation stack | AI writes code, AI reviews code, AI QA, AI merges | Pressure-test recommendation |

> If your team's token spend per engineer is under $1,000/month, you're likely underinvesting. The recommendation from practitioners at frontier labs: double or 10x current token budgets.

## THE HANDOFF PROTOCOL

Every AI workflow follows Human > AI > Human across three zones. Most failures trace to skipping Zone 1 or rubber-stamping Zone 3.

| Zone | Owner | Function | Failure If Skipped |
|------|-------|----------|-------------------|
| 1: Setup | Human | Define intent, context, constraints | AI produces without direction |
| 2: Scale | AI | Draft, synthesize, recognize patterns | Human bottleneck at volume |
| 3: Judgment | Human | Apply taste, verify, approve | Errors ship unchecked |

A "trust economy" is forming around Zone 3. When most of a document is AI-generated, teams need systems for indicating how much a human endorses the output. The endorsement signal (not authorship) becomes the trust anchor. Without it, AI-generated artifacts circulate with ambiguous accountability.

## THE SENSING FUNCTION

Replaces static annual planning with continuous calibration. AI costs drop ~50% per quarter; a 12-month roadmap is obsolete before you finish writing it.

| Component | What It Monitors | Key Question |
|-----------|---|---|
| External Pulse | Model releases, cost drops, competitor launches | Can we / afford to / must we act? |
| Internal Pulse | Unexpected usage, outlier segments, quality signals | Where are we ready to climb? |
| Guardrails | Pre-agreed thresholds and action triggers | <2%: expand. >5%: reduce. >10%: revert. |
| Monthly Review | All signals synthesized into decisions | Climb, hold, or pull back? |

## GUARDRAILS ENABLE SPEED

| Traditional Approach | Guardrails Approach |
|---|---|
| Create governance committees | Establish thresholds upfront |
| Require sign-offs for each decision | Define success/failure metrics before starting |
| Add approval layers | Agree on action triggers tied to specific numbers |
| **Result: bottlenecks that kill velocity** | **Result: permission to move fast within boundaries** |

## LADDER CLIMB CRITERIA

Before advancing any initiative to the next level, confirm:

> **Quality:** Stable at current level. Golden set passing, user signals positive.

> **Trust:** Team has built confidence in AI through sufficient operating time.

> **Guardrails:** Defined with specific thresholds and action triggers.

> **Handoff:** Protocol mapped with clear Zone 1, 2, 3 ownership.

> **Rollback:** Path defined and tested. Can revert within defined SLA.

## AI NORTH STAR FRAMEWORK

Four elements create a closed feedback loop from beliefs to daily work. Most teams start with features and back-fill strategy. This inverts it: worldview informs strategy, strategy defines metrics, metrics determine initiatives.

| Element | Contains | Connects To |
|---------|----------|-------------|
| Worldview | Shared beliefs: market, business, product, user | Informs which strategy to pursue |
| AI Product Strategy | Core thesis tied to company North Star Metric | Translates beliefs into testable hypothesis |
| OKRs | Key results derived from KPI graph levers | Decomposes strategy into quarterly targets |
| Roadmap | Sequenced initiatives moving specific KRs | Executes; data feeds back to worldview |

## WORLDVIEW ALIGNMENT

Misalignment hides in implicit beliefs. If the CEO believes costs approach zero and the VP Eng believes they stay significant, they make incompatible decisions without realizing they disagree.

### 1. Prepare: Distribute Assumptions Document

Cover four dimensions: Market (transformational vs incremental?), Business (cost trajectory, moat source?), Product (copilot vs agent?), User (trust evolution?).

### 2. Discuss: 60-90 min Cross-Functional Workshop

Surface disagreements respectfully. Make implicit beliefs explicit. This is where you discover incompatible assumptions.

### 3. Align: Document Shared Hypotheses

One-page worldview doc. Teams don't need to believe assumptions are true. They commit to testing them together. Alignment, not agreement.

## KPI GRAPH CONSTRUCTION

Maps how your AI Product NSM connects to the company NSM through a logical chain of levers. Your product NSM must be a leading indicator your team controls, not the company's lagging metric.

### 1. Identify Company NSM (Lagging)

Ex: Daily Active Users, revenue, Daily Learning Minutes.

### 2. Define AI Product NSM (Leading, You Control)

Ex: Learning Efficacy Index, AI resolution rate, context retrieval accuracy.

### 3. Decompose into Level 1 and 2 Levers

Direct drivers, then actionable sub-drivers. Identify self-reinforcing flywheels.

### 4. Validate Each Connection

Is the causal relationship real or assumed? Flag assumptions for testing.

## OKR DERIVATION

KRs map to specific levers in the KPI graph, not brainstormed targets. If a KR doesn't connect to a lever influencing the product NSM, it's orphaned.

| Check | Question | Red Flag |
|-------|----------|----------|
| Lever link | Which KPI graph lever does this KR target? | "Improves user experience" (no lever) |
| Traceability | Can this KR trace to product NSM? | Needs >2 logical leaps to connect |
| Assumption | Which worldview belief does this test? | Team can't name the assumption |
| Orphan audit | Any existing KRs with no lever link? | KR set by brainstorming, not graph |

## ROADMAP TRACEABILITY

Every roadmap item must trace back through the full chain. If any link is missing, it's AI theater: features that look strategic but connect to no testable thesis.

| Chain Link | Must Answer | Red Flag |
|-----------|---|---|
| Initiative > KR | Which KR does this move? | "Supports our general strategy" |
| KR > Lever | Which KPI graph lever? | "Improves experience" (vague) |
| Lever > Product NSM | How does lever influence NSM? | Causal chain has >2 leaps |
| Product > Company NSM | How does product NSM drive co.? | Relationship assumed, not validated |
| Strategy > Worldview | Which assumption does this test? | Team can't name it |

## AI-NATIVE TRANSFORMATION MODEL

Three layers, all required. Bottom-up capability without top-down pressure produces experimentation theater. Top-down pressure without bottom-up capability produces compliance theater.

| Layer | What It Contains | Failure If Missing |
|-------|-----------------|-------------------|
| 1. Capability (bottom) | People pushed past learning curve quickly; tools become the teacher; experimentation in the flow of work | Teams can't execute on leadership expectations; mandates feel arbitrary |
| 2. Culture (middle) | Rituals (ex: AI Fridays), open workflow sharing, standardizing on a small tool set; creates pull, not push | Adoption stays individual; no compounding across teams |
| 3. Leadership (top) | Aggressive concrete milestones, not vague goals; redefining ownership (ex: "every PM pushes code to production") | No forcing function; teams optimize for comfort, not transformation |

### Role Convergence

As execution cost drops, specialization becomes friction. Organizations converge toward two archetypes: people who build the thing, and people who get the thing adopted. Everyone becomes a builder, not necessarily writing code, but able to prototype, test, ship, and iterate without waiting on another function.

Hiring implication: expect candidates to use AI during interviews. Evaluate how they think with the model (prompt structure, iteration, judgment), not just the answer. Avoiding AI in an interview signals the candidate won't use it on the job.

## COMMON FAILURE MODES

| Failure | Symptom | Prevention |
|---------|---------|-----------|
| Stuck at Level 1 | Every initiative is task automation | Audit levels quarterly; plan climbs |
| Jet engine on bicycle | AI bolted onto unchanged structure | Redesign processes alongside AI |
| Skipping Zone 1 | AI produces without intent/constraints | Mandate Handoff Protocol |
| Rubber-stamp Zone 3 | Humans auto-approve; errors ship | Require active decisions, not passive |
| AI theater | Features trace to no testable thesis | Mandatory traceability: initiative > NSM |
| Orphaned OKRs | KRs set by brainstorm, no lever link | Derive all OKRs from KPI graph |
| Hidden misalignment | Teams on different assumptions | Worldview workshop before planning |
| Static roadmaps | 12-month plan obsolete by Q2 | Sensing Function; monthly calibration |

→ See: Evals & Observability (quality signals for guardrail thresholds)
→ See: Governance & Safety (guardrails framework)

---

**Sources:**
- Jack Clark, Import AI 458 (May 2026)
- Noam Lovinsky, CPO Superhuman; formerly CPO Grammarly
- Rohan (OpenAI Codex PM), Henry (Anthropic), Product Faculty AI PM Course (May 2026)

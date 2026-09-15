Last updated: September 2026

Trust engineering, humorphism, and relationship design for AI products

## CORE CONCEPT

Replace user interfaces built for operating tools with human interfaces built for collaborating with AI teammates. The shift: from interaction design to relationship design, from capturing attention to cultivating trust.

### The Paradigm Shift

- Skeuomorphism (physics) → **Humorphism (dynamics of people)**
- Desktop metaphor → **Collaboration metaphor**
- Tools → **Teammates**
- Operating → **Cooperating**
- Interaction design → **Relationship design**
- Capturing attention → **Cultivating trust**

## COLLABORATION PATTERNS

| Pattern | Behavior |
|---------|----------|
| Listen | AI reports progress, listens for context shifts, stays aware of user state |
| Interrupt | Shift to real-time multimodal collaboration when complexity peaks |
| Escalate | AI surfaces blockers with context + options; never hallucinate through ambiguity |

## HUMAN-AGENT HANDOFF & FLOW

The hard UX problems in human-AI collaboration are not about the model's output quality; they are about coordinating two workers in the same workspace. Surfaced from building a human-AI writing tool (Shreya Shankar, June 2026):

| Problem | Why It's Hard | Design Response |
|---|---|---|
| Fluid lead handoff | Either party may take or cede the lead mid-task; abrupt handoffs feel jarring or like loss of control | Make the handoff explicit and reversible; show who holds the lead and let the user reclaim it instantly |
| Concurrency | Human and agent editing the same artifact create conflicts | Define ownership boundaries per region/section; surface agent edits as reviewable, not silent |
| Keeping the user in flow | A burst of agent activity pulls attention and breaks concentration | Surface agent progress ambiently; interrupt only when input is genuinely needed (see LISTEN / INTERRUPT / ESCALATE) |
| Parallelizing think-time | In chat, human think-time and agent think-time happen sequentially — each waits for the other | Design for simultaneous work: the agent makes progress while the human thinks, not after |

> Chat is structurally sequential: it forces human think-time and agent think-time to alternate. For collaborative work products, design surfaces where both progress at once. The bottleneck is coordination, not model capability.

## ESCAPE HATCH DESIGN

When a product introduces a new transactional behavior, keep a visible path back to the familiar one. The exit is not a recovery step after failure; it runs in parallel with the new path at the moment the action commits.

Google Shopping's 2015 transactional ads test (buy without leaving the results page) produced greater than 10x conversion uplift among users who understood what they were doing, and broke trust among users who did not realize they had bought something. The fix was a "check out now" action beside a large "take me to the retailer site" button. Forking the traffic resolved user confusion and partner resistance with one control.

| Design Question | Why It Matters |
|---|---|
| Is the exit visible at the commitment moment, not after it? | An exit discovered afterward prevents nothing |
| Does the exit lead somewhere the user already knows? | A novel recovery path is a second thing to learn under stress |
| Does the fork let a partner keep the transaction on their surface? | Partner resistance and user distrust often share one fix |
| Can a user who took the exit still find the new path later? | A permanent opt-out trades a trust win for an adoption loss |

> The escape hatch is orthogonal to autonomy level. The staircase sets how much the agent decides; the escape hatch preserves the familiar manual path at the point of commitment.

→ See: Competitive Strategy & Defensibility (agent-first platforms: who owns the completing surface)

## DESIGN IMPLICATIONS

> Trust accumulates through repeated competence, transparency, and respect for boundaries. You design the conditions for trust, not trust itself.

> AI earns access to data by demonstrating immediate value, not via blanket permissions upfront.

> Stop building "onboarding flows"; start designing "first weeks" of a working relationship.

> Borrow from RTS/tower defense: dynamic visual canvas of agents surfacing requests, verifications, status. You're the commander, not the operator.

> Multi-agent management as gameplay, not inbox triage.

> Cognitive expansion over cognitive offloading. Products that automate tasks (offloading) commoditize. Products that help users see patterns, connections, and possibilities they'd miss on their own (expansion) create loyalty. "Do it for me" is table stakes. "Help me see what I'm missing" is the differentiated play. (Scott Belsky)

## TRUST DESIGN PRINCIPLES

Synthesized from Anthropic's agent framework and enterprise deployment patterns:

> Trust accumulates, doesn't switch on. Design for repeated small wins, not a single convincing demo.

> Guardrails are conversion levers, not compliance overhead. Visible safety reduces fear and increases exploration.

> A single high-visibility failure can undo weeks of earned trust. Failure-first design is not optional.

> Show reasoning without overwhelming. Users need enough to calibrate confidence, not full chain-of-thought.

> Reduce cognitive load, not just clicks. The system should do the thinking, not just the task.

> When the AI isn't sure, say so. Confident wrong answers erode trust faster than honest uncertainty.

## COGNITIVE SURRENDER

When AI is too frictionless, users stop thinking and defer to AI output even when it's wrong. Ethan Mollick's term: "cognitive surrender." The risk compounds with agentic systems that just do stuff without requiring user engagement at each step.

Three studies quantify the effect:

| Study | Setup | Finding |
|---|---|---|
| Turkish high school (~1,000 students) | One group used ChatGPT for math homework, one had no AI | ChatGPT group did homework better but scored worse on tests. AI gave answers, short-circuiting the effort required for learning. |
| Taipei Python course (~1,000 students, 5 months) | AI tutor provided personalized problem sequences | Students scored 0.15 SD higher on a final exam taken without AI (equivalent of 6-9 months additional schooling). Customized tutoring enhanced learning instead of replacing it. |
| BCG consultants (758 people) | Half got GPT-4, half had no AI | AI users vastly outperformed on tasks AI handles well. On a task where AI was wrong, AI users were significantly less likely to get the right answer. They accepted the incorrect AI output without catching it. |

The design distinction: "AI that does the work" and "AI that helps you do better work" produce opposite capability outcomes. Small implementation differences (answer-giving vs. problem-sequencing) determine whether users learn or atrophy.

A small Anthropic study found that programmers who asked AI to explain what it was doing, or used AI for only part of the work, avoided surrender. Those who let AI do everything couldn't answer questions about what they'd done.

### Product Design Implications

> Default to tutoring mode over answer mode when the task involves skill development. The three major AI products offer learning modes (Gemini: Guided Learning, ChatGPT: /learn, Claude: learning style), but they're buried and unintuitive to access.

> Frictionless is not always better. When AI required elaborate back-and-forth and made frequent errors, humans had to stay engaged. Agentic systems that "just do stuff" maximize throughput but minimize learning and authenticity.

> The commercial pressure pushes toward frictionless. Designing for cognitive preservation requires intentional product choices that run counter to "make it easier" defaults.

## 6 LAWS OF AI UX

1. **Invisible setup**
   System gathers context automatically; doesn't demand preparation

2. **Cognitive offload**
   System does the thinking, not just the task

3. **Adaptive interfaces**
   UI changes based on user's cognitive mode

4. **Predictable surprise**
   AI exceeds expectations without feeling rogue

5. **Context is king**
   System remembers and uses what it knows

6. **Failure-first design**
   Recovery from mistakes is trivially easy

## AUTONOMY STAIRCASE

1. **SUGGEST**
   "Here's what I would do."

2. **DRAFT**
   "I've done it, review before it goes live."

3. **GUARDED EXEC**
   "I'll handle these automatically, but ask about those."

4. **FULL AUTONOMY**
   "I'll operate continuously within these boundaries."

**Most products jump to Level 3-4 and lose users. Start at Level 1-2. Autonomy is earned through demonstrated competence.**

### Overton Window for AI Acceptance

The range of AI behaviors users find acceptable shifts rapidly. Features considered uncomfortable today can become expected within 8 months. Sheridan's 10-level autonomy scale (from "human does everything" to "computer ignores human") maps where users currently sit, but that position moves fast.

Implication for roadmap sequencing: design the autonomy staircase knowing users will climb faster than you expect if trust is maintained. Build the infrastructure for Level 3-4 while shipping Level 1-2. The bottleneck shifts from "will users accept this?" to "can we maintain trust at each new level?"

### Structural Opacity at Level 4

At Mythos-class capability (ex: Claude 5 Fable), agents work autonomously for hours, spawning sub-agents, making hundreds of judgment calls, and delivering finished output. The human role at this level is closer to patron than editor: brief the agent, fund execution, judge the result. Process visibility becomes impractical, not because the interface is bad, but because the decision volume exceeds what a human can meaningfully track.

This is distinct from cognitive surrender. Cognitive surrender is the user choosing not to engage. Structural opacity is the user being unable to engage with the process even if they want to. Mollick: "The details of the AI's decision making are not shown to me, and the process would be too long to even be worth following."

| Oversight Model | Works When | Breaks When |
|---|---|---|
| Process monitoring (step-by-step traces) | Agent takes 5-20 steps; human can follow the logic | Agent takes hundreds of steps over hours; trace is unreadable |
| Outcome verification (judge the deliverable) | Output is evaluable by the human; domain expertise sufficient | Output requires domain knowledge the human lacks |
| Adversarial verification (separate agent checks work) | High-stakes output; cost of verification justified | Verification agent shares blind spots with primary agent |

Design implication for high-autonomy agents: invest in outcome verification tooling (structured diffs, before/after comparisons, automated eval checks) over process transparency. Users at Level 4 need confidence in the result, not visibility into every step. (Ethan Mollick, "What it feels like to work with Mythos," June 2026)

## AGENT OVERSIGHT PRINCIPLES

From Anthropic's framework for safe, trustworthy agents:

> Read-only by default. Grant persistent permissions only for routine tasks the user trusts the agent to handle.

> Users can stop and redirect at any time. Control is non-negotiable.

> Transparency into problem-solving: without visibility, users can't calibrate trust or catch misalignment early.

> The right balance between autonomy and oversight varies by scenario. Build both built-in and customizable oversight.

> Privacy boundaries are hard constraints. Agents must not carry sensitive info across contexts.

## 10 PSYCHOLOGICAL TRIGGERS

Emotional levers that determine whether users engage once or integrate permanently:

| Trigger | How to Engineer |
|---------|---|
| Competence shock | First task shows non-trivial capability done well |
| Error recovery grace | Mistakes feel fixable, reversible, safe |
| Intent understanding | System infers from context, handles imprecision |
| Autonomy preview | Show what full automation could look like |
| Tailored responses | System learns and adapts to user patterns |
| Low-stakes exploration | Sandbox mode, no permanent changes |
| Cognitive offload | System tracks, remembers, connects info |
| Predictable surprise | Adds value users didn't request but need |
| Speed premium | Fast response without visible sloppiness |
| Cross-team visibility | Easy sharing, presentation-ready output |

**Design for competence shock first. The moment users realize the AI can do something non-trivial is what converts curiosity into engagement.**

## THE FIRST 30 SECONDS

Users form trust judgments almost immediately. The first interaction must demonstrate:

> The system understands their context

> It produces useful output quickly

> It doesn't feel dangerous or unpredictable

> Errors are recoverable

**If users feel uncertain in the first 30 seconds, they won't engage deeply enough to discover value. Script this sequence like a product demo.**

## BEHAVIORAL BARRIERS BY STAGE

| Stage | Barrier | Design Response |
|-------|---------|---|
| Awareness | Inertia | Demo capability in 30s. Proof compels. |
| Acquisition | Anxiety | Sandbox mode, sample data, reversible. |
| Activation | Friction | Script first interaction, pre-fill context. |
| Engagement | Dim. returns | Learning loops, visible personalization. |
| Retention | Trust erosion | Surface uncertainty, recover from errors. |

## BEHAVIOR CHANGE AUDIT

Behavior changes only when motivation, ability, and a prompt converge (Fogg Behavior Model). Most PM effort goes to ability because friction is visible and fixable inside the product. When motivation is the missing input, ability work does not move usage, CSAT, or retention.

| Bucket | Question | Example Investments |
|--------|----------|---------------------|
| Motivation | Is the pain or payoff large enough to act? Who else cares whether the user acts? | Leadership visibility, success stories, expert endorsement, time-limited offers |
| Ability | Can the user act with little effort? | Templates, AI assistance, better search, simpler flows |
| Prompt | Does a cue arrive at the right time and place? | Digest emails, in-app notifications, reminders, first-party integrations |

### 1. Bucket the backlog

Sort planned initiatives into the three buckets. Ability and prompt blur at the edges; the value is in the distribution, not perfect classification.

### 2. Check the skew

If most investment sits in ability while usage is flat, test motivation before shipping more UX.

### 3. Find the real motivation source

In B2B, motivation is often top-down: people act because a manager or exec will look. Features that make usage visible to leadership can move adoption more than UX polish.

### 4. Stack-rank within each bucket

Pick the highest-value item per bucket rather than ranking the whole backlog on one axis.

Lattice's OKR product: three teams improved UI and flows for months under pressure from sales, CX, and the CEO, and CSAT, engagement, and usage stayed flat. Employees updated OKRs because leadership asked, not because the tool was easy. Leadership overviews were the missing investment.

> Ability work cannot compensate for missing motivation. Diagnose which input is absent before choosing the fix.

For agentic features in enterprise accounts, manager and admin expectations may be the main motivation source. Treat that as an assumption to test per account, not a default. Evidence for the pattern is one case study.

→ See: Competitive Strategy & Defensibility (behavioral segmentation: drive vs resistance)
→ See: AI Product Leadership & Execution (AI-native transformation model: leadership layer)

## MEASUREMENT

| Stop Measuring | Start Measuring |
|---|---|
| DAU | Daily Delegated Decisions |
| Time in app | Time saved by user |
| Feature adoption % | Task completion rate with AI |
| NPS alone | Human-AI pair outcome quality |
| Clicks to complete | Cognitive load reduction |

Contested: a competing view holds that AI should not change product success metrics at all, since users should experience the product as better, not as "AI." Neither position is evidence-backed. The middle path is to keep the North Star about user value and redefine what counts as a valued interaction, as Google Search (DAU to multi-need engagement) and YouTube (watch time to valued watch time) did.

Task completion with AI often lands outside the AI surface, in the task, doc, or automation the answer led to. Instrument those downstream actions, or AI metrics read low.

→ See: AI Product Leadership & Execution (North Star evolution; cross-surface attribution in KPI graph validation)

## 7 AI UX TRAPS

| Trap | Prevention |
|-----|-----------|
| Over-automating early | Start with suggestions, not execution |
| Under-guiding ambiguity | Add scaffolding when AI is uncertain |
| Outputs without explanation | Show reasoning breadcrumbs |
| Everything in a chatbox | Structured UIs for structured tasks. For transactional domains (travel, e-commerce, task management), agents need rich UI: maps, calendars, comparison tables, booking flows. Chat is a local maximum that underserves users when the task involves browsing, comparing, or committing. Chat works for users with money who want to save time. Chat fails for users with excess time seeking to make or save money. The cost-time tradeoff determines which user segments a chat interface can serve. |
| Silent failures | Always surface uncertainty and errors |
| Punishing exploration | Generous limits, sandbox mode |
| Expecting prompt eng. | Translate user intent to instructions |

## MODEL-TO-PIXEL DESIGN

Design AI products from the user experience backward into model behavior, not the reverse. The interface is shaped by intelligence in real time; model orchestration decisions (what to call, when, how to combine outputs) are UX decisions.

At scale (Grammarly: 100B+ LLM calls/week, thousands per user per day), latency and cost stop being infrastructure concerns and become UX variables:

| Decision | UX Impact |
|----------|-----------|
| Which model to call | Response quality vs wait time the user experiences |
| When to call | Proactive feels smart or intrusive depending on timing |
| How to combine outputs | Coherence of the experience across multiple model calls |
| Cost per call | Determines where and how often intelligence can appear |

> Model behavior is the product experience. You are not building features on top of models. The orchestration layer is the design layer.

This reframes the PM/design relationship with infra: latency budgets, cost ceilings, and model routing are product spec elements, not eng implementation details.

## AGENT OUTPUT FORMAT

HTML outperforms Markdown as the default agent output format. Markdown constrains agents to flat text; HTML gives them a full rendering surface: interactive tables, editable specs, annotated code review, embedded charts, collapsible sections, and custom editing interfaces.

| Format | Best For | Limitation |
|--------|----------|------------|
| Markdown | Quick replies, inline chat, developer docs | No interactivity, no layout control, no embedded logic |
| HTML | Specs, reports, code review, data exploration, design artifacts | Heavier to render, requires sandboxed display |
| Structured JSON | Machine-to-machine handoffs, API responses | Not human-readable without a rendering layer |

> When an agent can produce an interactive artifact instead of a text blob, users engage with the output rather than just reading it. The output becomes a working surface, not a deliverable. (Thariq)

Design implication: agent output rendering is a UX decision, not an infrastructure detail. The output format determines whether users can act on results immediately or must copy-paste into another tool. For Super Agents producing multi-step work products (project plans, analysis, dashboards), HTML should be the default output target.

## GENERATED UI AS FEEDBACK LOOP

Agent-generated UI can be an input, not just an output. Using a monitor primitive that reacts to each new log line as an event, an agent generates UI code that logs user interactions ("clicked X, typed Y"), subscribes to those logs in real time, and adapts the interface as the user works.

> The UI code is no longer just output, it's the input too. The agent watches how its generated interface is used and revises it live, closing the loop between generation and interaction. (Shreya Shankar, July 2026)

Design implication: for Super Agents that produce interactive artifacts, instrument the generated surface so interaction telemetry flows back to the agent. The output becomes a running feedback channel, extending "output as a working surface" into a live loop.

## AGENT-MEDIATED FEATURE DISCOVERY

In traditional products, users discover features through menus, onboarding tours, and documentation. In agent-native products, the agent itself becomes the discovery mechanism: users learn what the product can do by interacting with the agent, which routes them to capabilities they didn't know existed.

Harvey (legal AI, $100M+ NNARR, 53% DAU/MAU) found that prior to agent unification, many capabilities were only discovered by power users. The fix was architectural: cloud agents use all product surfaces rather than being siloed per feature. A single agent entry point connects to the full capability set. When users ask the agent a question, it can draw on any product surface to answer, and the user learns a new capability in the process.

| Siloed Agents | Unified Agent |
|---|---|
| Each feature has its own AI entry point | One agent accesses all product surfaces |
| Users must know where to go first | Agent routes to the right capability |
| Power users discover 60%+ of features; casual users discover 20% | Discovery gap narrows as agent introduces capabilities contextually |
| Adoption metrics siloed per feature | Command center monitors adoption/ROI across all surfaces |

Design implications:

> Build one agent identity with access to the full product graph, not per-feature agents that each solve a narrow problem. Users shouldn't need a mental model of your product architecture to get value.

> As the product becomes more connected, users discover more by learning from their agents. The agent is both the product interface and the onboarding mechanism.

> Track agent-mediated discovery as a metric: how often does an agent interaction introduce a user to a capability they hadn't used before?

## COMMON FAILURE MODES

| Failure | Symptom | Prevention |
|---------|---------|-----------|
| Trust-value gap | High interest, low activation | Script first 30s, lower risk |
| Autonomy backlash | Users disable AI features | Staircase, never skip levels |
| Silent failures | Users perceive stupidity | Surface uncertainty, show errors |
| Prompt dependency | Only power users succeed | Context packs, not blank boxes |
| Cognitive overload | Users abandon complex UIs | Structured UI, structured tasks |
| No escape hatch | Users complete an action they did not intend | Parallel visible exit to the familiar path at commitment |
| Ability over-investment | UX keeps improving; usage and CSAT stay flat | Bucket backlog by motivation, ability, prompt; test motivation first |

→ See: Context Engineering (context curation for AI features)
→ See: Prompt Engineering (behavioral framing in system prompts)
→ See: Governance & Safety (positive alignment, preference-wellbeing divergence)

---

**Sources:**
- Ethan Mollick, "Choosing to Stay Human" (May 2026). Studies: Bastani et al. (Turkey), Chien et al. (Taipei), Dell'Acqua et al. (BCG/Wharton/Harvard)
- Scott Belsky (cognitive expansion vs offloading)
- Anthropic agent oversight framework
- Brian Chesky (structured UI for transactional domains)
- Henry (Anthropic / formerly Super.com), Product Faculty AI PM Course (May 2026)
- Shreya Shankar (human-agent handoff and flow, June 2026)
- John Alternis (former VP Google, CPO Walmart), AI Leadership course (Aug 2026): escape hatch design, Google Shopping transactional ads
- Ethan Mollick, "What it feels like to work with Mythos" (June 2026): structural opacity, patron model
- @gabepereyra (Jul 1 2026): Harvey Q2 results, agent-mediated feature discovery
- Shreya Shankar (July 2026): generated UI as input / feedback loop
- Jennifer Liu / AI Leadership course (Aug 2026): Fogg model applied as a backlog audit, Lattice OKR case, Airtable bucketing exercise
- Satyajit Salgar / AI Leadership course, Product Metrics & Growth session (Sep 2026): "AI should not change your product metrics" position

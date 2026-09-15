Last updated: September 2026

Operating model, leverage levels, and strategic alignment

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

Replaces static annual planning with continuous calibration. Per-token prices fall fast, but total cost per user action moves on several trajectories at once, so a 12-month roadmap is obsolete before you finish writing it whichever way costs break.

| Component | What It Monitors | Key Question |
|-----------|---|---|
| External Pulse | Model releases, cost drops, competitor launches | Can we / afford to / must we act? |
| Internal Pulse | Unexpected usage, outlier segments, quality signals | Where are we ready to climb? |
| Guardrails | Pre-agreed thresholds and action triggers | <2%: expand. >5%: reduce. >10%: revert. |
| Monthly Review | All signals synthesized into decisions | Climb, hold, or pull back? |

Sensing is detection only. Climb, hold, and pull back all assume the current line survives, so a sensing function with no paired investment mechanism is a well-instrumented way to watch a moat compress. Pair each cycle with one funded bet against a current line.

→ See: Competitive Strategy & Defensibility (moat decay and cannibalization)

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

### North Star Evolution

The North Star should change when strategy does. Google Search moved from daily active users to SUNs, which count distinct search needs a user meets in a day. YouTube moved from watch time to "valued watch time" (time the user felt was well spent). Keep one North Star, two at most, and repeat it in every update and all-hands until teams use it without the PM in the room.

## WORLDVIEW ALIGNMENT

Misalignment hides in implicit beliefs. If the CEO believes costs approach zero and the VP Eng believes they stay significant, they make incompatible decisions without realizing they disagree.

### 1. Prepare: Distribute Assumptions Document

Cover four dimensions: Market (transformational vs incremental?), Business (cost trajectory, moat source?), Product (copilot vs agent?), User (trust evolution?).

Cost trajectory is where implicit disagreement is most common and most expensive. Five scenarios, not mutually exclusive. Have the team rank them rather than assume one:

| Scenario | Claim | If True |
|---|---|---|
| Commodity | Inference prices toward zero like electricity | Deploy aggressively; cost stops being a design constraint |
| Jevons Paradox | Cheaper inference raises demand faster than price falls | Spend rises while unit price falls; budget on volume, not rate |
| Reasoning floor | Chain-of-thought compute stays expensive | Reasoning stays a routed exception, never a default |
| Energy constraint | GPUs, power, and data centers bind before model cost does | Capacity, not price, is the planning variable |
| Agent demand | Agents call models orders of magnitude more than humans | Per-token deflation stops reducing your bill |

> Three of the five break the assumption that falling per-token prices reduce spend. A team that has not ranked these will read the same cost data and reach incompatible conclusions without noticing.

### 2. Discuss: 60-90 min Cross-Functional Workshop

Surface disagreements respectfully. Make implicit beliefs explicit. This is where you discover incompatible assumptions.

### 3. Align: Document Shared Hypotheses

One-page worldview doc. Teams don't need to believe assumptions are true. They commit to testing them together. Alignment, not agreement.

## STAKEHOLDER REVIEW CADENCE

Worldview alignment settles beliefs at planning time. Individual bets still need approval, and a single late review is where most rejections happen. Split the review into three checkpoints so direction is agreed before detail is built.

| Stage | What to Share | Ask | Output |
|-------|---------------|-----|--------|
| 1. Early exploration | Rough problem framing and directions; no spec | "Should we move in this direction?" | Directional yes or redirect |
| 2. Top 3 options | Options with trade-offs and risks | "Which option, given these trade-offs?" | Approved direction |
| 3. Final solution | Designs, remaining risks, dependencies, launch plan | "Approve to ship?" | Launch approval |

> No one should see the bet for the first time in the formal review. Walking in cold puts the team's work at risk on one meeting.

If the org has no formal cadence, build an informal one. Send decision-makers a short options note labeled exploratory, or catch them one-on-one before the review. The label matters: it invites input on direction instead of critique of a finished plan.

### Co-Creation Over Presentation

People back what they helped build. A rough map made together produces more commitment than a polished solution presented by one person.

| Tool | What the Group Builds | Alignment Effect |
|------|----------------------|------------------|
| Customer journey map | Persona, journey stages, what the user does, thinks, and feels at each stage; dot vote on top pain points | Shared view of where the pain is |
| Story map | User stories grouped by journey stage, sliced horizontally into releases (MVP, then later) | Scope debate becomes the group against the constraint, not person against person |

The hardest decision in either exercise is what to leave out of the first release. Story mapping puts that cut on the wall where everyone can see its cost.

### Challenging a Top-Down Decision

When an executive mandate conflicts with the evidence, a journey-mapping session is a low-conflict way to surface it. Invite the relevant stakeholders, map the journey, and dot vote on pain points. If the mandate does not address the top-voted pain, the group sees that without anyone arguing against the executive.

Evidence for this section is practitioner experience (Google, Lattice), not controlled comparison.

→ See: Discovery & Validation (symptom vs root problem; questioning leadership-driven initiatives)

## STAKEHOLDER NEGOTIATION

Review cadence decides when a stakeholder sees a bet. This section covers the conversation itself: a timeline squeeze, a scope fight, a resistant executive. As AI drafts PRDs, alignment work is a larger share of what separates senior PMs.

> Enter with a position and a fallback. Engage on interests first. Offer fallbacks only after the other side's interest is on the table.

### 1. Prepare

| Element | Question | Practice |
|---------|----------|----------|
| Want | What do I think we should do? | Write it down before the meeting |
| BATNA | What happens if we don't agree? | Prepare 2-3 fallback options; do not open with them |
| Style | Which mode do I default to? | Competitive or compromising when urgent; collaborative or accommodating when the long-term relationship matters |
| Leverage | What backs my position? | Data, metrics, allies, coalitions; pre-meet every stakeholder and prepare answers to every likely question before a high-stakes review |
| Audience frame | What is top of mind for them? | Open with their concern (business impact, board, cost), not the team's journey |

The urgency vs relationship split is a heuristic. There is no tested method for deciding which style a situation calls for.

### 2. In the Room

| Move | How | Why It Works |
|------|-----|--------------|
| Interests over positions | "Tell me more," "Help me understand"; avoid "Why?" | Positions are demands; the interest behind one often has a cheaper answer |
| Mirroring | Repeat their last 2-3 keywords with a similar (not identical) gesture; on video, move the camera back so hands are visible | Lowers the temperature without conceding anything |
| Aim for "that's right" | Summarize their view until they say "that's right" | "You're right" ends a conversation; "that's right" means they feel heard |
| No-oriented questions | "Is this a bad idea?" instead of "Do you approve?" | Senior people guard their yeses; saying no lowers the guard and prompts them to reason through the proposal |
| Fewer, stronger reasons | Keep the top 2-3 justifications; cut the rest | The other side attacks the weakest reason and discounts the strong ones with it |

Worked case: a CEO demands a reporting feature in 6 weeks; engineering needs 10. Open-ended questions surfaced the interest: he had promised a board member customer feedback within 6 weeks, not a launch. Resolution: an alpha with trial customers in time for the board meeting, then a separate look at compressing the full timeline. The same move applies to agent features, where the interest behind "ship by X" is often evidence of traction that an alpha cohort can supply.

Unresolved: "fewer reasons" conflicts with cultures that reward showing all the work, and there is no guidance for stakeholders who explicitly ask for exhaustive justification.

### Getting a Correct Idea Past Resistance

Challenging a Top-Down Decision covers pushing back on a mandate. This covers the reverse: your proposal is the one being resisted from above.

> Data is necessary, not sufficient. Resistance to a correct idea is often threat perception: the proposal implies someone's system or judgment was wrong.

Google Enhanced Campaigns (circa 2012): Smart Pricing cut mobile bids because it counted only same-device conversions, while users browsed on mobile and bought on desktop. Jon Alferness proposed separate mobile and desktop bids with Smart Pricing off for mobile. His manager resisted regardless of the data, and a partial rollout was impossible because once some advertisers got bid control, all would demand it. A finance contact reviewed the case and sent it to CFO Patrick Pichette, who took it to Larry Page, who had Susan Wojcicki mandate the change. It became a 2+ year company-wide effort and closed the mobile revenue gap. Alferness was sidelined by his manager for a period afterward.

| Step | Action |
|------|--------|
| 1. Map the threat | Name whose system, decision, or team the proposal implicates |
| 2. Find the pain owner | Find who feels the problem in their own numbers (often finance or a business owner, not product) |
| 3. Build with them first | Walk them through the case starting from where they are, before going broad |
| 4. Price the escalation | Decide whether the resister can be converted; going around them has a relationship cost |

Open question: this case succeeded through an escalation that reached the founders. Whether it works without that access is untested, and the proposer himself had not found a less threatening way to push the change.

→ See: Discovery & Validation (symptom vs root problem)

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

Check where the value lands. A metric or optimizer that counts only outcomes inside one surface will undervalue a surface whose value shows up elsewhere. Google's Smart Pricing counted only same-device conversions; mobile browsing drove desktop purchases, so the system kept lowering mobile bids. AI surfaces carry the same risk (synthesis): an Ask AI answer that leads to a task update, or an agent run that ends in a doc edit, credits nothing to AI if only the AI surface is instrumented. Attribute downstream actions within a session or time window before judging the AI NSM or letting automated tuning act on it.

→ See: AI UX (measurement)

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

## FEATURE LAUNCH METRICS

Every feature ships with two metrics: a key action metric for its own success and a guardrail metric for the product around it. Guardrail metrics are the step most often skipped, because PMs are rewarded for their feature's adoption, not for protecting a neighbor's metric. The omission surfaces later as a code-red escalation.

### 1. Connect to a business objective

Trace feature to product goal to company objective. Ask whether the impact is large enough to matter.

### 2. Define the key action metric

Concrete, time-bound, and stated as a share of a base (ex: 5% of users run the integration daily over 30 days). Size the target three ways: bottoms-up from comparable feature benchmarks, top-down from what impact would matter to the org, then a litmus test with stakeholders. State it as a range. No empirical method exists for first-of-kind features with no benchmark.

### 3. Set the guardrail metric

Name the adjacent metric the feature could harm and an acceptable threshold before launch.

| Feature | Key Action Metric | Guardrail Metric |
|---|---|---|
| New search option | % of search volume using it | Overall search abandonment |
| New payment method with high fees | % of payment volume | Cannibalization of cheaper methods |
| New integration | % of users using it daily | Integration flow abandonment (choice overload) |
| Watch-provider links moved to page center | Clicks when triggered (+100%) | Whole-page quality; restricted to clear-intent queries |
| Agent that replaces a rule-based automation | Agent runs completed per workspace | Automation usage and reliability; support tickets |
| New AI entry point in an existing surface | % of sessions using it | Core-surface task completion and time to first interaction |

> If the only goal is the feature's own metric, a change can hit it while damaging everything around it. Moving Watch Actions to the page center doubled clicks and degraded the rest of the page.

Culture alone does not fix the incentive gap. Assign guardrail ownership explicitly (ex: the owner of the adjacent surface signs off on the threshold) rather than relying on launch PMs to self-police.

### 4. Launch, measure, iterate

Ship to a small slice first (ex: 1% of traffic) when the guardrail risk is unclear. Launch starts measurement; it does not end it.

## METRIC DROP TRIAGE

When a metric drops unexpectedly, run these steps in order before proposing a fix.

| Step | What to Do | What It Finds |
|---|---|---|
| 1. Segment | Split by persona, location, device, lifecycle stage | Averages hiding a single broken segment |
| 2. Critical path | Walk each funnel step; find where the conversion rate changed | The specific flow that broke |
| 3. Sample sessions | Read individual sessions (for agents: traces) | Bugs and unexpected behavior |
| 4. Feature collision | Compare usage of adjacent features before and after recent launches | Cannibalization by another launch |
| 5. Anecdotes | NPS, CSAT, support tickets, direct feedback | Adoption blockers invisible in data |

Step 5 catches cases the data misses. A feature that tested well with the analysts who were interviewed failed in production because executives, who were not interviewed, had not bought in and blocked adoption.

For AI features, add model, prompt, and tool changes to step 4: a model swap is a launch that collides with every feature it serves.

→ See: AI GTM & Pricing (diagnostic application: which funnel stage is failing)
→ See: Evals & Observability (check metric direction before alerting)

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

## GOVERNING DEMOCRATIZED BUILDING

When non-technical staff can build, the constraint moves from production to curation. Walmart's internal sandbox let associates build apps and dashboards; the result was more features, metrics, and analyses than anyone could process.

> The hardest product decision stops being what to build. It becomes what to cut from everything that can now be built.

| Control | What It Prevents | Cost |
|---|---|---|
| Swim lanes | Building outside the space a role should occupy | Narrows discovery; people build only where told |
| Shared library and leaderboards | Ten teams rebuilding the same dashboard | Requires curation to stay findable |
| Token and dollar budgets | Runaway spend from open experimentation | Caps the experiment that would have paid off |
| Unstructured play time | Fear of the tools, which hardens into resistance | Directly opposes the budget caps above |

The last row sits in unresolved tension with the third. People need open time or they avoid the tools, and open time is what the caps restrict. No resolution beyond holding the balance deliberately rather than by default.

Summarization compounds the problem: tools that generate digests produce more material to read, not less.

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
| Sensing without seizing | Signals reviewed monthly; no bet funded against a current line | Require one cannibalization bet per planning cycle |
| Single cost trajectory | Plans assume falling prices lower spend | Rank the five scenarios in the worldview workshop |
| Abundance without curation | Duplicate dashboards, metrics nobody reads | Swim lanes plus a shared library; curate, don't only enable |
| Cold-room review | Bet rejected in its first and only review | Three-stage review cadence; send an exploratory options note first |
| Presented, not co-created | Stakeholders nod in the meeting, then reopen scope | Build the journey or story map with them |
| No guardrail metric | Feature hits its target; an adjacent metric drops and escalates | Name the adjacent metric and threshold before launch; assign an owner |
| Fix before triage | Team ships a fix for the wrong cause of a metric drop | Run the five triage steps in order first |
| Frozen North Star | NSM still counts activity after strategy moved to value | Revisit the NSM when strategy changes |
| Opening with the fallback | Concession made before the other side's interest surfaced | Prepare 2-3 BATNAs; hold them until interests are on the table |
| Position fight | Both sides repeat demands; no creative option emerges | Ask open-ended questions to surface the interest behind the demand |
| Laundry-list case | Stakeholder attacks the weakest reason and the pitch collapses | Cut to the 2-3 strongest reasons |
| Journey-first pitch | Executive disengages; the deck tells the team's story | Open with the listener's top concern |
| Data-only persuasion | Strong evidence, no movement | Map who is threatened and who owns the pain; build with the pain owner first |
| Single-surface attribution | AI surface looks weak while its value appears in downstream tasks | Attribute downstream actions before judging or auto-tuning the AI NSM |

→ See: Evals & Observability (quality signals for guardrail thresholds)
→ See: Governance & Safety (guardrails framework)

---

**Sources:**
- Jack Clark, Import AI 458 (May 2026)
- Noam Lovinsky, CPO Superhuman; formerly CPO Grammarly
- Rohan (OpenAI Codex PM), Henry (Anthropic), Product Faculty AI PM Course (May 2026)
- Jennifer Liu / AI Leadership course (July 2026): sense-seize-transform, cannibalization as a planning artifact
- AI Leadership course, AI Margins session (Aug 2026): inference cost trajectory scenarios; governing democratized building (Walmart internal sandbox, Jon Alferness)
- Jennifer Liu / AI Leadership course, Product Discovery & Delivery session (Aug 2026): three-stage review cadence (speaker's framing), co-creation via journey and story mapping, journey mapping to challenge top-down decisions
- Jennifer Liu, Satyajit Salgar / AI Leadership course, Product Metrics & Growth session (Sep 2026): four-step feature metric framework, guardrail metrics and PM incentives, troubleshooting playbook, North Star evolution (SUNs, valued watch time), Watch Actions placement case; agent-specific guardrail rows, guardrail ownership, and the model-swap collision note are synthesis
- Jennifer Liu, Jon Alferness / AI Leadership course, Product Leadership session (Sep 2026): negotiation preparation (BATNA, styles, leverage), in-room moves drawn from Chris Voss, Never Split the Difference, CEO timeline demo, audience framing, Google Enhanced Campaigns and Smart Pricing case; the agent alpha application and cross-surface attribution for AI surfaces are synthesis

Last updated: September 2026

Cognitive load mapping, AI-fit classification, and capability validation

## CORE CONCEPT

AI solves cognitive problems, not task problems. The question isn't "what step is difficult?" but "what thinking is difficult?"

Validate capability before commitment. A prompt prototype in 2 hours can tell you whether to invest 2 months.

## AI-SHAPED VS LOGIC-SHAPED

| AI-Shaped Indicators | Logic-Shaped Indicators |
|----------------------|-------------------------|
| User must interpret unstructured info | Clear rules determine correct output |
| Correct answer depends on context | Same input always produces same output |
| Task requires judgment or synthesis | Task is validation or transformation |
| Output quality is subjective | A decision tree could handle it |
| Human expertise helps but won't scale | Deterministic rules exist |

## COGNITIVE LOAD MAP

Map where users think hard, not where they click. These cognitive load hotspots are where AI creates leverage.

| Hotspot | What Happens | AI Opportunity |
|---------|--------------|-----------------|
| Hesitation | User pauses, unsure of approach | Suggest next action |
| Context assembly | Gathers info from multiple sources | Summarize, surface key points |
| Interpretation | Reads and extracts meaning | Clarify, summarize content |
| Judgment calls | Weighs competing factors | Surface relevant factors |
| Verification | Checks and re-checks before commit | Validate, flag inconsistencies |

## AI-FIT CHECKLIST

Run this filter before investing. All "Yes" required to proceed with confidence.

| Question | If Yes | If No |
|----------|--------|-------|
| Tolerates probabilistic outputs? | Proceed | May need deterministic system |
| Errors recoverable/detectable? | Proceed | Needs guardrails or HITL |
| Volume justifies investment? | Proceed | Manual may be cheaper |
| Can define and measure success? | Proceed | Can't eval = can't ship |
| Have or can get eval data? | Proceed | Blocked; need data strategy |
| Genuinely AI-shaped? | Proceed | Consider rules engine instead |

## LATENT VS MANIFEST PAIN

Manifest pain is pain the user can describe. Latent pain becomes legible only after exposure: users do not report it or recall it, because they have no reference for the workflow that would remove it.

Recall-based discovery systematically underrates latent-pain products. A method anchored to past behavior returns weak signal for a workflow the user has never had, so a genuinely novel product and a bad idea produce the same interview transcript.

| Signal | Manifest | Latent |
|--------|----------|--------|
| Names the problem unprompted | Yes | No |
| Existing workarounds | Spreadsheets, scripts, manual steps | None; the work is simply not done |
| Interview evidence | Consistent friction stories | Vague interest, no past instance |
| Valid evidence source | Recall and observed behavior | Exposure: prototype, trial, usage |

> For latent pain, exposure replaces recall. Do not raise the interview bar; change the instrument.

Classify before validating. Pass criteria differ by pain type, and applying manifest criteria to a latent-pain category kills it at the first gate.

| Pain Type | Gate | Pass Signal |
|-----------|------|-------------|
| Manifest | 10 conversations | 7+/10 describe similar friction |
| Latent | 10 exposures | Unprompted return after first session; pain articulated in retrospect |

Retrospective articulation is the tell. After exposure, real latent pain becomes describable ("I did not realize how much time I spent on that"). If users cannot articulate it even after using the product, it was not latent pain, it was no pain.

## SYMPTOM VS ROOT PROBLEM

Complaints are the visible layer of a problem. Fixing the complaint without finding the root can make the product worse even when the fix looks logical.

| Layer | What It Is | Example |
|-------|-----------|---------|
| Complaint | Surface, immediate, stated | "The app crashes a lot" |
| Pain point | Recurring struggle behind complaints | Loses work mid-task several times a week |
| Root problem | The cause the fix must address | Stated as a root problem sentence (below) |

Write the root as a sentence before scoping a fix: "The user [who] needs to [do what] because [insight]." If the insight slot is empty or restates the complaint, discovery is not done.

| Case | Complaint | Fix Shipped | Root Missed | Outcome |
|------|-----------|-------------|-------------|---------|
| Facebook News Feed (2018) | Fake news; noisy brand and media posts | Boosted friends and family; weighted comments and shares over likes | Content quality and misinformation, not content mix | Amplified high-reaction content; misinformation spread further |
| Snapchat redesign (2018) | Interface not user-friendly | Cosmetic layout rearrangement | New content separation confused core users | DAU decline, growth slowdown, partial rollback |

For AI features, one complaint often maps to several roots. "Ask AI gave a wrong answer" can trace to retrieval misses, permission filtering, stale context, or model error. Each root needs a different fix, and a prompt change fixes none of the first three.

| If the complaint is... | Check the root before fixing |
|------------------------|------------------------------|
| Wrong or incomplete answer | Was the right source retrieved and permitted? Was it current? |
| Agent did the wrong thing | Was intent captured in setup, or did the agent misread it? |
| "UI is confusing" | Which task failed, and what did the user expect to happen? |

> Leadership-driven initiatives get the same treatment as customer requests. Ask the executive "How did you arrive at that?" and "Tell me the last time you saw this problem." A top-down mandate is a complaint-level input until its root is found.

→ See: Probing for Deep Jobs (emotional and social layers beneath the functional request)
→ See: AI Product Leadership & Execution (stakeholder review cadence; co-creation to challenge a top-down decision)

## 10-100-1000 VALIDATION LOOP

### 1. 10 conversations: Problem clarity

Is the pain real, recurring, and expensive? Do users describe similar cognitive friction? Would they use a solution?

### 2. 100 prototype interactions: Capability

Can the model do this task? Where does it break? What context does it need? What failure modes emerge?

### 3. 1000 production logs: Reliability

What edge cases at scale? Where does retrieval drift? What cost/latency patterns? What silent failures?

## PROTOTYPE BEFORE YOU BUILD

| Method | Time | What It Validates |
|--------|------|-------------------|
| Fake door | Days | Demand and adoption rate; entry point placement |
| Wizard of Oz | Hours | JTBD, user appetite, trust in interaction model |
| Prompt prototype | Hours | Capability ceiling; whether task is even possible |
| Narrow pilot | Weeks | Production behavior, constrained users, tight scope |

Fake door: ship the entry point (button, menu item, template tile) with no feature behind it. On click, show "coming soon" and offer early access. Click-through and sign-up rates replace a guessed adoption number in the business case, and the sign-up list becomes the alpha cohort.

Wizard of Oz for agents: a human performs the agent's steps behind the interface. Use it to test whether the multi-step outcome is worth having before the model can produce it reliably. A failed Wizard of Oz test kills the idea regardless of future model quality.

Sequence the methods by cost. Each stage should retire a hypothesis before the next stage spends more.

| Stage | Methods | Hypothesis Retired |
|-------|---------|--------------------|
| Early | Fake door, Wizard of Oz, prompt prototype | Users want it; model can plausibly do it |
| Mid | Partial automation, narrow pilot | It works for a constrained segment at acceptable cost |
| Full build | Production investment | None left open on value; only scale risks remain |

> A full build should not be the first test of demand. By the time engineering commits, value hypotheses should already be settled.

## PRIORITY MATRIX

Prioritize by cognitive leverage x technical feasibility. Error severity determines guardrail investment.

| Opportunity | Leverage | Feasibility | Action |
|-------------|----------|-------------|--------|
| High value | High | High | Ship first |
| Risky value | High | High (risky errors) | Needs guardrails |
| Hard value | High | Low | Wait or prototype |
| Low value | Low | Any | Deprioritize |

## VALIDATION PASS CRITERIA

| Stage | Pass Criteria |
|-------|--------------|
| Problem, manifest (10 convos) | 7+/10 similar friction, recurring pain, expensive workarounds |
| Problem, latent (10 exposures) | Unprompted return, pain articulated in retrospect |
| Capability (100 runs) | 70%+ task completion, recoverable failures, viable cost/latency |
| System (1000 logs) | Accuracy on golden set, no catastrophic failures, net positive feedback |

## DEMAND SIGNAL VALIDATION

Revenue is a weak proxy for product-market fit while enterprise buyers hold exploratory AI budgets. Money allocated to avoid missing the cycle produces paying customers who never reach production and never renew, so early ARR can rise without durable adoption underneath it.

Validate that demand survives budget normalization.

| Signal | Weak (budget-driven) | Strong (durable) |
|--------|---------------------|------------------|
| Budget source | Innovation or exploratory line | Replaces an existing line item |
| Usage after purchase | Pilot cohort, flat or decaying | Expanding past the pilot team |
| Failure consequence | Project quietly ends | A live workflow breaks |
| Renewal driver | Strategic optionality | Measured outcome |

> A pilot that never reaches production is a research grant, not revenue. Count production deployments, not logos.

Structured PMF surveys (ex: the disappointment-score method) read more reliably than revenue under inflated budget conditions.

### Retention Curves as Behavioral PMF

Plot cohort retention (share of users who return, typically over 28-30 days). A curve that flattens, even at a low percentage, signals PMF for that cohort. A curve that trends to zero is a leaky bucket. Segment by persona: one persona flattening while another drops means PMF with one segment only, which tells you where to invest.

| Stage | Goal |
|---|---|
| Finding PMF | Any flattening, at any level |
| Optimizing | Raise the plateau (ex: 20% to 30% at day 28) |
| New persona | Flatten a previously churning segment |

Define "retained" with a key action, not a login (ex: Spotify listens 5+ minutes, Instacart places an order). The threshold is judgment: plot curves at several thresholds (ex: 3, 5, 10 minutes, or one vs three completed agent runs) and pick the one whose curve best separates returning users.

## COMMON FAILURE MODES

| Failure | Symptom | Prevention |
|---------|---------|-----------|
| Logic misclassified | AI unreliable; rules would work | AI-shaped checklist first |
| Task-level discovery | Mapped steps, not cognition | Shadow users; ask what they thought |
| Skipped prototype | Committed without validation | Mandatory prompt prototype |
| Undefined success | Can't tell if AI is working | Define threshold before dev |
| Invisible work missed | Automated easy, not hard part | Ask what new hire would struggle with |
| Built at capability cliff | Complex scaffolding, temp gap | Assess if next-gen solves natively |
| Revenue read as PMF | Paying pilots that never reach production | Check budget source and post-purchase expansion |
| Latent pain read as no pain | Novel workflow fails the interview gate; recall evidence is thin | Classify pain type first; validate latent pain by exposure |
| Symptom fixed, root missed | Complaint volume drops briefly, then a new problem appears | Write the root problem sentence before scoping a fix |
| Guessed adoption | Business case rests on an assumed 5-10% uptake | Run a fake door test on the entry point first |
| Aggregate retention read | Blended curve hides one persona with PMF and one without | Segment retention curves by persona and key action threshold |

→ See: Evals & Observability (validation/eval pipeline)
→ See: Empathetic User Interviews (10-conversation discovery phase; recall limits for novel categories)

---

**Sources:**
- Joff Redfern / AI Leadership course (July 2026): enterprise AI budgets as false PMF signal, survey-based PMF measurement
- Jennifer Liu / AI Leadership course (July 2026): latent pain in novel categories, raised as an open question; the exposure-based gate is synthesis, not the speaker's framework
- Jennifer Liu / AI Leadership course, Product Discovery & Delivery session (Aug 2026): complaint vs root problem framing (speaker's model), Facebook and Snapchat 2018 cases, fake door and staged validation sequence; the AI-feature root-cause table and the Wizard of Oz rule for agents are synthesis
- Jennifer Liu / AI Leadership course, Product Metrics & Growth session (Sep 2026): retention curves as PMF signal, persona segmentation, key action threshold calibration

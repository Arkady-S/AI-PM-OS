Cognitive load mapping, AI-fit classification, and capability validation

**March 2026**

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
| Wizard of Oz | Hours | JTBD, user appetite, trust in interaction model |
| Prompt prototype | Hours | Capability ceiling; whether task is even possible |
| Narrow pilot | Weeks | Production behavior, constrained users, tight scope |

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
| Problem (10 convos) | 7+/10 similar friction, recurring pain, expensive workarounds |
| Capability (100 runs) | 70%+ task completion, recoverable failures, viable cost/latency |
| System (1000 logs) | Accuracy on golden set, no catastrophic failures, net positive feedback |

## COMMON FAILURE MODES

| Failure | Symptom | Prevention |
|---------|---------|-----------|
| Logic misclassified | AI unreliable; rules would work | AI-shaped checklist first |
| Task-level discovery | Mapped steps, not cognition | Shadow users; ask what they thought |
| Skipped prototype | Committed without validation | Mandatory prompt prototype |
| Undefined success | Can't tell if AI is working | Define threshold before dev |
| Invisible work missed | Automated easy, not hard part | Ask what new hire would struggle with |
| Built at capability cliff | Complex scaffolding, temp gap | Assess if next-gen solves natively |

→ See: Evals & Observability (validation/eval pipeline)
→ See: Empathetic User Interviews (10-conversation discovery phase)

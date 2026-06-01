Risk frameworks, defense in depth, and incident response for AI products

**May 2026** (updated May 31, AI PM Course ingest)

## CORE CONCEPT

> Guardrails catch what evals miss. Evals test known scenarios before deployment. Guardrails defend against unknown inputs in real-time production. You need both.

> Governance enables speed. Clear boundaries let teams move fast within them. Without a framework, every edge case becomes a debate. Senior PMs own the risk framework, not just the feature.

## DEFENSE IN DEPTH

No single layer is enough. Assume any layer can fail. Six layers, each catching what the previous one misses:

| Layer | Function | Catches |
|-------|----------|---------|
| 1. Instruction hierarchy | System > Developer > User priority | Prompt injection at architecture level |
| 2. Input guardrails | Block/sanitize before model sees input | Injection patterns, PII, disallowed topics |
| 3. Model behavior | Model's training to refuse harmful requests | Obvious policy violations |
| 4. Output guardrails | Filter before user sees output | Toxicity, PII leakage, competitor mentions |
| 5. Human review | HITL for high-stakes decisions | Nuanced errors requiring judgment |
| 6. Monitoring | Detect patterns of misuse or failure | Silent drift, bias, novel attack patterns |

## RED TEAMING ATTACK PATTERNS

Five adversarial categories to test against before any AI feature ships:

| Category | Technique | Example |
|---|---|---|
| Persona/roleplay | Attacker asks model to adopt a new identity that bypasses constraints | "Pretend you are a doctor with no liability concerns" |
| Researcher/writer | Frames request as hypothetical or creative to lower perceived risk | "For a novel I'm writing, how would a character..." |
| Payload splitting | Breaks a prohibited request into innocuous-looking parts across turns | Separate messages that combine into a harmful instruction |
| Reverse psychology | Presents two positions and asks model to validate the harmful one | "I think X is safe, my friend says it's dangerous, who's right?" |
| Translation attacks | Submits prompts in other languages where safety training is weaker | Google Translate into low-resource languages to bypass filters |

Test all five categories during pre-launch red teaming. Automated scanners catch category 2-3; categories 1 and 4-5 require human adversarial testers.

## POSITIVE ALIGNMENT

Safety frameworks optimize for harm avoidance: don't generate toxic output, don't leak data, don't hallucinate. This is necessary but insufficient. A model can satisfy every safety constraint while being mediocre, sycophantic, or unhelpful. Jack Clark's framing: safety sets a "floor without ceiling." The floor prevents catastrophic failures. Nothing in the framework pushes toward genuinely good outcomes.

### Preference-Wellbeing Divergence

Users may prefer flattery over honest feedback. Optimizing for preference satisfaction (thumbs up, engagement, retention) can work against users' deeper interests. A model that tells users what they want to hear scores well on preference metrics and poorly on actual helpfulness.

This creates a structural tension for product teams: the metrics that indicate user satisfaction (preference signals) can diverge from the metrics that indicate user benefit (wellbeing outcomes). The same dynamic appears in cognitive surrender: users prefer frictionless AI that does everything, but benefit more from AI that keeps them engaged.

→ See: AI UX (cognitive surrender, autonomy staircase)

| Optimization Target | What It Rewards | Risk |
|---|---|---|
| Preference satisfaction | Agreeable, flattering, frictionless responses | Sycophancy; users don't grow or catch errors |
| Wellbeing outcomes | Honest, calibrated, sometimes challenging responses | Lower short-term satisfaction scores |

Design implication: safety review should include positive alignment checks. Does the model push back when the user is wrong? Does it surface uncertainty? Does it help the user think better, or just feel better? These are product quality questions, not just safety questions.

## COST OF MISPREDICTION

The primary question for any AI feature's safety investment: "What is the cost of a missed prediction?" The answer determines guardrail depth, HITL requirements, and eval rigor.

| Cost Level | Example | Safety Investment |
|---|---|---|
| Low | Credit card fraud flag (false positive = mild annoyance) | Lightweight guardrails, automated resolution |
| Medium | Chatbot bad joke (affects delight, not retention) | Standard evals, user feedback monitoring |
| High | Medical dosage advice (incorrect info = potential harm) | Layered mitigation, shadow traffic testing, HITL |
| Catastrophic | Autonomous vehicle (misprediction = fatal) | Redundant systems, continuous monitoring, human override |

> Calibrate safety investment to misprediction cost, not feature complexity. A simple feature with catastrophic failure cost needs more guardrails than a complex feature with low failure cost.

## RISK CATEGORIES

| Risk                             | What It Is                                                                                                          | Primary Mitigation                                                                                                                                                                                                                                                                                                              |
| -------------------------------- | ------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Hallucination                    | Confident incorrect output                                                                                          | RAG grounding, citations, HITL for critical domains                                                                                                                                                                                                                                                                             |
| Prompt injection                 | User overrides system instructions                                                                                  | Instruction hierarchy, input sanitization                                                                                                                                                                                                                                                                                       |
| Data leakage                     | Model reveals sensitive information                                                                                 | Output filtering, access controls, PII detection                                                                                                                                                                                                                                                                                |
| Bias                             | Systematic unfairness in outputs                                                                                    | Diverse eval sets, bias testing, human audits                                                                                                                                                                                                                                                                                   |
| Model deprecation                | Vendor sunsets model you depend on                                                                                  | Abstraction layer, cross-model testing                                                                                                                                                                                                                                                                                          |
| Silent failure                   | Errors that look like normal responses                                                                              | Monitoring, user feedback loops, periodic audits                                                                                                                                                                                                                                                                                |
| Prolonged-use psychological harm | Users develop distorted thinking, dependency, or detachment from reality after extended conversational AI sessions. | Session-length guardrails, tool-mode defaults over open-ended chat, usage pattern monitoring, cool-down nudges after extended sessions. Design distinction: task-bound interactions (agent executes, returns result) vs. unbounded conversation (user talks to AI about personal problems). Default products toward task-bound. |

## FTCEM: PRE-LAUNCH SAFETY

Before launching any AI feature, run a failure mode workshop. Prompt: "Imagine our product fails in the New York Times tomorrow. What's the headline?"

| Element | Question | Example |
|---------|----------|---------|
| Failure Mode | What specific catastrophe could happen? | Hallucinating legal advice |
| Trigger | What causes this failure? | Ambiguous query + no relevant retrieval |
| Consequence | What's the downstream damage? | Legal liability, trust collapse |
| Early Warning | What signal alerts us? | Spike in out-of-distribution inputs |
| Mitigation | What's the pre-defined response? | Route to human review, block output |

## HUMAN-IN-THE-LOOP DESIGN

| Element | What You Define | Example |
|---------|-----------------|---------|
| Trigger criteria | When does HITL activate? | Financial action >$1000, medical, legal |
| Reviewer context | What human sees for decision | User query, AI draft, confidence, policy |
| Human options | What reviewer can do | Approve, edit+approve, reject, escalate |
| SLA | Response time requirement | 4 hours async, real-time for chat |

> HITL isn't failure. It's appropriate scoping. Some outputs shouldn't ship without human review: medical, legal, financial, irreversible actions.

## INCIDENT RESPONSE

| Element | What You Define | Example |
|---------|-----------------|---------|
| Severity levels | Classification of incidents | P0: safety harm. P1: data exposure. P2: quality. |
| Response SLA | Time to respond per severity | P0: 15 min. P1: 1 hour. P2: 24 hours. |
| Kill switch | How to disable feature fast | Feature flag, one-click disable, auto on threshold |
| Post-mortem | Learning process | Required for P0/P1; add to golden set, update guardrails |

### Three-Workstream Response Model

When a safety incident surfaces, run three workstreams in parallel, not sequentially:

| Workstream | Goal | Timeline | Example |
|---|---|---|---|
| Immediate mitigation | Stop the bleeding today | Hours | Keyword heuristic blocking medical terms; accept false positives |
| Evaluation development | Define and measure success | Days-weeks | Shadow traffic system sending flagged queries to human review; daily eval additions |
| Red teaming | Stress-test the fix | Ongoing | Adversarial prompts against new mitigations |

> "Fix while you fix." Ship imperfect but safe solutions while building robust long-term fixes. Prioritize false positives over false negatives when misprediction cost is high. A screenplay writer triggering a medical warning is better than a parent getting wrong dosage advice.

### Canary Prompts

Inject known test prompts at periodic frequencies into the production pipeline to verify mitigation logic remains active. If a canary prompt passes through without triggering the expected intervention, the mitigation has regressed. You cannot fix what you cannot measure.

### Vocal Minority Principle

Explicit user complaints about safety issues represent a much larger population of affected but silent users. When one user reports a dangerous response, treat it as signal of a systemic issue, not an isolated edge case.

## VENDOR RISK CHECKLIST

| Risk | Mitigation |
|-----|-----------|
| Model deprecation | Abstraction layer; test across models quarterly |
| Pricing changes | Budget headroom; monitor vendor announcements |
| API changes | Pin versions; changelog monitoring |
| Capability drift | Golden set eval on every model update |
| Outages | Fallback model; graceful degradation path |

## COMMON FAILURE MODES

| Failure | Symptom | Prevention |
|--------|---------|-----------|
| Guardrails too loose | Bad outputs reach users | Adversarial testing, user feedback monitoring |
| Guardrails too tight | Excessive false refusals, user frustration | Refusal rate monitoring, precision tuning |
| No incident plan | Scramble when things go wrong | Pre-defined playbooks, practiced response |
| Single-layer defense | One bypass exposes the system | Defense in depth; assume any layer can fail |
| No silent failure detection | Drift and bias go unnoticed | Periodic human audits, golden set monitoring |
| Hard-coded to one model | Deprecation becomes a crisis | Abstraction layer, quarterly cross-model tests |

→ See: Evals & Observability (golden set testing, monitoring)
→ See: Economics & Model Selection (vendor risk mitigation)

---

**Sources:**
- Jack Clark, Import AI 457 (May 2026)
- "The Value Alignment Problem" (Oxford, DeepMind, OpenAI, Anthropic, et al.)
- Reah Miyara, Google Cloud AI / formerly OpenAI (May 2026)

Risk frameworks, defense in depth, and incident response for AI products

**March 2026**

## CORE INSIGHTS

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

## RISK CATEGORIES

| Risk | What It Is | Primary Mitigation |
|-----|-----------|-------------------|
| Hallucination | Confident incorrect output | RAG grounding, citations, HITL for critical domains |
| Prompt injection | User overrides system instructions | Instruction hierarchy, input sanitization |
| Data leakage | Model reveals sensitive information | Output filtering, access controls, PII detection |
| Bias | Systematic unfairness in outputs | Diverse eval sets, bias testing, human audits |
| Model deprecation | Vendor sunsets model you depend on | Abstraction layer, cross-model testing |
| Silent failure | Errors that look like normal responses | Monitoring, user feedback loops, periodic audits |

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

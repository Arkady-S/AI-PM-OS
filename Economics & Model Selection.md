Cost, latency, and capability ceilings for every AI product decision

**May 2026** (updated May 31, AI PM Course ingest)

## CORE CONCEPT

Every token has a cost (money), a latency cost (time), and an attention cost (quality). Model selection is a product decision with 30-50x cost implications, not an engineering detail.

Choose the cheapest model that clears your quality bar. Optimize the token budget before optimizing the prompt. Build for vendor optionality before you need it.

## KEY PRINCIPLES

> Input tokens are cheap; output tokens cost 3-4x more. Constrain output length first.

> Prompt caching: stable prefixes (system prompts, examples) cached for 50-90% savings. Put stable elements first.

> Streaming reduces perceived latency dramatically. A 3s streaming response feels faster than 2s blocked.

> Model deprecation is when, not if. Build an abstraction layer between product and provider.

> Price for p95 cost, not averages. Two users on the same plan can have radically different cost profiles.

> Wait vs. build: building complex scaffolding around a capability gap that disappears in 6 months is waste.

## MODEL SELECTION FRAMEWORK

1. **Define the quality floor** - Minimum acceptable performance. Be specific: "90% accuracy on golden set" not "good enough."

2. **Define the latency budget** - p95 response time requirement. Streaming or blocked?

3. **Estimate volume** - Monthly queries at launch and at scale. Cost scales linearly.

4. **Test tiers bottom-up** - Start cheapest. Clears quality floor? Ship it. If not, move up one tier.

5. **Calculate unit economics** - Cost/query x queries/user x users = AI cost. Does this work in your model?

## REASONING COST-BENEFIT MATRIX

Extended thinking / reasoning models (OpenAI O-series, Claude extended thinking, DeepSeek R1) are a product decision, not an automatic upgrade. They trade 5-20x cost and 5-60+ second latency for deeper logic.

| | High Stakes | Low Stakes |
|---|---|---|
| High Complexity | Strong reasoning candidate (ex: medical diagnosis, financial advisory) | Test if cost is justified; often standard generation suffices |
| Low Complexity | Reasoning as safety margin (ex: high-value but simple compliance check) | Standard generation preferred; reasoning adds overhead without benefit |

Three dials move simultaneously: quality (better logic), latency (seconds to minutes vs. sub-second), cost (5-20x per query).

> Reasoning doesn't always improve output. On creative tasks and conversational exchanges, it can produce overthinking (unnecessary caveats), less creative responses (more rigid, formulaic), and reasoning collapse (model loses thread in long chains). Wrong assumptions compound through reasoning steps, creating hidden hallucination.

Apply reasoning surgically: route complex queries to reasoning models, simple queries to standard. Cascading (start fast, escalate if confidence is low) is the default architecture for mixed-complexity workloads.

## 7-LAYER COST STRUCTURE

Costs compound across layers, not add. A "simple $0.04 call" becomes a "$0.40 workflow" when layers stack.

| # | Layer | Cost Driver |
|---|-------|-------------|
| 1 | Data Preparation | Scope (not usage) |
| 2 | Retrieval | Query volume x chunks retrieved |
| 3 | Context Construction | Token count per request |
| 4 | Model Execution | Model tier x tokens |
| 5 | Orchestration | Calls per user action (often 5-10x) |
| 6 | Parallelism | Peak load, not average |
| 7 | Evaluation & Monitoring | Scales with trust requirements |

## SAAS P&L SHIFT

Traditional SaaS has near-zero marginal cost. AI breaks this. Every query has variable compute cost that scales with usage.

| Dimension | AI Impact |
|-----------|-----------|
| Marginal cost | Variable + significant (inference, retrieval, context) |
| Cost at scale | Linear with usage; popular features cost more |
| Gross margin | Can degrade with scale unless actively managed |
| Feature econ. | Each AI feature has its own P&L |
| Optimization | Per-interaction (tier, tokens, caching, retrieval) |

## COST OPTIMIZATION LEVERS

In priority order, highest impact first:

| Lever | Impact | When to Use |
|-------|--------|-------------|
| Output length limits | 2-5x | Responses are verbose |
| Model downgrade | 10-50x | Quality acceptable at lower tier |
| Prompt caching | 50-90% | Stable system prompts, few-shot |
| Prompt compression | 2-5x | Long context, verbose RAG chunks |
| Batch processing | 50% | Non-real-time workloads |
| Self-hosting | Variable | High volume, data privacy needs |

## LATENCY LEVERS

| Lever | Impact | When to Use |
|-------|--------|-------------|
| Streaming | Perceived latency | Always for user-facing |
| Smaller model | TTFT + gen speed | Quality floor allows |
| Shorter prompts | TTFT reduction | Context is bloated |
| Parallelization | Wall clock time | Independent LLM calls |
| Edge deployment | Network latency | Global users, latency-critical |

## VENDOR RISK CHECKLIST

| Risk | Mitigation |
|------|-----------|
| Model deprecation | Abstraction layer, test across models quarterly |
| Pricing changes | Monitor announcements, budget 20% headroom |
| API breaking changes | Pin versions, changelog monitoring |
| Capability drift | Regression tests on model version updates |
| Outages | Fallback model, graceful degradation |

## QUICK COST MODEL (100K CONVOS/MO)

5 turns avg, 500-tok system prompt, 1K-tok RAG, 200-tok responses:

| Model | Monthly | Per Convo |
|-------|---------|-----------|
| GPT-4o | ~$4,000 | ~$0.040 |
| Claude Sonnet | ~$4,900 | ~$0.049 |
| GPT-4o-mini | ~$120 | ~$0.001 |
| Claude Haiku | ~$160 | ~$0.002 |

30-40x difference between tiers. Model selection is a product decision, not an engineering detail.

## COMMON FAILURE MODES

| Failure | Symptom | Prevention |
|---------|---------|-----------|
| Premium model default | AI costs consume margin at scale | Test tiers bottom-up; cheapest that clears quality floor |
| Ignoring output tokens | Cost 3-4x higher than expected | Constrain output length first |
| No vendor abstraction | Model deprecation becomes crisis | Abstraction layer between product and provider |
| Averaging costs | P95 users blow budget | Price for p95 cost, not averages |
| Building vs waiting | Complex scaffolding for temporary gap | Assess if next-gen model solves natively |
| No per-feature economics | Subsidizing expensive features with cheap ones | Each AI feature gets its own P&L |

→ See: Governance & Safety (vendor risk mitigation, model deprecation)

---

**Sources:**
- Product Faculty AI PM Course (May 2026)

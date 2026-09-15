Last updated: September 2026

Cost, latency, and capability ceilings for every AI product decision

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

## CARELESSNESS AS A SELECTION AXIS

Benchmarks rank capability; production is gated by carefulness. The trait that decides whether a model is safe to run unattended is whether it verifies its own work, not its peak score. For customer-facing or agentic work (Super Agents), weight this above raw benchmark position.

> "It must first and foremost be careful. That's the only dimension that matters for real production, customer-facing work." A more capable but less careful model is a worse production choice than a careful, slightly weaker one. (Steve Yegge)

Self-verification is the observable form of carefulness: the model runs its output, catches what broke, fixes it, and re-runs before returning, rather than handing over a confident first draft and letting the user find the errors. Newer frontier models (ex: Opus 5) are trained toward this loop; treat "checks its own work" as a selection criterion, not a bonus.

A well-designed harness shows up as a lower failure rate on model swaps, not a benchmark jump. If a model upgrade produces a large day-one behavior change, the harness was over-fit to the prior model. Push carefulness into the harness (verification loops, self-check steps) so a model swap is uneventful.

→ See: Tools & Orchestration (verification loops; adversarial verification)

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

## THE VERIFICATION TAX

Inference is the visible cost. The cost of making an output trustworthy enough to ship is larger and routinely excluded from unit economics.

| Cost | Driver | Scales With |
|---|---|---|
| Human review | Reviewer time per output | Volume x share requiring review |
| LLM-as-judge loops | Judge calls per output | Volume x criteria per output |
| Compliance monitoring | Logging, retention, audit response | Regulatory surface, not usage |
| Guardrail maintenance | Rule updates, red-team cycles, regression fixes | Model changes and incident rate |
| Exception handling | Escalation, retries, manual completion | Failure rate x cost per escalation |

> Two features on the same model can have opposite margins if one ships unverified and the other requires human sign-off. Compare per-feature economics only after both carry their verification cost.

Agentic features carry the heaviest tax: multi-step runs need trajectory review, approval gates, and exception paths that single-turn features do not. Counting inference alone makes an agentic surface look structurally cheaper per unit of work than a reactive one, which distorts routing, pricing, and investment decisions between them.

No public benchmark exists for verification cost as a share of total. Treat this as a checklist that forces the line items into the model, not as a ratio you can assume.

→ See: Evals & Observability (verifier economics; cheap-judge substitution)

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

## PROMPT CACHING DEEP DIVE

Prompt caching is the single highest-impact cost optimization for production AI agents. KV-cache hit rate (the percentage of input tokens served from cache rather than recomputed) is the most important production metric for agent economics, per Manus AI.

### How It Works

LLMs process input tokens by computing key-value (KV) pairs in the attention mechanism. These KV pairs encode the model's "understanding" of the input. Prompt caching stores these computed KV pairs so that when the same prefix appears in a subsequent request, the model skips recomputation and reads from cache. Only the new tokens after the cached prefix need fresh computation.

Requirements for cache hits:
- The cached portion must be an exact prefix match (same tokens in the same order)
- Stable elements (system prompt, few-shot examples, tool definitions) should go first in the prompt
- Variable elements (user query, retrieved context) go last
- Most providers require a minimum prefix length for caching to activate (ex: Anthropic requires 1,024+ tokens for automatic caching, 2,048+ for some models)

### Model-Specific Savings (Jun 2026 benchmarks on real agent trajectories)

| Model | Cache Hit Savings | Effective Cost Reduction |
|---|---|---|
| claude-haiku-4-5 | 90% off cached tokens | -77% total input cost |
| gpt-5.4-mini | 50% off cached tokens | -80% total input cost |
| gemini-3.5-flash | 75% off cached tokens (>128 tokens) | -49% total input cost |

The variation comes from differences in pricing structure (how much the cache discount is per provider), typical agent trajectory length (longer trajectories = more cacheable prefix), and minimum cache thresholds.

### Maximizing KV-Cache Hit Rate

Structure prompts so the maximum number of tokens appear in the stable prefix:

1. **System prompt**: instructions, persona, constraints (most stable, always first)
2. **Tool definitions**: schemas, descriptions (change rarely)
3. **Few-shot examples**: input/output pairs (change per feature, not per request)
4. **Domain context**: wiki memory, AGENTS.md content (changes infrequently)
5. **Retrieved context**: RAG chunks, search results (varies per query)
6. **Conversation history**: prior turns (grows per session)
7. **User query**: current request (always different)

Items 1-4 form the cacheable prefix. Items 5-7 are variable. Moving tool definitions or examples below the user query destroys cache hits for all preceding tokens.

Harness-level automation: Deep Agents (LangChain) automatically sets cache breakpoints at the boundary between stable and variable context, removing the need for manual prompt structure management.

### Interaction with Model Routing

Prompt caching creates a strong economic bias toward sticking with the initially-routed model. Switching models mid-session resets the cache (different model = different KV computation), so you pay full price for the first request on the new model. This means:

- Model routing strategies that switch frequently between models sacrifice caching benefits
- For multi-turn agent sessions, the cost of switching models is not just the current request but the loss of accumulated cache
- In practice, route at session start and stick with the chosen model unless quality requires escalation

## MODEL ROUTING VS MODEL COUNCIL

Two runtime model selection strategies beyond the upfront selection framework above:

| Strategy | How It Works | Optimize For | Trade-off |
|---|---|---|---|
| Model routing | Classify each request, route to one model | Cost (send easy queries to cheap models) | Misrouting sends hard queries to weak models |
| Model council | Send to multiple models, aggregate outputs | Frontier performance (ensemble wisdom) | Multiplied cost per request |

Routing is the default for production. Council is for use cases where correctness matters more than cost (ex: medical, legal, financial advisory). Devin Fusion (Cognition) implements a hybrid: mix model tiers within a single session, reducing cost 35% while maintaining Fable-level quality on coding tasks.

The prompt caching interaction (above) creates a further constraint: once you route to a model, switching is expensive. This biases routing toward conservative initial selection (choose a model you won't need to upgrade from mid-session).

### Routing as a Product Category

Routing has moved from an in-house technique to a standalone product (model gateways). Ramp Router (built internally, then opened externally) cut LLM costs ~30% while making features faster and smarter with no application rewrite; Cursor Router claims frontier-quality results at ~60% lower cost by selecting the model per task. The pressure driving productization: the price-intelligence-latency frontier shifts roughly weekly, so a statically-chosen model is either overpaying or under-serving within a month.

The stakes are quantified: rebuilding SQLite with a team of agents, cost varied 15x depending on the model mix used for the same task. Model selection inside a workflow is a primary cost lever, not a rounding error.

### Harness-Aware vs Gateway Routing

Two placements for the router, an open architectural question:

| Placement | Router sees | Best when |
|---|---|---|
| Harness-aware | Task structure, tool set, trajectory state | Routing depends on where the agent is in a multi-step task |
| Gateway (harness-agnostic) | Request in, response out | One routing layer across many apps/harnesses, model-swappable |

A gateway is simpler and reusable across products; a harness-aware router makes better mid-task decisions but couples routing to the harness. Weigh against the caching-lock-in constraint above: frequent mid-session switching by either placement sacrifices cache hits.

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
| Inference-only costing | Agentic features look cheaper than reactive ones | Carry verification cost in every per-feature P&L |

→ See: Governance & Safety (vendor risk mitigation, model deprecation)

---

**Sources:**
- Product Faculty AI PM Course (May 2026)
- AGENTIC Twitter List digests, Jun 21-Jul 3 2026 (prompt caching economics, model routing vs council)
- @its_ao (Jun 2026), @hwchase17 (Jun 2026), Manus AI: prompt caching benchmarks
- @cognition (Jun 2026): Devin Fusion model routing
- @Steve_Yegge via @_catwu, @clickup, @mattpocockuk (Jul 2026): carefulness / self-verification as a model-selection axis
- @rahulgs (Ramp Router, ~30%), @cursor_ai (Cursor Router ~60%; SQLite rebuild 15x cost variance), @hwchase17 (Jul 2026): routing as a product category; harness-aware vs gateway routing
- AI Leadership course, AI Margins session (Aug 2026): the verification tax as a unit-economics category

How LLMs act in the world: function calling, pipelines, and agent loops

**July 2026** (updated Jul 3, AGENTIC Twitter List ingest)

## CORE CONCEPT

The model proposes, your app executes. The LLM outputs structured JSON saying "call this function with these arguments." Your code runs it and returns the result. The model never directly executes anything.

Agent reliability scales with constraint. Narrow, well-defined scope is testable and shippable. Broad autonomous agents are impressive demos and production nightmares.

## KEY PRINCIPLES

> Fewer tools is better. Every tool is context overhead that burns tokens. Start with 3-5 core tools; add based on observed need, not anticipated need.

> Frameworks for prototypes, custom for production. LangChain for exploration; production usually ends up with custom orchestration.

> State management is the hard part. Most agent failures trace to state bugs, not model capability. What persists across turns?

## TOOL DESIGN PRINCIPLES

| Principle | What It Means | Example |
|---|---|---|
| Clear scope | Each tool does one thing | search_orders not search_everything |
| Descriptive name | Name indicates function | get_customer_by_email not lookup |
| When-to-use Description | Says when | "Use when user asks about order status" |
| Explicit params | Required vs optional clear | order_id (req), include_history (opt) |
| Structured errors | Errors model can interpret | {error: order_not_found, suggestion: ...} |

## PIPELINE VS. AGENT

### Pipeline (Default)

- Static sequence of steps
- Predictable and debuggable
- Same flow for all queries
- Can enumerate all paths

### Agent (When Necessary)

- Model decides next action
- Next step depends on results
- Different queries, different paths
- Flexible but harder to test

Default to pipeline. Only use agent orchestration when you genuinely can't predict the flow. Most "agent" use cases are better served by pipelines with a few branch points.

### Agent Decision Framework

Five dimensions for evaluating whether a task warrants agent architecture vs. simpler automation:

| Dimension | Build Agent When | Don't Build Agent When |
|---|---|---|
| Task complexity | Open-ended, unpredictable workflows requiring dynamic decisions (ex: software debugging) | Simple, predictable tasks with clear workflows (ex: expense approval) |
| Performance requirements | Sophisticated context management and deep reasoning needed (ex: medical diagnosis) | Quick response time and deterministic behavior required (ex: real-time fraud detection) |
| Resources | Value generated justifies higher operational costs (ex: high-stakes financial advisory) | Simpler solutions suffice (ex: basic content summarization) |
| Environment | Trusted environments allowing experimentation (ex: internal tools) | High-risk environments where errors are costly (ex: critical infrastructure controls) |
| Scale | Flexible model-driven decisions needed at scale (ex: large customer service operation) | Straightforward rules handle all cases at any scale (ex: timesheet processing) |

> When NOT to use agents: deterministic processes (rent payments, simple transactions), narrow repetitive tasks, anything where a decision tree covers all cases. The question is whether the task requires judgment under ambiguity, not whether it's complex.

## THE ORCHESTRATION LOOP

The fundamental cycle for any agent or multi-step system:

### 1. Observe

Read current state: user input, tool results, conversation history

### 2. Think

Model decides what to do next: call a tool, ask for clarification, or respond

### 3. Act

Execute the chosen tool or produce output

### 4. Repeat

Feed result back into context. Continue until termination condition met

### 4-Level Loop Stack

The observe/think/act cycle above is the innermost loop (Level 1). Production agent systems stack four loop levels with increasing time horizons. Value compounds primarily at Levels 3 and 4.

| Level | Loop | Time Horizon | What It Does |
|---|---|---|---|
| 1 | Agent loop | Single task | Model calls tools until task complete (observe/think/act above) |
| 2 | Verification loop | Wraps Level 1 | Grader checks output against rubric; rejects and re-runs on failure |
| 3 | Event-driven loop | Hours to days | Connects agent to ecosystem via triggers (webhooks, cron, channel messages). Agent activates on external events, not just user prompts |
| 4 | Hill-climbing loop | Days to weeks | Analysis agent reviews accumulated traces, rewrites harness config (prompts, tool selection, routing rules) with improved settings |

> Loops amplify behavior, making them a double-edged sword. To avoid amplifying bad patterns, you need an engaged human in the loop. The agent needs to learn your taste.

Level 1 is table stakes. Level 2 adds correctness guarantees. Level 3 makes agents proactive. Level 4 makes agents self-improving. Most teams build Levels 1-2 and skip 3-4, which is where compounding returns live.

## PRD ELEMENTS: TOOL DEFINITIONS

| Element | What You Define | Example |
|---|---|---|
| Tool inventory | What tools the system has | search_orders, get_customer, ... |
| Tool schemas | Name, desc, params each | See tool design principles |
| Tool limits | Max tools, calls per turn | 5 tools max, 3 calls per response |
| Error handling | What happens on failure | Retry once, then escalate |

## PRD ELEMENTS: ORCHESTRATION

| Element | What You Define | Example |
|---|---|---|
| Orch. mode | Pipeline or agent | Pipeline with 2 branch points |
| State mgmt | What persists across steps | Order ID, customer ctx, action history |
| Termination | When the flow ends | Task complete, max 5 steps, user stop |
| Fallback | What if agent gets stuck | After 3 fails, hand off to human |

## EXAMPLE TOOL SCHEMA

```json
{
  "name": "search_orders",
  "description": "Search customer orders. Use when user asks about order status, delivery, or order history.",
  "parameters": {
    "customer_id": {
      "type": "string",
      "required": true
    },
    "status_filter": {
      "type": "string",
      "required": false,
      "enum": ["pending", "shipped", "delivered"]
    },
    "limit": {
      "type": "integer",
      "default": 5
    }
  }
}
```

## FRAMEWORK LANDSCAPE

### LangChain

Rich abstractions, fast prototyping. Gets messy at scale.

### LlamaIndex

Strong data connectors. Better for RAG-heavy apps.

### Raw APIs

Full control, most debuggable. More boilerplate.

Most production systems end up closer to raw APIs than frameworks. Abstraction overhead exceeds value at scale.

## HARNESS ENGINEERING

Agent = Model + Harness. The harness is every piece of code, configuration, and execution logic that isn't the model itself. A raw model is not an agent. It becomes one when a harness gives it state, tool execution, feedback loops, and enforceable constraints.

### Harness Components

| Component | What It Provides | Why the Model Needs It |
|---|---|---|
| System prompts + skills | Task framing, domain knowledge | Model has no context without injection |
| Filesystem | Durable storage, workspace, collaboration surface | Model can only operate on what's in context; filesystem lets it offload and persist |
| Bash / code execution | General-purpose tool; model writes and runs code on the fly | Removes constraint of pre-configured tool set; agent solves problems autonomously |
| Sandbox | Safe isolated environment with pre-installed tooling | Agent-generated code needs somewhere to run without risk to host |
| Memory (AGENTS.md, MEMORY.md) | Continual learning across sessions | Without persistent memory, every session starts from zero |
| Orchestration logic | Subagent spawning, handoffs, model routing | Complex tasks require decomposition and parallel execution |
| Hooks / middleware | Deterministic execution: compaction, continuation, lint checks | Some behaviors must be reliable, not probabilistic |

> The filesystem is the most foundational harness primitive. It enables durable state, incremental offloading, and multi-agent coordination through shared files. Git adds versioning, rollback, and branching on top.

### Tool Call Offloading

Large tool outputs clutter the context window without providing useful information. The harness keeps the head and tail tokens of tool outputs above a size threshold and offloads the full output to the filesystem. The model can access the full output if needed, but context stays clean by default.

### The Ralph Loop

A harness pattern for long-horizon autonomous work. The harness intercepts the model's exit attempt via a hook and reinjects the original prompt in a clean context window, forcing the agent to continue against a completion goal. Each iteration starts with fresh context but reads state from the filesystem written by the previous iteration.

Requirements: filesystem for state persistence across iterations, a plan file the agent updates to track progress, self-verification at each iteration boundary (run tests, check output against goal).

### Model-Harness Co-Evolution

Today's agent products (Claude Code, Codex) are post-trained with models and harnesses in the loop. Useful primitives get discovered, added to the harness, then used during training of the next model generation. This creates a feedback loop where models become more capable within the harness they were trained in.

Side effect: changing tool logic leads to worse model performance even when the model is intelligent enough to adapt. Example: the apply_patch tool logic for file editing in Codex. A truly general model should handle different patch methods, but training with a specific harness creates overfitting to that harness's conventions.

> The best harness for your task is not necessarily the one a model was post-trained with. On Terminal Bench 2.0, Opus 4.6 in Claude Code scores far below Opus 4.6 in other harnesses. Harness optimization for your specific workload has significant upside independent of model choice.

Model-Harness fit is one of three optimization surfaces. Harness-Task fit (does the harness's tool set, context strategy, and continuation logic match the task's structure?) and Model-Task fit (does the model's training distribution cover the task domain?) are independent variables. Optimizing only one pair leaves performance on the table. (Viv, May 2026)

### Composable Harness Architecture

Monolithic harness loops bundle context assembly, tool execution, memory retrieval, output validation, and continuation logic into a single orchestration function. The composable alternative decomposes these into independent workers on a shared message bus. Each worker handles one concern and can be swapped, versioned, or A/B tested independently.

Mike Piccolo's iii system runs 15 independent harness jobs on a shared bus. Each job reads from and writes to the bus without direct coupling to other jobs. This makes the harness "a slider, not a fork": you tune individual capabilities (ex: swap memory retrieval strategy, change context assembly logic) without rebuilding the loop.

| Monolithic Harness | Composable Harness |
|---|---|
| Single orchestration loop | Independent workers on shared bus |
| Change one piece, retest everything | Change one worker, test in isolation |
| A/B test the whole harness | A/B test individual workers |
| Simpler to build initially | Higher setup cost, lower iteration cost |
| Debugging follows one path | Debugging requires bus observability |

Design implication for PRDs: when specifying harness architecture, decide whether each harness concern (context, tools, memory, validation, continuation) needs independent iteration velocity. If yes, design for composability upfront. If the harness is simple (3-5 components, stable requirements), monolithic is fine.

### Agent-on-Agent Monitoring

Long-running or parallel agent sessions drift as errors accumulate. A secondary agent session with fresh context monitors the primary session by reading the original spec and recent transcript turns. Two drift types require different check cadences:

| Drift Type | What It Catches | Check Cadence | Example |
|---|---|---|---|
| Execution drift | Is the agent doing the task right? | Frequent (every N steps) | Ignoring an error, reporting a bad metric, diverging from spec |
| Direction drift | Is the agent doing the right task? | Occasional (milestone boundaries) | Misinterpreting original intent, building the wrong thing for hours |

Implementation: two tmux panes (or equivalent), one primary, one monitor. Initial instructions and follow-up prompts go to a shared file. The monitor periodically reads the spec against the primary's recent transcript and provides course-correction feedback.

For parallel sessions sharing a repo, use git worktrees so each session gets its own checkout. The bottleneck shifts from doing the work to writing clear specs and reviewing outputs fast enough to keep the pipeline moving.

### Transcript Mining for Config Updates

Agent session transcripts contain systematic signal about gaps in CLAUDE.md, skills, and harness configuration. Scan past user turns for correction patterns: "can you also...", "did you check...", "still wrong", "I already told you." Hit counts show how often a correction recurs; the transcript shows exactly what failed.

Each correction pattern maps to an update: a missing rule in CLAUDE.md, a missing step in a skill, or a broken verification check. Make corrections within the active session (not by editing config files directly) so the transcript captures before-and-after pairs. After corrections converge, have the model merge feedback into the config.

Periodically refactor configs: rules should live in exactly one place (though critical instructions can repeat in the main CLAUDE.md). Check for stray directory-level settings that should consolidate. Overlapping or contradicting rules cause silent instruction-following failures.

### Harness Design Decisions for PRDs

| Decision | Options | Trade-off |
|---|---|---|
| Execution environment | Local, sandbox, cloud VM | Security vs latency vs cost |
| State persistence | Filesystem, database, in-context | Durability vs complexity |
| Continuation strategy | Ralph Loop, checkpoint/resume, single-context | Long-horizon capability vs implementation cost |
| Tool set | Fixed tools, code execution, hybrid | Predictability vs flexibility |
| Context management | Compaction, offloading, summarization | Cost vs information retention |
| Verification | Self-eval, test suite, human review | Autonomy vs reliability |

## DYNAMIC WORKFLOWS (MODEL-GENERATED HARNESSES)

Instead of a human pre-building the harness, the model writes its own workflow file (ex: JavaScript) at runtime: a custom harness tailor-made for the task. The workflow orchestrates separate model instances, each with an isolated context window, then synthesizes their outputs. This extends harness engineering from "human designs the scaffold" to "model assembles the scaffold per task."

> Dynamic workflows attack failure modes that a single context window can't escape on its own. The fix is structural (isolated contexts, separate roles), not a better prompt.

### Failure Modes They Address

| Failure Mode | What Happens | Why Isolation Fixes It |
|---|---|---|
| Agentic laziness | Agent stops after partial progress (ex: 20 of 50 items) and declares done | A separate driver loop holds the full task list; workers can't shrink the goal |
| Self-preferential bias | When judging against a rubric, the agent prefers its own prior output | A judge instance with no authorship stake scores neutrally |
| Goal drift | Lossy compaction silently drops edge-case and "don't do X" constraints | Constraints live in the workflow file, re-injected into each fresh context |

### Orchestration Patterns

| Pattern | Shape | Use When |
|---|---|---|
| Classify-and-act | Route input to a handler based on a first-pass classification | Heterogeneous inputs need different handling |
| Fan-out-and-synthesize | Split work across parallel instances, merge results | Independent subtasks; large item sets |
| Adversarial verification | One instance extracts claims, separate instances check each | Output correctness matters more than speed |
| Generate-and-filter | Over-generate candidates, score, keep the best | Quality variance is high |
| Tournament | Candidates compete; pairwise-judged until a winner emerges | Selecting the single best of many |
| Loop-until-done | Re-invoke against a completion goal until criteria met | Long-horizon work that exceeds one context |

Use cases: large migrations/refactors, deep research, deep verification (extract claims → check each), memory/rule adherence (one verifier instance per CLAUDE.md rule; mine sessions for recurring corrections and distill into config), root-cause investigation (independent hypotheses from disjoint evidence to avoid self-preferential bias), and scaled triage. Pair with the quarantine pattern: instances reading untrusted public content are barred from high-privilege actions.

> Caveat: dynamic workflows often use more tokens. Pair with a hard completion requirement (ex: /goal), explicit token budgets ("use 10k tokens"), and save proven workflows as skills so they're reusable rather than regenerated each run.

## SELF-IMPROVING HARNESSES

Dynamic workflows (above) generate per-task harnesses at runtime. Self-improving harnesses accumulate improvements across runs, so each execution benefits from lessons learned in prior ones. This is the mechanism behind Level 4 of the loop stack.

### Self-Harness Pattern

Three-step cycle:

1. **Weakness mining**: analyze traces from recent runs to identify recurring failure patterns (ex: agent consistently fails at a specific tool call sequence, drops constraints during compaction, or misroutes queries)
2. **Harness proposal**: agent generates a concrete change to its own configuration (updated prompt section, new tool, modified routing rule, adjusted compaction strategy)
3. **Proposal validation**: run the proposed change against a regression test suite before accepting. If regressions appear, reject the proposal. Only accept changes that maintain existing pass rates while fixing the identified weakness

Self-Harness formalizes what transcript mining (see above) does manually. The difference: transcript mining requires a human to review corrections and update config. Self-Harness closes the loop with automated validation, allowing unsupervised improvement within the safety boundary of the regression suite.

### Continual Learning Loops

The broader pattern that Self-Harness implements. If you build a good continual learning loop, you can get compounding ~1% daily improvement. The prerequisite: a useful gradient to hill-climb against. Without data, evals, and feedback providing directional signal, auto-improvement loops produce lateral movement or drift, not gains.

Concrete architecture (Moe Ali, Product Faculty):

| Component | What It Does | Cadence |
|---|---|---|
| Automated cron | Scheduled tasks and heartbeat checks | Continuous |
| Memory | Accumulated context about preferences, audience, past performance | Updated per run |
| Self-review | Each agent reviews its own work against criteria | Monthly |
| Supervisor review | A supervisor agent reviews its squad, grades performance, proposes config changes | Bi-weekly |
| Escalation | Agents do work, human makes final decision on high-stakes outputs | Per decision |

> Auto-research style proposal loops work best only when Data/Evals/Feedback give a useful gradient to hill-climb against. Without that gradient, the loop is just busy.

The escalation layer is critical: agents do work, but humans retain decision authority on anything consequential. This prevents compounding errors from auto-accepted bad proposals.

## BACKGROUND CODING AGENTS

Agents that work autonomously in the cloud without user-initiated sessions. The user triggers a task (or a system event does), the agent executes in a sandboxed environment, and results appear when done.

Examples (as of May 2026): Stripe Minions (leveraged years of internal platform tooling), Ramp Inspect (dedicated engineering team built the infrastructure). Both demonstrate that the infrastructure layer is the hard part, not the model capability.

### Build vs. Buy Considerations

| Factor | Build | Buy |
|--------|-------|-----|
| Existing platform tooling | Strong (like Stripe): amortize investment | Weak: vendor gets you running faster |
| Security requirements | Custom sandboxing, credential management | Vendor handles isolation but you trust their boundary |
| Agent customization depth | Full control over harness, memory, tool set | Constrained to vendor's abstractions |
| Team investment | Full dedicated eng team (Ramp's approach) | Integration team, not platform team |

Resource: background-agents.com (maintained by Ona) tracks the vendor landscape.

> Background agents require the same harness primitives as interactive agents (sandbox, filesystem, memory, orchestration) plus: job scheduling, async result delivery, credential vaulting, and cost controls for unattended execution.

## COMMON FAILURE MODES

| Failure | Symptom | Fix |
|---|---|---|
| Tool misuse | Wrong tool or wrong params | Better descriptions, fewer tools |
| Parameter hallucination | Model fabricates input values rather than asking user (ex: user says "check my order," model calls lookup_order with invented order ID) | Explicit "do not fabricate" in param descriptions; server-side validation; require confirmation for inferred values |
| Infinite loops | Agent retries same action | Step limits, loop detection |
| State confusion | Agent forgets what it learned | Explicit state tracking, context mgmt |
| Harness overfitting | Agent breaks when tool logic changes | Test with modified tool implementations; don't assume model generalizes |
| Context overflow | Agent reasoning degrades mid-task | Tool call offloading, compaction triggers, shorter tool outputs |
| Premature exit | Agent stops before task is complete | Ralph Loop or continuation hooks; plan file with completion criteria |
| Self-preferential bias | Agent rates its own output highest when judging against a rubric | Use a separate judge instance with no authorship stake (dynamic workflows) |
| Goal drift | Compaction drops edge-case and "don't do X" constraints mid-task | Keep constraints in a workflow/plan file, re-inject into each fresh context |

→ See: MCP (protocol layer for tool connections)
→ See: Agent Skills (skill system, knowledge management)
→ See: Context Engineering (context management strategies)

---

**Sources:**
- Eugene Yan, "How to Work and Compound with AI" (May 2026)
- Viv (model-harness co-evolution, May 2026)
- Product Faculty AI PM Course (May 2026)
- Anthropic, "Dynamic Workflows in Claude Code" (Thariq Shihipar & Sid Bidasaria, June 2026)
- AGENTIC Twitter List digests, Jun 21-Jul 3 2026 (4-level loop stack, self-harness, continual learning)
- @sydneyrunkle (Jun 2026): 4-level loop stack
- @hwchase17 (Jun 2026): self-harness pattern
- @Vtrivedy10, @ProductFaculty (Jun 2026): continual learning loops

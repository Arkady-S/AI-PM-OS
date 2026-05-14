How LLMs act in the world: function calling, pipelines, and agent loops

**May 2026**

## CORE CONCEPTS

The model proposes, your app executes. The LLM outputs structured JSON saying "call this function with these arguments." Your code runs it and returns the result. The model never directly executes anything.

Agent reliability scales with constraint. Narrow, well-defined scope is testable and shippable. Broad autonomous agents are impressive demos and production nightmares.

## TOOL DESIGN PRINCIPLES

| Principle | What It Means | Example |
|---|---|---|
| Clear scope | Each tool does one thing | search_orders not search_everything |
| Descriptive name | Name indicates function | get_customer_by_email not lookup |
| When-to-use Description | Says when | "Use when user asks about order status" |
| Explicit params | Required vs optional clear | order_id (req), include_history (opt) |
| Structured errors | Errors model can interpret | {error: order_not_found, suggestion: ...} |

## PIPELINE VS. AGENT

### PIPELINE (default)

- Static sequence of steps
- Predictable and debuggable
- Same flow for all queries
- Can enumerate all paths

### AGENT (when necessary)

- Model decides next action
- Next step depends on results
- Different queries, different paths
- Flexible but harder to test

Default to pipeline. Only use agent orchestration when you genuinely can't predict the flow. Most "agent" use cases are better served by pipelines with a few branch points.

## KEY PRINCIPLES

> Fewer tools is better. Every tool is context overhead that burns tokens. Start with 3-5 core tools; add based on observed need, not anticipated need.

> Frameworks for prototypes, custom for production. LangChain for exploration; production usually ends up with custom orchestration.

> State management is the hard part. Most agent failures trace to state bugs, not model capability. What persists across turns?

## THE ORCHESTRATION LOOP

The fundamental cycle for any agent or multi-step system:

### 1. OBSERVE

Read current state: user input, tool results, conversation history

### 2. THINK

Model decides what to do next: call a tool, ask for clarification, or respond

### 3. ACT

Execute the chosen tool or produce output

### 4. REPEAT

Feed result back into context. Continue until termination condition met

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

### Harness Design Decisions for PRDs

| Decision | Options | Trade-off |
|---|---|---|
| Execution environment | Local, sandbox, cloud VM | Security vs latency vs cost |
| State persistence | Filesystem, database, in-context | Durability vs complexity |
| Continuation strategy | Ralph Loop, checkpoint/resume, single-context | Long-horizon capability vs implementation cost |
| Tool set | Fixed tools, code execution, hybrid | Predictability vs flexibility |
| Context management | Compaction, offloading, summarization | Cost vs information retention |
| Verification | Self-eval, test suite, human review | Autonomy vs reliability |

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
| Infinite loops | Agent retries same action | Step limits, loop detection |
| State confusion | Agent forgets what it learned | Explicit state tracking, context mgmt |
| Harness overfitting | Agent breaks when tool logic changes | Test with modified tool implementations; don't assume model generalizes |
| Context overflow | Agent reasoning degrades mid-task | Tool call offloading, compaction triggers, shorter tool outputs |
| Premature exit | Agent stops before task is complete | Ralph Loop or continuation hooks; plan file with completion criteria |

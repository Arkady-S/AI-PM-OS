Last updated: July 2026

Curating the right tokens for every model call

## CORE CONCEPT

Context engineering is curating what information enters a model's limited attention budget at each step. Unlike prompt engineering (writing good instructions), context engineering is iterative and happens every time you decide what to pass to the model.

Most AI failures trace back to context failures, not model failures. The model didn't hallucinate because it's dumb; it hallucinated because it didn't have the right information.

Context is the only durable moat. Competitors can buy the same model via API. They can't replicate your domain knowledge graph, user memory, orchestration logic, or environment signals.

## 9 KEY PRINCIPLES

1. **Prompting = interface design**
   Reliability comes from explicit task framing, constraints, and a tight output contract. Treat prompts like API contracts, not natural language requests.

2. **Context is a finite budget**
   Every token has cost (latency, money, attention). Packing order: non-negotiables > task-critical facts > examples > history > drop the rest.

3. **Instruction hierarchy = conflict resolution**
   System prompt > Developer prompt > User prompt. When instructions conflict, the model needs explicit priority.

4. **Structure beats cleverness**
   XML tags and labeled delimiters reduce model guesswork. A well-structured mediocre prompt outperforms a clever unstructured one.

5. **Examples are the fastest multiplier**
   One strong input-output example often outperforms paragraphs of instructions. Show, don't tell.

6. **Bad behavior = missing spec**
   Fix is usually: tighten constraints, select better context, or add eval-style tests. Debug the spec, not the model.

7. **Prompts are code**
   Version and test accordingly. Maintain a golden prompt set: curated scenarios that must never regress.

8. **Context rot is real**
   More tokens != better answers. Recall accuracy degrades as context grows. Aggressive curation beats throwing everything in.

9. **Design for missing context**
   When required context is unavailable, what's the behavior? Block, caveat, clarify, or fallback? Product decision, not edge case.

## THE 6-LAYER CONTEXT PYRAMID

Every production AI product has a hierarchy of context. These six layers build on each other:

### L1: Intent Context
**Triple-I Framework**

Interpret explicit text into structured objective. Infer hidden meaning from recent behavior. Identify gaps the system must fill before reasoning.

### L2: User Context
**5-P Personalization Matrix**

Preferences (tone, depth, voice). Patterns (recurring edits, formatting habits). Proficiency (auto-adjust complexity). Pacing (consumption speed). Purpose (role-specific goals).

### L3: Domain Context
**Five Structural Pillars**

Entities (objects in your world). Attributes (metadata). Relationships (depends on, caused by). Definitions & Rules (formulas, business logic). Lineage (version history, provenance).

### L4: Rule Context
**Two-Wall Framework**

Soft Wall: advisory constraints via prompting (tone, brand, style). Hard Wall: mandatory constraints via code (schemas, validation, permissions). Never rely on prompts alone for hard rules.

### L5: Environment Context
**N.O.W. Awareness Model**

Nearby Activity (current file, selection, cursor). Operational Conditions (logs, errors, system state). Window of Time (deadlines, timestamps, recency signals).

### L6: Exposition Context
**Context Distillation Loop**

Collect signals from L1-L5. Compress (remove noise, collapse redundancy). Construct (labeled sections, clear boundaries). Constrain (rules, schemas, safety). Check (validate consistency).

## C.E.O. FRAMEWORK

The pyramid tells you what context to include. C.E.O. tells you how to build it at runtime.

### Capture

Collect raw signals from user behavior, app state, domain artifacts. Three input families: explicit (prompts, selections), implicit (scroll patterns, cursor behavior), system-generated (timestamps, metadata, analytics freshness).

### Enrich

Convert raw signals into structured, model-ready representations. Extract entities, identify relationships, normalize fields, resolve ambiguity ("this issue", "the latest version"), stitch related objects, augment with domain knowledge.

### Orchestrate

Decide which context the model receives, in what order, at what detail level. Balance four forces:

> Relevance: model sees only what affects the request

> Brevity: fits within token limits without losing meaning

> Precision: clearly structured so the model can use it

> Timing: right information at right moment in multi-step flows

## 4D CONTEXT CANVAS (PRD FRAMEWORK)

Use when speccing any AI feature to define context requirements:

### D1: Demand - What is the model's job?

Translate fuzzy requirements into precise job specs. Define: inputs, assumptions, required outputs (format, structure, tone), and success criteria.

**AVOID:** Draft a status update.

**BETTER:** Summarize key changes in project X since last report, structured for stakeholder Y, using user's preferred tone, adhering to reporting format.

### D2: Data - What context is required?

Build a Context Requirements Table for each data source:

| Data Needed | Source | Availability | Sensitivity |
|-------------|--------|--------------|-------------|
| Project updates | Project DB | Always | Internal |
| User prefs | User profile | Always | PII |
| Hist. velocity | Analytics API | Sometimes | Internal |
| Stakeholder ctx | User input | Sometimes | None |

Availability levels: Always (100%), Sometimes (depends on freshness/actions), Never (must explicitly request).

### D3: Discovery - How do we get context?

Define retrieval strategy per data requirement:

> Search-based retrieval: when semantic similarity needed (may miss exact)

> Graph traversal: following relationships (requires structured graph)

> Precomputed context: high-latency queries (may be stale)

> Real-time fetch: freshness critical (adds latency)

For each source, decide: real-time vs. precomputed? Cache strategy? Latency budget?

### D4: Defense - How do we handle failures?

Design failure handling before building:

> Pre-checks: enough context? Entities missing? Data too old?

> Post-checks: followed constraints? Logically consistent? Matches schema?

> Fallback paths: partial answer with caveats, clarifying questions, conservative defaults

> Feedback loops: explicit ratings, implicit behavior (undo, edits), cross-mistake patterns

## MEMORY ARCHITECTURE

The 6-layer context pyramid defines what to include in a single call. Memory architecture defines what persists across calls and sessions. Six levels, each adding complexity:

| Level | Type | What It Does | Complexity |
|---|---|---|---|
| 1 | Buffer | Appends all prior messages to each request | Simplest; grows unbounded |
| 2 | Summary | Compresses earlier conversation into summaries | Manages growth; loses detail |
| 3 | Cross-session | Persistent storage with retrieval strategy | Requires storage + retrieval design |
| 4 | Entity | Structured profiles (people, projects, companies) with relationship tracking | Requires schema + entity resolution |
| 5 | Collective/shared | Patterns learned across users and teams | Requires aggregation + privacy controls |
| 6 | Agent | Memory boundaries between multiple cooperating agents | Requires scoping rules per agent |

> Misremembering is worse than not remembering. Wrong memory erodes trust faster than no memory. This is the "uncanny valley of memory": surfacing incorrect context feels worse to users than a blank-slate interaction.

### Memory Scoring at Runtime

When the memory store grows large, not everything can load into context. Score memories across four dimensions to decide what surfaces:

| Dimension | What It Measures | Example |
|---|---|---|
| Recency | How recently the memory was created or updated | Last week's project context > last year's |
| Source gravity | Authority level of the memory's origin | CEO directive > one-time customer feedback |
| Context specificity | Relevance to the current department or task | Sales context stays out of engineering queries |
| Conflict resolution | Whether the memory contradicts other memories | Flag conflicting entries for user-mediated resolution |

Design decision: always-load memories (under 2,000 tokens, apply to nearly every task) vs. on-demand retrieval (searched when task matches a known pattern). Keep the always-load set small.

### Functional Memory Taxonomy (Cognitive Science)

Complementary to the structural 6-level taxonomy above. Classifies memory by what kind of knowledge it represents, not where it lives:

| Type | Contains | Example | Impact |
|---|---|---|---|
| Semantic | Facts about the world, domain knowledge | "ClickUp uses a Space > Folder > List hierarchy" | Grounds reasoning in correct facts |
| Episodic | Records of past interactions and outcomes | "Last time user asked for a report, they wanted PDF" | Personalizes behavior over time |
| Procedural | Instructions, skills, tool-use rules | "When creating a task, always check for duplicate titles first" | Drives most visible quality improvements |

Procedural memory drives the most visible quality gains in practice. It directly shapes agent behavior, while semantic and episodic memory primarily inform reasoning. Implication: invest in capturing and refining procedural memory (skills, rules, tool-use patterns) before scaling episodic or semantic stores.

Three-step update loop: capture traces > analyze for patterns > update the relevant memory type. Design principles: not everything should be a memory update (be selective), ensure future runs actually read the update (test retrieval), protect important behavior with evals before overwriting.

### Wiki Memory

A named pattern converging across the agent ecosystem. Instead of retrieving raw chunks at query time (RAG), an agent precomputes and maintains a synthesized knowledge layer from raw sources (code, logs, Slack threads, transcripts, notes). The wiki is compact, persistent, and agent-readable.

| Implementation | Approach | Update Mechanism |
|---|---|---|
| OpenWiki (LangChain) | CLI generates repo wiki, connects to coding agents via CLAUDE.md/AGENTS.md reference | Scheduled GitHub Action diffs recent commits, updates relevant wiki pages |
| Operational Language Wiki (Hasura) | Captures delta between team's internal "dialect" and LLM's trained "world language." Markdown + JSON | Coding agents maintain |
| PaperWiki | Obsidian-backed knowledge base, indexed via full-text + semantic search | Agents on a loop ingest from multiple sources |
| Context Graph (Hyperspell) | Every company source feeds a conflict-resolved graph; wikis render on top | Auto-updates every 15 minutes |

> Wiki memory is great right up until the wiki is confidently wrong and every agent inherits the same bad assumption. Storing was never the hard part; trusting it is.

Mitigation: hybrid architecture pairing a knowledge graph (for the agent) with a bi-directionally synced wiki (for human readability and audit). Graph provides structured relations; wiki provides natural-language summaries. Edits to either sync back to the other.

RAG retrieves raw chunks. Wiki memory retrieves precomputed synthesis. They are complementary: use RAG for specifics that change too fast for wiki maintenance; use wiki for stable, high-level understanding that would be expensive to re-derive every call.

### Sleep-Time Compute

Background process that analyzes agent trajectories after execution to update a persistent memory store. Named by Letta (Sarah Wooders): the next scaling axis for intelligence after train-time and inference-time compute.

Three-step implementation:

1. **Trace**: capture full agent trajectory (tool calls, reasoning, outcomes) to an observability platform
2. **Analyze**: run an analysis engine (can be a cheaper model) over traces to identify patterns, errors, user preferences, and recurring failure modes
3. **Update**: write extracted insights to a persistent memory store that future runs read at startup

The pattern separates the "doing" agent from the "learning" agent. The doing agent runs in real time. The learning agent runs asynchronously on cheaper models, processing traces in bulk. This avoids contaminating real-time context with learning overhead.

Key design consideration: memory built on traces stays close to where the run happened and where data lives, reducing drift from reality. Watch for stale memory outliving the context it was true in. Apply the same context rot principle (Principle 8) to memory updates.

## TOKEN PRIORITY STACK

When filling context, pack in this order:

1. **Non-negotiables**
   Guardrails, output contract, hard constraints

2. **Task-critical facts**
   Goal, definitions, entities, relationships

3. **Examples**
   One excellent example beats five okay ones

4. **History**
   Only what changes the answer

5. **Everything else**
   Drop or summarize

## CONTEXT QUALITY CHECKLIST

Run before every LLM call:

### Relevance
- [ ] Every piece directly contributes to answering intent
- [ ] Removed "kind of related" but non-essential content
- [ ] Stripped decorative metadata that confuses the model

### Freshness
- [ ] Timestamps recent enough for this task
- [ ] Metrics, logs, dashboards updated
- [ ] Cached artifacts still valid

### Sufficiency
- [ ] All entities needed for reasoning included
- [ ] Related objects (dependencies, history) provided
- [ ] Enough context to avoid hallucinating missing links

### Structure
- [ ] Broken into clean sections with clear labels
- [ ] Relationships explicitly described, not implied
- [ ] Domain knowledge structured, not dumped as text

### Constraints
- [ ] Business rules embedded explicitly
- [ ] Tone, formatting, domain rules included
- [ ] Permission logic represented accurately

## COMMON FAILURE MODES

| Failure | Symptom | Fix |
|---------|---------|-----|
| Token overload | Inconsistent behavior, ignored instructions | Priority stack, aggressive curation |
| Semantic-only search | Irrelevant chunks distort reasoning | Hybrid search, domain structure |
| Prompts as rule enforcement | Model eventually ignores soft constraints | Hard walls in code, not prompts |
| Context as afterthought | Weak AI that can't grow | Define schemas and structure upfront |
| Mixing raw and enriched context | Contradictions and hallucinations | Consistent enrichment pipeline |
| No provenance tracking | Can't debug or explain reasoning | Track source of every context piece |
| Trusting model to infer structure | Unpredictable behavior | Explicit schemas, labeled sections |

→ See: Prompt Engineering (behavioral framing layer)
→ See: RAG (retrieval context)
→ See: Evals & Observability (quality measurement)

---

**Sources:**
- Anthropic + Product Faculty research (2026)
- Muhammad Umer Farooq, Flow/Brain Chain (May 2026)
- AGENTIC Twitter List digests, Jun 21-Jul 3 2026 (wiki memory, sleep-time compute, cognitive science taxonomy)
- @jakebroekhuizen, LangChain memory guide (Jun 2026): functional memory taxonomy
- @hwchase17, @BraceSproul, @tanmaigo, @omarsar0, @eddzsh, @SydSachar (Jun-Jul 2026): wiki memory implementations
- @hwchase17, @sarahwooders (Jun 2026): sleep-time compute

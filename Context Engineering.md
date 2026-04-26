Curating the right tokens for every model call

**March 2026**

Based on Anthropic + Product Faculty research

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

## COMMON FAILURE MODES

**"Just put everything in the prompt"**
Token overload, inconsistent behavior. Fix: priority stack, aggressive curation.

**Relying solely on semantic search**
Irrelevant chunks distort reasoning. Fix: hybrid search, domain structure.

**Prompts as rule enforcement**
Model eventually ignores soft constraints. Fix: hard walls in code, not prompts.

**Context as afterthought**
Weak AI that can't grow. Fix: define schemas and structure upfront.

**Mixing raw and enriched context**
Contradictions and hallucinations. Fix: consistent enrichment pipeline.

**No provenance tracking**
Can't debug or explain reasoning. Fix: track source of every context piece.

**Trusting model to infer structure**
Unpredictable behavior. Fix: explicit schemas, labeled sections.

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

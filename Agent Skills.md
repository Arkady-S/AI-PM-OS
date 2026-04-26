Patterns for building reusable agent instructions across any platform

**April 2026**

Based on Anthropic best practices, LangChain skill benchmarking (2026)

## WHAT IS A SKILL?

A skill is a folder of instructions that teaches an AI agent how to handle specific tasks. Instead of re-explaining workflows every session, you teach the agent once and reuse it consistently.

### Folder Anatomy

```
your-skill/
  SKILL.md              # Core instructions (required)
  scripts/              # Executable code
  references/           # Docs loaded on demand
  assets/               # Templates, fonts, icons
```

### Three Principles

> Progressive disclosure: metadata always loaded, body on match, linked files on demand

> Composability: skills work alongside each other, not in isolation

> Portability: create once, run across surfaces without modification

## SKILL CATEGORIES

The best skills fit cleanly into one category. Nine recurring types:

1. **Library & API Reference**
   Internal libs, CLIs, SDKs: edge cases, gotchas, reference snippets

2. **Product Verification**
   Drive the running product to verify code; pair with browser automation, etc.

3. **Data & Analysis**
   Connect to data/monitoring stacks; IDs, field names, query patterns

4. **Business Automation**
   Multi-tool workflows in one command: standups, tickets, recaps

5. **Scaffolding & Templates**
   Generate framework boilerplate with natural-language requirements

6. **Code Quality & Review**
   Enforce org code standards; deterministic scripts for robustness

7. **CI/CD & Deployment**
   Commit, push, deploy safely; babysit PRs, gradual rollouts

8. **Incident Runbooks**
   Symptom to investigation to structured report, multi-tool

9. **Infrastructure Ops**
   Safety-gated cleanup & maintenance: orphans, cost investigation

## PLAN BEFORE YOU WRITE

### Define 2-3 Concrete Use Cases

**Use Case: Sprint Planning**

- Trigger: "plan this sprint", "create sprint tasks"
- Steps: fetch status > analyze velocity > suggest priorities > create tasks with estimates
- Result: Fully planned sprint with tasks created

### Define Success Criteria

> Triggers on 90%+ of relevant queries (test 10-20 prompts)

> Completes workflow in target number of tool calls

> Users don't need to prompt the agent about next steps

> Consistent results across sessions (run same request 3-5x)

## WRITING EFFECTIVE SKILLS

### The Description Field = Trigger Logic

The agent scans descriptions to decide which skill to load.

**Structure: [What it does] + [When to trigger] + [Key capabilities]**

**AVOID:**
```
Helps with projects.
```

**BETTER:**
```
Manages sprint planning and task creation. Use when user says 'sprint', 'plan tasks', or 'create tickets'.
```

### Don't State the Obvious

Agents already know how to code. Focus on information that pushes the agent out of its defaults: org conventions, internal API quirks, design taste, domain knowledge.

### Build a Gotchas Section

Highest-signal content in any skill. Accumulate failure points over time. Add a line each time the agent trips on something.

```
## Gotchas
- Proration rounds DOWN, not nearest cent
- test-mode skips invoice.finalized hook
- Idempotency keys expire after 24h
- Refunds need charge ID, not invoice ID
```

### Avoid Railroading

Give information + constraints, not rigid step sequences. Skills are reusable; overly specific instructions break in edge cases.

**AVOID:**
```
Step 1: Run git log. Step 2: Run cherry-pick. Step 3: If conflicts, run git status...
```

**BETTER:**
```
Cherry-pick onto a clean branch. Resolve conflicts preserving intent. If it can't land cleanly, explain why.
```

### Use Progressive Disclosure

> SKILL.md = core instructions only. Detailed docs go in references/

> Tell the agent what files exist; it reads them when needed

> Include template files in assets/ for the agent to copy and fill

### Store Scripts, Not Boilerplate

Give the agent composable code (helper functions, query libraries) so it spends turns on composition rather than reconstruction. Think of it as providing building blocks.

### Think Through Setup

Store user-specific config (ex: Slack channel, project ID) in config.json. If missing, prompt the user on first run. Cache it.

### Memory & State

Skills can persist data between runs: append-only logs, JSON, or SQLite. Ex: standup skill keeps a log so the agent diffs what changed since yesterday. Store in a stable folder that survives skill upgrades.

## AGENT KNOWLEDGE MANAGEMENT

A skill tells an agent how to do something. A knowledge base tells it what it has already learned. Without persistent knowledge, every session starts from zero: the agent re-discovers the same patterns, re-makes the same mistakes, and can't build on prior work. The difference between a stateless tool-caller and a capable assistant is accumulated context.

Based on Isaac Flath's open-source agentkb system (April 2026).

### The Knowledge Management Problem

Agents interact with codebases, conversations, documents, and tool outputs across sessions. Useful patterns, domain facts, user preferences, and successful approaches emerge during those interactions but are lost when the session ends. The agent can't see things it has learned or done before, so it can't do the same kinds of things quicker.

Skills solve the "how" problem (reusable instructions). Knowledge management solves the "what" problem (accumulated facts and context the agent draws on at runtime).

### Four Knowledge Types

1. **Agent Chats**
   Prior conversation logs, distilled into structured takeaways. Not raw transcripts (too noisy, too many tokens). Extract: decisions made, approaches that worked, corrections received, domain facts surfaced.

2. **External Content**
   Curated posts, articles, documentation the agent should reference. Indexed for retrieval, not dumped wholesale. The agent pulls relevant items when a task matches, rather than loading everything into context.

3. **Wiki / Domain Knowledge**
   Stable facts about the domain: internal terminology, org conventions, architectural decisions, product rules. Updated infrequently. High retrieval priority because these facts apply across many tasks.

4. **Skills**
   The existing skill system (SKILL.md, scripts, references). Knowledge management wraps around skills by tracking which skills were used, when, and how effectively.

### Architecture Pattern

```
agent-kb/
  chats/            # Distilled session logs
  content/          # Indexed external content
  wiki/             # Domain knowledge entries
  skills/           # Existing skill folders
  index.json        # Retrieval index across all types
```

The key design choice: the agent reads from the knowledge base at session start (or on-demand during a task) and writes back to it at session end. This creates a feedback loop where each session makes the next one better.

### Distillation Over Accumulation

Raw logs grow without bound and degrade retrieval quality. The high-value pattern is distillation:

> After each session, extract structured takeaways into the knowledge base

> Periodically consolidate: merge duplicates, resolve contradictions, prune stale entries

> Maintain a MEMORY.md (or equivalent) that loads every session with the most important persistent context

Isaac Flath's approach: daily session logs capture raw notes (what happened, what was drafted, what feedback came in). Important patterns get promoted into MEMORY.md, which loads automatically every session. The correction never needs to be given again.

### Retrieval at Runtime

The knowledge base is only useful if the agent can find relevant entries when it needs them. Two retrieval strategies:

> **Always-load**: MEMORY.md and core wiki entries load at session start. Keep this small (under 2,000 tokens). These are the facts that apply to nearly every task.

> **On-demand**: Agent searches the knowledge base when a task matches a known pattern. Triggered by keyword overlap, entity matching, or explicit skill reference. Returns top-K results ranked by relevance and recency.

### Relationship to Skills

Skills and knowledge are complementary:

| Dimension | Skill | Knowledge Entry |
|---|---|---|
| Purpose | How to do a task | What the agent knows |
| Lifetime | Stable across sessions | Grows and updates over time |
| Loading | Triggered by user request | Loaded at start or retrieved on demand |
| Authoring | Written by humans | Written by humans or extracted from agent sessions |
| Structure | SKILL.md + scripts + refs | Structured entries in a searchable store |

A mature agent system uses both: skills define repeatable workflows, knowledge entries provide the context those workflows operate on.

## WORKFLOW PATTERNS

1. **Sequential Orchestration**
   Multi-step processes in fixed order. Explicit dependencies, validation gates at each stage, rollback on failure.

2. **Multi-Service Coordination**
   Workflows spanning multiple tools/APIs. Phase separation, data passing between services, validate before advancing.

3. **Iterative Refinement**
   Output quality improves with loops. Draft > validate > fix > re-validate. Repeat until quality threshold met.

4. **Context-Aware Tool Selection**
   Same outcome, different tools by context. Decision trees (file size, type, collab needs) with fallbacks.

5. **Domain-Specific Intelligence**
   Embed specialized knowledge beyond tool access: compliance rules, audit trails, governance checks.

## TESTING

### Three Testing Layers

1. **Triggering tests**
   - Loads on obvious requests? Paraphrased requests? Quiet on unrelated?

2. **Functional tests**
   - Valid outputs, API calls succeed, edge cases handled

3. **Performance comparison**
   - With vs. without: message count, tool calls, tokens, error rate

### Iteration Signals

#### UNDERTRIGGERING

Skill doesn't load when it should. Users manually invoke it.

**Fix:** Add more trigger phrases and keywords to description.

#### OVERTRIGGERING

Skill loads for irrelevant queries. Users disable it.

**Fix:** Add negative triggers, narrow scope, clarify boundaries.

#### EXECUTION ISSUES

Inconsistent results, API failures, user corrections needed.

**Fix:** Improve instructions, add error handling, bundle validation scripts.

### Quick Iteration Method

Iterate on a single hard task until the agent succeeds, then extract the winning approach into the skill. This uses in-context learning for faster signal than broad test suites. Expand to multiple test cases only after the foundation works.

### Evaluation Pipeline (LangChain Benchmarking Patterns)

LangChain's skill benchmarking found: Claude Code with skills completed tasks 82% of the time vs 9% without skills. The delta validates that skill investment has outsized returns on agent performance.

**Clean environment is non-negotiable.** Coding agents are sensitive to starting conditions. Claude Code explores the working directory before starting, and what it finds shapes its approach. Use Docker scaffolds, Harbor, or equivalent sandboxes to create consistent starting state per eval run. Without this, test results are not reproducible.

**Constrained task design.** Open-ended generation is hard to grade. Having the agent fix buggy code constrains the design space and makes correctness validation simpler: if the resulting code still produces buggy behavior on predefined tests, fail it. Design checks are also less brittle because the agent is primed to use the existing approach rather than inventing from scratch.

**Metrics to track per eval run:**

| Metric | What It Tells You |
|---|---|
| Skill invoked? | Triggering reliability |
| Task steps completed (partial credit) | Separates "total failure" from "almost worked" |
| Turn count | Efficiency; does the skill reduce turns? |
| Wall clock time | Not every turn is equal for measuring efficiency |

**Self-tracing for faster iteration.** Have the agent use a tracing skill to inspect its own traces and summarize what happened. A human reviews the summary, proposes fixes, reruns, and checks results. This is significantly faster than manually reading raw agent trajectories.

### Invocation Reliability

Skills are not always invoked reliably. LangChain found that even with explicit prompting to invoke skills, invocation rate topped 70% on some tasks. On one task to create a LangChain agent, Claude Code never invoked the relevant skill at all.

**Mitigation:** Put invocation guidance in AGENTS.md or CLAUDE.md, since those files load into context reliably every session. Tell the agent how and when to use specific skills. This brought invocation rates to consistent levels.

### Skill Consolidation Trade-offs

The number of skills affects selection accuracy. At ~20 similar skills, Claude Code sometimes called the wrong one. At 12 skills, it consistently called the correct skill.

| Approach | Upside | Downside |
|---|---|---|
| Many small skills | Each loads only relevant content | Agent selects wrong skill; fragmented coverage |
| Few large skills | Consistent loading; content always in context | Agent sees content it doesn't need; context overhead |

Finding the right balance requires testing with your specific skill set. Start consolidated, split only when you observe context overhead degrading performance on specific tasks.

## DISTRIBUTION

### Two Sharing Models

> **Repo-checked:** commit skills into your repo. Works for small teams, few repos. Each skill adds to agent context.

> **Marketplace/registry:** distribute as installable packages. Teams choose which to enable. Better at scale.

### Curation Matters

Bad or redundant skills are easy to create. Gate additions: sandbox folder for experiments, traction, then PR to promote. Track usage via hooks to find popular or undertriggering skills.

### Composing Skills

Skills can reference each other by name. No formal dependency management yet. Keep skills self-contained where possible.

## COMMON MISTAKES

### Verbose Instructions, Buried Priorities

Put critical rules at top. Move reference docs to separate files.

### Ambiguous Language

- "Validate things properly" ✗
- "Verify: name non-empty, member assigned, start date not past." ✓

### Over-Prescriptive Steps

Give intent + constraints, not click-by-click. Let the agent adapt.

### Too Much Context Loaded at Once

Keep SKILL.md under 5,000 words. Progressive disclosure for the rest.

### No Error Handling

Include common errors, causes, solutions. Bundle validation scripts.

### Stale Gotchas

Treat skills as living docs. Update after every new failure mode.

## FRONTMATTER REFERENCE

```yaml
---
name: your-skill-name                    # kebab-case
description: >                           # what + when
  Manages sprint planning and task
  creation. Use when user says
  'sprint', 'plan tasks', 'create tickets'.
license: MIT                             # optional
compatibility: requires-x                # optional
metadata:                                # optional
  author: Your Team
  version: 1.0.0
---
```

### Rules

> name: kebab-case, must match folder name

> description: under 1024 chars, include trigger phrases

> No XML angle brackets in frontmatter (security risk)

> No README.md inside skill folder

## PRE-SHIP CHECKLIST

- [ ] 2-3 concrete use cases defined
- [ ] SKILL.md with valid YAML frontmatter
- [ ] Description has WHAT + WHEN + trigger phrases
- [ ] Gotchas section with known failure points
- [ ] Refs/scripts split out (progressive disclosure)
- [ ] Tested: triggering, output, performance
- [ ] Error handling and troubleshooting included
- [ ] Config/setup flow for user-specific state

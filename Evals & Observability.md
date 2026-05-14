Measuring, maintaining, and scaling AI product quality

**May 2026**

Based on Braintrust, LangChain, and Product Faculty research

## CORE CONCEPT

> Evals turn vibes into data. Without systematic measurement, you're guessing.
> Observability tells you when quality degrades in production. Together they answer: "Is it working?" and "Is it still working?"

> AI fails silently. A bad LLM response looks like a normal response. Unlike a 500 error, quality degradation is invisible without instrumentation.

> Evals are the modern PRD. They transform qualitative product specs into quantifiable targets engineering can optimize against.

> The harness is the moat. Competitors buy the same model. They can't replicate your eval suite, memory architecture, or production feedback loops.

## 9 KEY PRINCIPLES

### 1. Evals are acceptance tests

A golden prompt set with expected outputs must never regress. Run on every prompt change, tool change, model update, or retrieval change.

### 2. Prompts are code

Version, test, rollback. A small prompt tweak can break things in unexpected ways. No unversioned changes in production.

### 3. LLM-as-judge scales evaluation

Use a stronger model to score outputs against explicit rubrics. Calibrate against human ratings quarterly. Watch for biases: models prefer longer outputs.

### 4. Distance from users drives eval rigor

The further your team is from end users and domain expertise, the more structured your eval process needs to be. Vibe checks work only when builders are also users.

### 5. Failing evals are assets

If all your evals pass, you don't know your limits. Failing evals reveal current pain points and identify opportunities on new model releases.

### 6. More evals != better agents

Build targeted evals that reflect desired production behaviors. Blindly adding hundreds of tests creates an illusion of improvement.

### 7. Measure efficiency, not just correctness

Two models solving the same task can behave very differently. Track step ratio, tool call ratio, and latency alongside accuracy.

### 8. Offline evals enable experimentation

Rapid iteration without production risk. Top teams average 12+ experiments per day against curated datasets before deploying.

### 9. User signals complete the loop

Thumbs down, regenerates, task abandonment catch failure modes automated evals miss. Feed production signals back into your eval pipeline.

## EVAL TYPES & WHEN TO USE

| Type | How | Best For | Limits |
|------|-----|----------|--------|
| Exact match | Output == expected | Deterministic tasks | Not for open-ended |
| LLM-as-judge | Model scores vs rubric | Subjective quality | Biases, calibration |
| Human review | Human rates output | Ground truth, calibration | Expensive, slow |
| Behavioral | Does system do X? | Specific capabilities | Tests what you spec |
| Trajectory | Tool call sequence | Agent path validation | Fragile to changes |

Use them together: exact match for deterministic, LLM-as-judge for subjective, human review for calibration, behavioral for capability gates.

## THREE-TIER EVAL SCALING STRATEGY

### Tier 1: Human-Powered

PMs and domain experts review outputs by hand. Define what 'good' looks like. Build initial 50-100 golden set examples.

### Tier 2: Programmatic

Python scripts run deterministic checks against golden set. Format compliance, guardrail adherence. Automated regression detection in CI.

### Tier 3: LLM-as-Judge

Stronger model evaluates qualitative dimensions at scale: helpfulness, relevance, tone. Calibrate against human ratings quarterly.

**Build tiers in order. Tier 3 without Tier 1 has no calibration target.**

## AGENT EVAL METRICS

- **Correctness:** Did the agent complete the task correctly?
- **Step ratio:** Observed steps / ideal steps. Lower is better.
- **Tool call ratio:** Observed tool calls / ideal calls. Lower is better.
- **Latency ratio:** Observed latency / ideal latency. Lower is better.
- **Solve rate:** Expected steps / observed latency. 0 if incorrect. Higher is better.

Use ideal trajectories as baselines. For simple tasks the optimal path is obvious. For open-ended tasks, use the best-performing model seen so far.

## OFFLINE / ONLINE EVAL LOOP

**Offline evals:** Run against curated datasets. Rapid experimentation without production risk. Test prompt, model, and retrieval changes safely.

**Online evals:** Same scoring functions against production logs. Validates offline results and identifies examples worth adding to eval datasets.

**The gap matters:** Offline 0.75 but online 0.3 means your eval set doesn't represent production. Daily: review yesterday's traces, surface new patterns.

## SCORING FORMAT: WHEN TO USE WHAT

All scores normalize to 0-1. The question is binary (0 or 1) vs continuous (0.0-1.0).

### Binary (0 or 1)

Deterministic tasks: tool selection, format compliance, guardrails. Clear pass/fail, no ambiguity. LangChain uses binary correctness for all agent evals.

### Continuous (0.0-1.0)

Subjective quality: helpfulness, tone, completeness. Captures partial credit. Requires rubric calibration against human ratings.

### Separate axes

Score correctness (binary) and quality/efficiency (continuous) independently. LangChain pairs binary correctness with continuous step and latency ratios.

### Practical rules

> Never ask an LLM to output an arbitrary number. Use categorical rubrics (ex: 5-point) mapped to 0-1.

> Binary is the default. Use continuous only when partial credit changes what you'd do next.

> Consistency > precision. A rubric reliably giving 0.6 beats one swinging 0.3 to 0.9.

> Braintrust normalizes all scores 0-1 for temporal comparability (avg 12.8 experiments/day).

## DEEP AGENT EVAL PATTERNS

Agents break the traditional eval assumption that every datapoint is treated identically. Success criteria vary per test case and span trajectory, final response, and state.

### Single-step evals

Constrain the agent loop to one turn. Validate the right tool was called with the right arguments. Fast, cheap, catches regressions at individual decision points. ~50% of practical test cases.

### Full-turn evals

Run the agent end-to-end on one input. Test trajectory (was a tool called at some point?), final response quality, and other state (files created, memory updated). Best for open-ended tasks.

### Multi-turn evals

Simulate realistic user conversations with sequential inputs. Add conditional logic: check output after each turn, fail early if the agent deviates. Set up later turns with appropriate initial state.

### Environment matters

Deep agents need a fresh, clean environment per eval run. Mock API requests (vcr for Python, Hono proxy for JS) to make evals faster and reproducible.

## EVAL TAXONOMY BY CAPABILITY

Categorize evals by what they test, not where they come from. This gives a middle view of agent performance between a single number and individual runs.

| Capability | Test |
|------------|------|
| file_operations | Read, write, edit, ls, grep, glob, parallel invocation |
| retrieval | Search strategies, multi-hop document synthesis |
| tool_use | Selecting right tool, chaining multi-step calls, state tracking |
| memory | Recalling context, extracting preferences, persisting info |
| conversation | Clarifying questions, multi-turn dialogue, correct actions |
| summarization | Context overflow, triggering compaction, info recovery |

## OBSERVABILITY: WHAT TO MONITOR

| Dimension | Metrics | Alert When |
|-----------|---------|------------|
| Latency | p50, p95, p99 response time | p95 exceeds SLA 5+ min |
| Cost | Cost/query, daily spend, tokens | Daily cost >150% forecast |
| Errors | API failures, guardrail triggers | Error rate >X% for 5+ min |
| Quality | Thumbs down, regenerates, abandon | Significant shift from baseline |

**Tracing is non-negotiable for agents.** Capture: input at each step, model output, tool calls, return values. Without tracing, you're guessing at root cause.

## DATA SOURCING FOR EVALS

### Dogfooding

Use your own agent daily. Every error becomes an eval candidate. Trace every interaction so mistakes become regression tests.

### External benchmarks

Pull selected tasks from established benchmarks (ex: Terminal Bench, BFCL) and adapt for your specific agent and domain.

### Handwritten evals

Write focused tests by hand for behaviors you think are important. These 'artisanal' evals often catch what benchmarks miss.

### Production logs

Mine real user interactions for edge cases. Feed production failures back into eval datasets on a weekly cadence.

### Agent session transcripts

Agent transcripts (skill invocations, MCP tool calls, reasoning traces, error recovery sequences) are a distinct data category from production logs. They contain the full decision trajectory, not just inputs and outputs. Cursor uses coding session data for real-time RL. If transcripts flow uncaptured through LLM API calls, the signal is lost permanently.

Capture strategy: persist full transcripts server-side, distill into structured records (tool sequences, success/failure outcomes, time-to-completion, error types), feed into eval pipelines as trajectory-level test cases. Over time, transcript-derived data becomes a proprietary asset for fine-tuning and RL.

**Separate SDK unit/integration tests from model capability evals.** Any model passes plumbing tests, so including them in scoring adds no signal.

## GOLDEN SET STRUCTURE

- **30 common cases:** High-frequency queries representing daily usage
- **15 edge cases:** Boundary conditions, unusual inputs, format variations
- **5 adversarial cases:** Prompt injection attempts, policy violations

Start with 20-50 examples. Grow over time from production signals. Run on every PR, nightly full suite, weekly human calibration.

## ROBUSTNESS & CONSISTENCY

Two systems with identical aggregate accuracy can have very different failure profiles. Separate these dimensions:

### Robustness

Accuracy across input types. A system 95% accurate on CSVs and 60% on PDFs has a robustness problem. Test golden set segmented by input type. Fix: better chunking, format-specific preprocessing.

### Consistency

Same answer for same question across time and phrasing. Run identical inputs multiple times. Fix: temperature reduction, stricter output contracts, caching for identical inputs.

## EVAL STANDARD DRIFT

As models improve, static eval thresholds create a false sense of progress. An output that scored 4/5 six months ago may deserve 3/5 relative to current frontier capabilities. Your dashboards show green while your product falls behind.

### Recalibrate

Every major model update. Quarterly at minimum. When user satisfaction diverges from eval scores. Re-rate past 4/5 and 5/5 outputs with current expectations.

## COMMON FAILURE MODES

| Failure | Symptom | Fix |
|---------|---------|-----|
| "Vibes only" | No systematic measurement | Build golden set first |
| Silent regression | Users complain, metrics fine | Golden set + user signals |
| Eval gaming | Teams optimize scores | Maintain failing evals |
| Offline/online gap | Offline 0.75, online 0.3 | Score production logs too |
| Unfocused evals | No mapping to prod behaviors | Docstring per eval |
| Stale golden set | Same examples for 6 months | Weekly production feedback |

## EVAL QUALITY CHECKLIST

Run before shipping any AI change:

> Golden set passes with no regressions

> Prompts versioned with changelog and rollback trigger

> LLM-as-judge rubrics calibrated against human ratings

> Cost, latency, and quality all within bounds

> Failing evals documented with expected future pass conditions

> Production monitoring covers latency, cost, errors, user signals

> Tracing enabled for all multi-step agent flows

> Eval standards reviewed against current frontier model output

---

**Sources:**
- Braintrust (Goyal, 2025)
- LangChain Deep Agents (2025-2026)
- Product Faculty (2026)

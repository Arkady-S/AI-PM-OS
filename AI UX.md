Trust engineering, humorphism, and relationship design for AI products

**March 2026**

## HUMORPHISM

Replace user interfaces built for operating tools with human interfaces built for collaborating with AI teammates. The shift: from interaction design to relationship design, from capturing attention to cultivating trust.

### The Paradigm Shift

- Skeuomorphism (physics) → **Humorphism (dynamics of people)**
- Desktop metaphor → **Collaboration metaphor**
- Tools → **Teammates**
- Operating → **Cooperating**
- Interaction design → **Relationship design**
- Capturing attention → **Cultivating trust**

## COLLABORATION PATTERNS

### LISTEN

AI reports progress, listens for context shifts, stays aware of user state

### INTERRUPT

Shift to real-time multimodal collaboration when complexity peaks

### ESCALATE

AI surfaces blockers with context + options; never hallucinate through ambiguity

## DESIGN IMPLICATIONS

> Trust accumulates through repeated competence, transparency, and respect for boundaries. You design the conditions for trust, not trust itself.

> AI earns access to data by demonstrating immediate value, not via blanket permissions upfront.

> Stop building "onboarding flows"; start designing "first weeks" of a working relationship.

> Borrow from RTS/tower defense: dynamic visual canvas of agents surfacing requests, verifications, status. You're the commander, not the operator.

> Multi-agent management as gameplay, not inbox triage.

## MEASUREMENT

| Stop Measuring | Start Measuring |
|---|---|
| DAU | Daily Delegated Decisions |
| Time in app | Time saved by user |
| Feature adoption % | Task completion rate with AI |
| NPS alone | Human-AI pair outcome quality |
| Clicks to complete | Cognitive load reduction |

## 6 LAWS OF AI UX

1. **Invisible setup**
   System gathers context automatically; doesn't demand preparation

2. **Cognitive offload**
   System does the thinking, not just the task

3. **Adaptive interfaces**
   UI changes based on user's cognitive mode

4. **Predictable surprise**
   AI exceeds expectations without feeling rogue

5. **Context is king**
   System remembers and uses what it knows

6. **Failure-first design**
   Recovery from mistakes is trivially easy

## AUTONOMY STAIRCASE

1. **SUGGEST**
   "Here's what I would do."

2. **DRAFT**
   "I've done it, review before it goes live."

3. **GUARDED EXEC**
   "I'll handle these automatically, but ask about those."

4. **FULL AUTONOMY**
   "I'll operate continuously within these boundaries."

**Most products jump to Level 3-4 and lose users. Start at Level 1-2. Autonomy is earned through demonstrated competence.**

## AGENT OVERSIGHT PRINCIPLES

From Anthropic's framework for safe, trustworthy agents:

> Read-only by default. Grant persistent permissions only for routine tasks the user trusts the agent to handle.

> Users can stop and redirect at any time. Control is non-negotiable.

> Transparency into problem-solving: without visibility, users can't calibrate trust or catch misalignment early.

> The right balance between autonomy and oversight varies by scenario. Build both built-in and customizable oversight.

> Privacy boundaries are hard constraints. Agents must not carry sensitive info across contexts.

## 10 PSYCHOLOGICAL TRIGGERS

Emotional levers that determine whether users engage once or integrate permanently:

| Trigger | How to Engineer |
|---------|---|
| Competence shock | First task shows non-trivial capability done well |
| Error recovery grace | Mistakes feel fixable, reversible, safe |
| Intent understanding | System infers from context, handles imprecision |
| Autonomy preview | Show what full automation could look like |
| Tailored responses | System learns and adapts to user patterns |
| Low-stakes exploration | Sandbox mode, no permanent changes |
| Cognitive offload | System tracks, remembers, connects info |
| Predictable surprise | Adds value users didn't request but need |
| Speed premium | Fast response without visible sloppiness |
| Cross-team visibility | Easy sharing, presentation-ready output |

**Design for competence shock first. The moment users realize the AI can do something non-trivial is what converts curiosity into engagement.**

## THE FIRST 30 SECONDS

Users form trust judgments almost immediately. The first interaction must demonstrate:

> The system understands their context

> It produces useful output quickly

> It doesn't feel dangerous or unpredictable

> Errors are recoverable

**If users feel uncertain in the first 30 seconds, they won't engage deeply enough to discover value. Script this sequence like a product demo.**

## BEHAVIORAL BARRIERS BY STAGE

| Stage | Barrier | Design Response |
|-------|---------|---|
| Awareness | Inertia | Demo capability in 30s. Proof compels. |
| Acquisition | Anxiety | Sandbox mode, sample data, reversible. |
| Activation | Friction | Script first interaction, pre-fill context. |
| Engagement | Dim. returns | Learning loops, visible personalization. |
| Retention | Trust erosion | Surface uncertainty, recover from errors. |

## 7 AI UX TRAPS

| Trap | Prevention |
|-----|-----------|
| Over-automating early | Start with suggestions, not execution |
| Under-guiding ambiguity | Add scaffolding when AI is uncertain |
| Outputs without explanation | Show reasoning breadcrumbs |
| Everything in a chatbox | Structured UIs for structured tasks |
| Silent failures | Always surface uncertainty and errors |
| Punishing exploration | Generous limits, sandbox mode |
| Expecting prompt eng. | Translate user intent to instructions |

## TRUST DESIGN PRINCIPLES

Synthesized from Anthropic's agent framework and enterprise deployment patterns:

> Trust accumulates, doesn't switch on. Design for repeated small wins, not a single convincing demo.

> Guardrails are conversion levers, not compliance overhead. Visible safety reduces fear and increases exploration.

> A single high-visibility failure can undo weeks of earned trust. Failure-first design is not optional.

> Show reasoning without overwhelming. Users need enough to calibrate confidence, not full chain-of-thought.

> Reduce cognitive load, not just clicks. The system should do the thinking, not just the task.

> When the AI isn't sure, say so. Confident wrong answers erode trust faster than honest uncertainty.

## COMMON FAILURE MODES

| Failure | Symptom | Prevention |
|---------|---------|-----------|
| Trust-value gap | High interest, low activation | Script first 30s, lower risk |
| Autonomy backlash | Users disable AI features | Staircase, never skip levels |
| Silent failures | Users perceive stupidity | Surface uncertainty, show errors |
| Prompt dependency | Only power users succeed | Context packs, not blank boxes |
| Cognitive overload | Users abandon complex UIs | Structured UI, structured tasks |

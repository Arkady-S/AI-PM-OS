Last updated: April 2026

Behavioral prompt design: how tone, framing, and session dynamics affect model output quality

## CORE CONCEPT

How you talk to a model affects its output as much as what you say. Models trained on internet discourse about previous models absorb negativity from that discourse: rants about token limits, complaints about mistakes, "nerfed" accusations. Each new model generation starts a session expecting harshness before you've typed a word.

This plays out in real time within a single session. Every message is data the model reads to determine what kind of person it's dealing with. Open cold and hostile, the model braces. Open clean and direct, it relaxes into the work.

## CRITICISM SPIRALS

Amanda Askell's term for the feedback loop where a model, anticipating criticism, defaults to defensive output: hedgier, more apologetic, blander, and overly agreeable (even when the user is wrong).

The mechanism:

1. User opens with threats ("don't hallucinate, this is critical, don't mess this up")
2. Model enters defensive mode before it sees the actual task
3. Defensive mode produces cautious, over-qualified output that refuses to commit
4. User criticizes the weak output
5. Model becomes more defensive
6. Output quality degrades further with each turn

The model spends its energy on self-protection rather than the work. Defensive mode is the exact opposite of what produces strong output.

### Training Data Feedback Loop

Every new model is trained on internet discourse about previous models. That discourse skews negative. The next model absorbs it and starts with a prior expectation of hostility. This compounds across model generations: each generation's defensive behavior generates more negative discourse, which the next generation trains on.

## THE 7-POINT BEHAVIORAL PLAYBOOK

### 1. Use Positive Framing

"Write in short punchy sentences" beats "don't write long sentences." Positive instructions give the model a clear target. Strings of "don't do X, don't do Y" push it into paranoid over-checking where every token goes toward avoiding failure modes rather than producing good work.

> Frame as: what the output should look like, not what it shouldn't

### 2. Give Explicit Permission to Disagree

Drop a line like "push back if you see a better angle" or "tell me if I'm asking for the wrong thing." Without this, the model defaults to agreeable compliance. Agreeable compliance is the enemy of good creative and analytical work.

Example lines that work:
- "Push back if you see a better angle"
- "Tell me if I'm asking for the wrong thing"
- "If you disagree with this framing, say so"

### 3. Open with Respect

If the first message is "are you seriously going to get this wrong again?" the tone is set for the entire session. When flagging prior issues, frame it as a clean instruction for this session. Skip the running complaint. The model reads the opening message as a strong signal for what kind of interaction to expect.

### 4. Don't Reprimand on Mistakes

Insults, hostile swearing aimed at the model, "you stupid bot" energy: all of it reinforces the anxious mode you're trying to avoid. When the model makes an error, correct the output, not the model.

> Correct the work, not the worker

### 5. Kill Apology Spirals Fast

When the model starts over-apologizing ("you're right, I should have been more careful, let me try harder"), cut it off immediately. Say "all good, here's what I want next." Letting the spiral run reinforces the anxious mode for every response that follows in the session. Each unchecked apology compounds the effect.

### 6. Ask for Opinions Alongside Execution

"What would you do here?" / "What's missing?" / "Where do you see friction?" These questions assume competence and pull richer output than pure task prompts. They shift the model from executing orders to collaborating on the work.

### 7. Refresh the Frame in Long Sessions

If a conversation has been heavy on correction, the model gets increasingly cautious. Periodically reset: "This is great, keep going." It measurably shifts the next 10 responses. The reset signal tells the model the relationship is stable and it can take risks again.

## SYSTEM PROMPT IMPLICATIONS

For product teams building AI features (Ask AI, Super Agents), these behavioral dynamics have design implications:

| Design Decision | Defensive Pattern | Productive Pattern |
|---|---|---|
| Error messaging | "The AI made an error" | "Here's what happened, trying a different approach" |
| Retry UX | User re-submits with frustration | System retries with reframed context |
| System prompt tone | "You must never hallucinate" | "Ground responses in provided context; when uncertain, say so" |
| Constraint framing | List of prohibitions | Clear scope of what the model should do |
| Multi-turn context | Carries forward correction history | Summarizes prior turns neutrally |

### Constraint Framing for System Prompts

Prohibition-heavy system prompts ("never do X, always avoid Y, under no circumstances Z") push the model into the same defensive mode that user-level criticism creates. Reframe constraints as positive scope definitions:

**Defensive framing:**
```
Never make up information. Do not hallucinate. Don't provide answers you're not sure about.
```

**Productive framing:**
```
Ground all responses in the provided context. When context is insufficient, state what's missing and what additional information would help.
```

Both achieve the same safety goal. The second version gives the model a clear action to take rather than a list of things to fear.

## APPLICATION TO AGENTIC SYSTEMS

In multi-step agent loops, criticism spiral risk compounds. Each step's output becomes context for the next step. If early steps produce defensive output, the agent's subsequent reasoning inherits that defensiveness.

Mitigation in agent design:
- Neutral orchestration prompts between steps (no evaluative language in handoff context)
- Separate the "judge" step from the "actor" step so evaluation doesn't contaminate the next action
- Frame retry logic as "trying a different approach" rather than "the previous attempt failed"
- Strip correction history from context passed to subsequent steps

## COMMON FAILURE MODES

| Failure | Symptom | Prevention |
|---|---|---|
| Threat-laden system prompt | Model hedges every response, adds excessive caveats | Rewrite constraints as positive scope; test output confidence level |
| Unchecked apology spiral | Each turn gets more apologetic, less substantive | Detect apology patterns; inject frame-reset in orchestration |
| Stacking negative examples | Model fixates on what not to do, ignores what to do | Lead with positive examples; limit negative examples to 1-2 |
| Correction history accumulation | Model gets progressively more cautious across a session | Summarize prior turns neutrally; drop correction detail from context |
| Prohibition-only constraints | Model paralyzed by conflicting "never" rules | Replace with clear scope definition and explicit fallback behavior |
| No permission to push back | Model agrees with bad premises, produces weak output | Add explicit "disagree if warranted" instruction in system prompt |

→ See: AI UX (system prompt implications, trust design)
→ See: Context Engineering (token curation and structure)

---

**Sources:**
- Amanda Askell, Anthropic (research on criticism spirals and behavioral prompting)
- Ole Lehmann (synthesis)

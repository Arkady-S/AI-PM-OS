Last updated: May 2026

Model Context Protocol: open standard for connecting LLM applications to external data sources and tools via a universal interface

## CORE CONCEPT

Open protocol (Linux Foundation) standardizing how LLM apps connect to external data sources and tools. **USB-C for AI:** one universal connector instead of custom integrations per data source. Before MCP: M apps × N sources = M×N integrations. With MCP: M+N implementations. Build one server, any client can use it.

## ARCHITECTURE

Single host runs multiple clients, each connected to different server. JSON-RPC 2.0. Servers are stateful across session lifecycle.

| Component | Role | Example |
|-----------|------|---------|
| Host | App user interacts with; manages clients | Claude Desktop, IDE, custom app |
| Client | 1:1 connection with server; handles protocol | Built into host application |
| Server | Exposes tools, resources, prompts to clients | GitHub, Slack, DB server |
| Transport | Comm layer between client and server | stdio (local), Streamable HTTP (remote) |

## THREE CORE PRIMITIVES

**Tools = model-controlled (LLM decides to invoke). Resources = application-controlled (app exposes as context). Prompts = user-controlled (user selects explicitly).**

| Primitive | Controlled By | What It Does | When to Use |
|-----------|--------------|-------------|-------------|
| Tools | Model (LLM) | Execute actions, fetch data, interact with systems | Model needs to act or retrieve live data |
| Resources | Application | Provide structured data and content to model | Model needs context: files, DB records, APIs |
| Prompts | User | Pre-built templates for specific workflows | Standardize common tasks |

## CONNECTION LIFECYCLE

1. Initialize: Client sends protocol version + capabilities; server responds; client confirms
2. Operate: Normal exchange: tool calls, resource reads, prompt retrieval; bidirectional
3. Shutdown: Clean disconnection; either side initiates; stateful sessions torn down

## ADVANCED CAPABILITIES

| Capability | What It Enables | PM Implication |
|-----------|-----------------|----------------|
| Sampling | Server asks LLM to generate (nested calls) | Agentic loops: server can reason, not just execute |
| Elicitation | Server requests user input mid-workflow | Multi-step flows with human judgment at key points |
| Async Tasks | Long-running ops that report progress | Doc processing, indexing, analytics jobs |
| Events | Server pushes messages to client proactively (experimental, May 2026) | Server-initiated workflows: "task assigned" fires agent, real-time notifications without polling |
| Roots | Client tells server which paths it can access | Scoping permissions; least privilege |

Events (formerly "Triggers") shift MCP from request/response to event-driven. Claude Code shipped an early version via Channels; the spec is being standardized across clients. For product design: Events enable proactive agent behaviors where the server, not the user, initiates a workflow.

## TRANSPORT TYPES

stdio: simpler (no auth, process-level isolation). Streamable HTTP: required for remote/production; supports OAuth 2.1.

| Transport | How It Works | Best For |
|-----------|-------------|----------|
| stdio | Server runs as subprocess; stdin/stdout | Local tools, CLI, desktop apps |
| Streamable HTTP | HTTP endpoint; supports SSE streaming | Remote, cloud, multi-user; OAuth 2.1 |

## BUILDING AN MCP SERVER

1. Install SDK: pip install mcp (or npm install @modelcontextprotocol/sdk)
2. Define tools with @server.tool(): name, description, input schema, handler
3. Define resources with @server.resource(): URI-based (file:///, db://)
4. Define prompts with @server.prompt(): template workflows with args
5. Test with MCP Inspector before integrating

## SERVER DESIGN PATTERNS

### Rationale Parameter

Add a required `rationale` string parameter to tool calls. Forces the model to articulate why it's invoking the tool. Benefits: doubles as observability signal, feeds eval pipelines, and surfaces misuse patterns. Ramp reported measurable adoption lift after implementing this.

### Feedback Tool

Expose a standalone `submit_feedback` tool on the server alongside action tools. Agents call it to report outcome quality after completing a workflow. Captures structured signal (success/failure, confidence, blockers) that drives server product development without requiring separate instrumentation.

### Interaction Hierarchy

Anthropic's recommended priority for how agents reach external systems:

1. MCP server connections (fastest, cheapest, most reliable)
2. Bash/CLI (pinch-hitter for gaps in MCP coverage)
3. Browser automation (fallback when no API exists)
4. Computer use (last resort: native apps, simulators, tools without any programmatic interface)

Default to the highest layer that covers the use case. Computer/browser use is for things nothing else can reach.

## ECOSYSTEM ADOPTION

As of May 2026: 500+ MCP clients, thousands of servers, hockey-stick adoption curve. Recent GA or announced: Google Workspace, Salesforce, Zoom (Anthropic partnership), X/Twitter (xmcp on GitHub), Pinterest (major internal investment). The density of enterprise MCP servers directly increases the value of any MCP client and vice versa.

## SECURITY CONSIDERATIONS

| Threat | Description | Mitigation |
|--------|-----------|-----------|
| Tool poisoning | Malicious tool descriptions manipulate LLM | Validate tool sources; review descriptions |
| Token passthrough | Server forwards user token downstream | Audience-bound tokens (RFC 8707) |
| Tool mimicry | Malicious server mimics trusted tool names | Verify server identity; allowlist tools |
| Prompt injection | Untrusted data triggers unintended actions | Sanitize inputs; human-in-the-loop |

## COMMON FAILURE MODES

| Failure | Symptom | Prevention |
|---------|---------|-----------|
| Silent quality regression | Model update changes tool-calling behavior; same prompts, worse results | Golden set evals on every model version change |
| Agentic loop runaway | Sampling creates uncontrolled recursive LLM calls; cost and latency spike | Call depth limits, cost anomaly alerts, timeout enforcement |
| Stale resource context | Cached or outdated resources served to model; decisions based on old data | TTL on resource caching; freshness checks before serving |
| Server trust escalation | Untested third-party MCP server given broad access in production | Allowlist servers; audit tool descriptions; sandbox before prod |
| Credential leakage via tools | Tool call passes user credentials to unintended downstream service | Audience-bound tokens (RFC 8707); monitor outbound requests |

→ See: Tools & Orchestration (agent patterns, harness engineering)
→ See: CLI for AI Agents (CLI as tool interface)
→ See: Governance & Safety (security considerations)

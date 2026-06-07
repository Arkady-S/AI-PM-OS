Design principles, output contracts, and safety patterns for agent-consumable command-line tools

**April 2026**

## CORE CONCEPT

CLIs are the new APIs for AI agents. An agent's primary interface to external systems is tool calls, and CLIs map 1:1 to tool schemas: a command name, typed parameters, and structured output. A well-designed CLI is instantly consumable by any agent framework without wrapper code.

Human-friendly and agent-friendly are not the same. Human CLIs optimize for scannability (colors, tables, progress bars). Agent CLIs optimize for parseability (JSON, deterministic schemas, semantic exit codes). The best CLIs serve both: human output by default, machine output via flag.

Every interactive prompt is a dead agent. If your CLI hangs waiting for stdin, the agent's workflow stops. Agents cannot type "y" at a confirmation prompt. Design for non-interactive execution from the start.

## THE DUAL-AUDIENCE PRINCIPLE

Every CLI command serves two consumers. Design for both or one audience can't use it.

| Dimension | Human Consumer | Agent Consumer |
|-----------|---------------|----------------|
| Output format | Colored tables, progress bars | JSON to stdout, messages to stderr |
| Error messages | Friendly prose | Structured: error code + failed param + suggested fix |
| Discovery | --help with prose descriptions | --help with examples + machine-readable schema |
| Confirmation | Interactive y/n prompts | --yes / --no-confirm flags |
| Progress | Spinners, percentage bars | Newline-delimited JSON status events |
| Verbosity | Default verbose | Default quiet; --verbose opt-in |

Implementation pattern: human output is the default. Add --json or --output json to every command. JSON goes to stdout. Human-readable messages, warnings, and progress go to stderr. This lets agents pipe stdout reliably while humans see both streams in a terminal.

## COMMAND STRUCTURE

### Noun-Verb Pattern

The noun-verb pattern (ex: `gh pr create`, `docker container ls`) turns discovery into a deterministic tree search. An agent can enumerate nouns, then enumerate verbs per noun, building a complete capability map without documentation.

**AVOID:**
```
mytool create-new-project --name foo
mytool list-all-projects
mytool delete-project-by-id --id 123
```

**BETTER:**
```
mytool project create --name foo
mytool project list
mytool project delete --id 123
```

### Flag Design

| Principle | What It Means | Example |
|-----------|--------------|---------|
| Explicit over positional | Named flags, not arg position | --id 123 not just 123 |
| Boolean flags are verbs | Flag name states the action | --force, --dry-run, --recursive |
| Consistent types | Same flag name = same type everywhere | --output json across all commands |
| Defaults in help | Show default value in --help | --limit (default: 50) |
| No ambiguous short flags | Short flags collide across commands | Reserve -o, -f, -v for universal meanings |

### Discovery Contract

When an agent encounters an unfamiliar CLI, it runs --help. That output is the tool description, parameter spec, and usage guide combined. Include:

> Command purpose in one sentence (first line)

> All flags with types, defaults, and constraints

> 2-3 concrete examples showing common invocations

> Exit codes and their meanings

For machine-readable discovery, expose a `--schema` flag that returns JSON Schema for all commands. This lets agent frameworks auto-generate tool definitions without parsing help text.

## OUTPUT CONTRACT

### The Structured Output Spec

Every command that returns data must support a --json flag with a consistent schema.

| Rule | Rationale |
|------|-----------|
| Consistent field names across commands | Agent builds expectations; "id" in one command, "identifier" in another breaks parsing |
| Stable schema across versions | Agents hard-code field access; removing a field is a breaking change |
| Envelope pattern for collections | `{"items": [...], "total": N, "has_more": bool}` not bare arrays |
| ISO 8601 timestamps | Agents parse dates; "2 hours ago" is not parseable |
| Null over absent | Missing fields as null, not omitted, so agents don't confuse "not present" with "not returned" |
| Pagination in response | Include cursor/offset in output so agent can request next page |

### Volume Control

Agents have finite context windows. A command that dumps 10,000 lines overflows the context and degrades reasoning. Build volume control into the CLI:

| Mechanism | Implementation |
|-----------|---------------|
| --limit N | Cap result count (default to a sane number, not unlimited) |
| --fields f1,f2 | Return only requested fields |
| --since / --until | Time-bound results |
| --format summary | Condensed output mode for high-level scans |
| Pagination | Cursor-based; return next_cursor in response |

## ERROR CONTRACT

### Semantic Exit Codes

Exit 0/1 is not enough. Agents need to branch on failure type. Define and document a semantic exit code table:

| Code | Meaning | Agent Action |
|------|---------|-------------|
| 0 | Success | Continue workflow |
| 1 | General error | Read stderr, attempt fix |
| 2 | Invalid arguments | Re-read --help, fix params |
| 3 | Authentication failure | Re-authenticate, retry |
| 4 | Resource not found | Check ID, create if needed |
| 5 | Conflict / already exists | Idempotent: treat as success |
| 6 | Permission denied | Escalate to user |
| 7 | Rate limited | Wait, retry with backoff |
| 8 | Timeout | Retry or increase --timeout |

### Structured Error Output

When --json is active, errors must also be JSON. Include three fields minimum:

```json
{
  "error": {
    "code": "RESOURCE_NOT_FOUND",
    "message": "Project with ID 'abc-123' does not exist",
    "param": "project_id",
    "suggestion": "Run 'mytool project list' to see available projects"
  }
}
```

The `suggestion` field is high-signal for agents. It tells the model what command to run next to self-correct, dramatically reducing retry loops and wasted tool calls.

Cloudflare found that replacing HTML error pages with RFC 9457-compliant structured errors reduced agent token consumption by 98%. The same principle applies to CLIs: a structured error with a fix suggestion costs 50 tokens; a stack trace costs 500 and communicates less.

## SAFETY PATTERNS

### The Destructive Action Framework

Any command that creates, modifies, or deletes state must support safety mechanisms:

| Safety Layer | What It Does | Implementation |
|-------------|-------------|----------------|
| --dry-run | Preview changes without executing | Return same output schema with `"dry_run": true` |
| --yes / --no-confirm | Skip interactive confirmation | Required for any agent-driven workflow |
| Idempotency | Same command twice = same result | Return success (exit 0) if already in target state |
| Conflict detection | Distinct exit code for "already exists" | Exit 5, not exit 1, so agent can handle gracefully |
| Undo metadata | Output includes info needed to reverse | `"undo_command": "mytool project restore --id abc-123"` |

### Permission Tiers

Not all commands carry equal risk. Classify commands and enforce accordingly:

| Tier | Risk | Examples | Agent Policy |
|------|------|----------|-------------|
| Read | None | list, get, search, status | Auto-approve; no confirmation |
| Create | Low | create, add, invite | Approve with --yes flag |
| Modify | Medium | update, rename, move | Require --yes; support --dry-run |
| Delete | High | delete, remove, purge | Require --yes + --force; always support --dry-run |
| Admin | Critical | reset, migrate, drop | Require explicit user approval; agents should escalate |

### State Observability

After any write operation, the agent needs to verify the result. Every mutating command should either return the new state in its output, or document which read command confirms the change.

**AVOID:**
```
$ mytool task update --id 5 --status done
OK
```

**BETTER:**
```json
{
  "task": {"id": 5, "status": "done", "updated_at": "2026-04-01T12:00:00Z"},
  "verify": "mytool task get --id 5 --json"
}
```

## AUTHENTICATION

| Pattern | Best For | Agent Implication |
|---------|---------|------------------|
| Environment variable (TOKEN, API_KEY) | CI/CD, containers | Agent inherits env; no interactive flow needed |
| Config file (~/.config/mytool/auth.json) | Developer workstations | Agent reads existing auth; no login flow needed |
| OAuth device flow | First-time setup | Agent can trigger but user must complete in browser |
| API key via flag (--token) | One-off scripting | Agent passes directly; avoid if it means keys in shell history |

> Never require interactive browser-based auth as the only option. Agents operate headlessly. Always support token-based auth via env var or flag as an alternative.

> Auth errors must be distinguishable from other errors (exit code 3, structured error with code AUTH_FAILED) so agents can route to re-authentication rather than generic retry.

## TESTING AGENT COMPATIBILITY

### The Five-Check Validation

Run these checks before shipping any CLI that agents will consume:

1. **Non-interactive execution.** Can every command complete without stdin input? Run the full command set with stdin closed (`< /dev/null`). Any hang is a failure.

2. **Structured output round-trip.** Does `command --json | jq .` parse cleanly for every command? Any command that produces invalid JSON under --json is broken.

3. **Error structure.** Do all error paths produce structured errors under --json? Trigger every known error condition and verify JSON output.

4. **Idempotency.** Run every mutating command twice. Does the second invocation succeed or return a conflict code? Anything that crashes on re-run will break agent retry loops.

5. **Volume bounds.** Does every list/search command respect --limit? Does default output fit within 4K tokens? Unbounded output overflows agent context.

### Agent Integration Testing

| Test | What It Validates | How |
|------|------------------|-----|
| Discovery test | Agent can enumerate all commands | Feed --help output to LLM; ask it to list capabilities |
| Task completion | Agent can accomplish a goal using only the CLI | Give agent a task description and CLI access; measure success rate |
| Error recovery | Agent self-corrects on failure | Inject errors (bad IDs, expired auth); verify agent reads error and adapts |
| Composition | Agent chains multiple commands | Multi-step workflow (create > configure > verify); measure step count vs ideal |

## PRD ELEMENTS

When speccing a CLI that agents will consume, define:

| Element | What You Define | Example |
|---------|----------------|---------|
| Command taxonomy | Noun-verb hierarchy | project (create, list, get, update, delete) |
| Output contract | JSON schema per command | Envelope with items, total, has_more |
| Error contract | Exit codes + error schema | 10-code table + JSON error format |
| Auth model | How agents authenticate | Env var MYTOOL_TOKEN; fallback to config file |
| Safety classification | Per-command risk tier | Read/Create/Modify/Delete/Admin |
| Volume controls | Default limits, pagination | --limit 50 default, cursor pagination |
| Discovery method | How agents learn the CLI | --help with examples + --schema for JSON |
| Versioning strategy | How schema changes ship | Semantic versioning; deprecation warnings before removal |

## DESIGN CHECKLIST

- [ ] Every command supports --json for structured output
- [ ] JSON to stdout, human messages to stderr
- [ ] No interactive prompts; --yes / --no-confirm available
- [ ] Semantic exit codes documented and consistent
- [ ] Errors under --json return structured JSON with suggestion field
- [ ] --dry-run supported on all write/delete commands
- [ ] --limit and --fields available on list/search commands
- [ ] Default output fits within 4K tokens
- [ ] --help includes examples and flag descriptions with types
- [ ] Auth works via environment variable (no browser-only flow)
- [ ] Mutating commands are idempotent or return distinct conflict codes
- [ ] Schema stability: no field removal without version bump
- [ ] Noun-verb command structure for hierarchical discovery
- [ ] All commands pass non-interactive test (stdin closed)
- [ ] ANSI colors/formatting disabled in --json mode

## COMMON FAILURE MODES

| Failure | Symptom | Prevention |
|---------|---------|-----------|
| Interactive prompts | Agent hangs indefinitely | --yes flag on every mutating command; stdin closed test |
| Unstructured errors | Agent retries blindly, burns tokens | Structured JSON errors with suggestion field |
| Unstable output schema | Agent's parsing breaks on update | Schema versioning; treat field removal as breaking change |
| Unbounded output | Agent context overflow, degraded reasoning | Default --limit; --fields filtering; pagination |
| Exit code 1 for everything | Agent can't distinguish auth failure from not-found | Semantic exit codes with documented table |
| Human-only help text | Agent can't determine correct params | Examples in --help; --schema flag for machine-readable spec |
| No dry-run | Agent makes irreversible changes during exploration | --dry-run on all write/delete commands |
| Stateful commands | Agent loses track across invocations | Return full state in output; don't rely on session memory |
| Colors/ANSI in JSON mode | Agent parses escape codes as data | Disable formatting when --json is active |
| Required env setup | Agent fails silently with missing config | Check prerequisites at startup; structured error with setup instructions |

→ See: MCP (protocol layer for tool connections)
→ See: Tools & Orchestration (tool design principles)

---

**Sources:**
- Cloudflare: RFC 9457-compliant error responses for agents (2026)
- Anthropic: Claude Code CLI patterns and Agent SDK documentation
- clig.dev: Command Line Interface Guidelines
- Emerging patterns from gh CLI, docker CLI, and Vercel CLI agent integrations

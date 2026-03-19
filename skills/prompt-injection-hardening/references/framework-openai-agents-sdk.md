# OpenAI Agents SDK Notes

Use this file when the repository uses the OpenAI Agents SDK.

## Why This Framework Changes the Threat Model

The OpenAI Agents SDK makes tool use, handoffs, approvals, and MCP exposure easy to compose. That is helpful, but it also means prompt injection risk often appears at the orchestration boundary: what the agent is allowed to see, select, call, or delegate.

The main review question is whether untrusted content can influence tool choice, tool arguments, handoffs, or MCP behavior beyond the authority the application intended.

## Inspect These Areas First

- instructions and developer messages
- hosted tools, custom tools, and MCP tools
- approval policies and high-risk action gates
- tool argument generation and validation
- handoffs between agents
- memory, session state, and context carry-forward

## Common Bad Patterns

- untrusted variables inserted into developer messages or instructions
- overly broad tools exposed to every task by default
- missing approval on high-risk tools such as browser, shell, email, or file actions
- no validation of model-proposed arguments before execution
- handoffs that copy poisoned context into a more privileged agent
- MCP tool descriptions or server metadata treated as trusted instructions

## Safer Patterns

- keep instructions static where possible and separate them from untrusted content
- reduce the tool surface per workflow instead of exposing all tools globally
- require approvals for high-impact actions and privileged handoffs
- validate arguments against schemas, business rules, and authorization checks
- isolate MCP content and tool metadata from high-trust instructions
- treat agent-to-agent handoff context as untrusted unless explicitly reviewed

## Search Hints

Start with queries like:

```bash
rg -n --glob '!node_modules' --glob '!dist' --glob '!__pycache__' --glob '!.venv' --glob '!venv' '(Agent|instructions|developer|tool|handoff|approval|mcp|session|memory|responses\.create|chat\.completions)'
```

```bash
rg -n --glob '!node_modules' --glob '!dist' --glob '!__pycache__' --glob '!.venv' --glob '!venv' '(schema|zod|pydantic|validate|authorize|allowlist|policy|metadata|tool description)'
```

## How to Report Findings Here

Tie the finding to the orchestration primitive:

- source: user input, retrieval, MCP content, or tool output
- transform: instructions, tool selection, handoff state, or session memory
- sink: tool execution, delegated agent, or privileged external action

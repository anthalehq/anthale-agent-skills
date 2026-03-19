# Vercel AI SDK Notes

Use this file when the repository uses the Vercel AI SDK.

## Why This Framework Changes the Threat Model

The Vercel AI SDK often sits close to route handlers, server actions, streaming UI, and tool orchestration. That means prompt injection risk is frequently tied to server boundaries: which identifiers, scopes, tools, and actions the server lets the model influence.

The main review question is whether model-controlled output crosses into server authority without validation.

## Inspect These Areas First

- `generateText`, `streamText`, and prompt assembly
- tools, tool execution, and MCP-generated tools
- middleware, route handlers, and server actions
- model-selected identifiers, scopes, and resource references
- streaming output handling and downstream side effects
- persistence of chat state, summaries, and retrieved context

## Common Bad Patterns

- model-controlled identifiers or scopes passed directly into server-side actions
- dynamic MCP descriptions exposed without review or trust separation
- missing server-side validation of model-selected actions
- streaming output consumed as if it were safe before validation completes
- route handlers trusting tool output or assistant output as authority
- chat state or summaries replaying poisoned content into later calls

## Safer Patterns

- keep all privilege decisions on the server side
- validate model-selected actions, identifiers, and arguments before execution
- scope tools to the route or workflow that actually needs them
- keep MCP metadata and remote content separate from high-trust instructions
- avoid making security decisions from partial streamed output
- review what gets persisted in chat history, summaries, and task state

## Search Hints

Start with queries like:

```bash
rg -n --glob '!node_modules' --glob '!dist' --glob '!__pycache__' --glob '!.venv' --glob '!venv' '(generateText|streamText|tool|tools|mcp|route\.ts|server action|middleware|useChat|messages|metadata)'
```

```bash
rg -n --glob '!node_modules' --glob '!dist' --glob '!__pycache__' --glob '!.venv' --glob '!venv' '(id|scope|resource|validate|zod|authorization|allowlist|approval|stream|onFinish|onStepFinish)'
```

## How to Report Findings Here

Keep the server boundary explicit:

- source: user content, retrieval, MCP content, or tool output
- transform: prompt assembly, tool selection, or streamed intermediate output
- sink: server action, route handler, external call, or persisted state

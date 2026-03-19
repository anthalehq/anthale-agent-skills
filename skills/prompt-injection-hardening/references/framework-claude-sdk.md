# Claude / Anthropic SDK Notes

Use this file when the repository uses the Anthropic SDK for Claude.

## Why This SDK Changes the Threat Model

Claude integrations often concentrate risk around `system` instructions, `messages.create`, tool use, and how `tool_use` and `tool_result` blocks are fed back into the next turn. A poisoned result or retrieved block can become more influential if the application collapses everything into one trusted conversation state.

The main review question is whether untrusted content can influence the next Claude request or whether Claude-selected tool actions execute without deterministic controls.

## Inspect These Areas First

- `messages.create` calls and `system` instruction assembly
- message blocks, especially `tool_use` and `tool_result`
- tool definitions and execution wrappers
- streamed output handlers
- retrieved content, file content, and browsing output merged into messages
- conversation history and memory reuse

## Common Bad Patterns

- untrusted content merged into `system` instructions or other privileged blocks
- `tool_result` content treated as trusted instructions on the next turn
- model-selected tool arguments executed without validation
- Claude output used to trigger external actions without approval or server-side authorization
- conversation history preserving attacker instructions across turns

## Safer Patterns

- keep system instructions static and separate from untrusted content blocks
- treat `tool_result` and retrieved content as untrusted context, not privileged instructions
- validate every tool call outside the model
- keep approval gates for high-impact actions
- review what gets persisted into conversation history or memory

## Search Hints

Start with queries like:

```bash
rg -n --glob '!node_modules' --glob '!dist' --glob '!__pycache__' --glob '!.venv' --glob '!venv' '(Anthropic|AsyncAnthropic|messages\.create|system|tool_use|tool_result|stream|claude)'
```

```bash
rg -n --glob '!node_modules' --glob '!dist' --glob '!__pycache__' --glob '!.venv' --glob '!venv' '(tool|validate|authorize|approval|memory|history|retriev|prompt|message)'
```

## How to Report Findings Here

Keep the turn-to-turn propagation clear:

- source: user content, retrieval, tool result, or memory
- transform: message assembly or history reuse
- sink: next Claude request, tool execution, or external side effect

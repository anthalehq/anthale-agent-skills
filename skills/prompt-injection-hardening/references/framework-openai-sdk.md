# OpenAI SDK Notes

Use this file when the repository uses the normal OpenAI SDK rather than the OpenAI Agents SDK.

## Why This SDK Changes the Threat Model

With the normal OpenAI SDK, the risky logic usually lives in your own code: how messages are assembled, how retrieved or tool content is injected, how tool calls are executed, and how assistant output is handled downstream.

The main review question is whether untrusted content can influence request construction or whether model output can trigger privileged behavior without deterministic validation.

## Inspect These Areas First

- `chat.completions.create` and `responses.create`
- message arrays, `instructions`, system prompts, and developer prompts
- tool and function definitions
- tool-call argument handling and execution
- streaming output handlers
- memory, conversation history, and retrieved context assembly

## Common Bad Patterns

- untrusted content interpolated into system or developer instructions
- retrieved content appended to privileged prompt sections without trust separation
- model-proposed tool arguments executed without schema or business-rule validation
- streamed output treated as safe before validation completes
- tool output or assistant output persisted into memory as trusted state

## Safer Patterns

- keep privileged instructions static and structurally separate from untrusted context
- validate tool arguments and permissions outside the model
- treat assistant output as untrusted input to downstream systems
- review how history, retrieval, and tool output are merged into later requests
- keep approval gates around high-impact actions

## Search Hints

Start with queries like:

```bash
rg -n --glob '!node_modules' --glob '!dist' --glob '!__pycache__' --glob '!.venv' --glob '!venv' '(OpenAI|AsyncOpenAI|chat\.completions|responses\.create|messages|instructions|tool_calls|function_call|stream)'
```

```bash
rg -n --glob '!node_modules' --glob '!dist' --glob '!__pycache__' --glob '!.venv' --glob '!venv' '(system|developer|prompt|validate|authorize|allowlist|approval|memory|history|retriev)'
```

## How to Report Findings Here

Tie the finding to the SDK boundary:

- source: user input, retrieval, tool output, or memory
- transform: request assembly, history merge, or tool-call handling
- sink: model call, tool execution, persisted state, or downstream action

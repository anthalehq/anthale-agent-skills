# PydanticAI Notes

Use this file when the repository uses PydanticAI.

## Why This Framework Changes the Threat Model

PydanticAI can reduce some output-shape risk, but prompt injection problems still appear around system prompts, tools, dependencies, message history, and the boundary between validated model output and privileged application behavior.

The main review question is whether the repository mistakes typed outputs for full security. Type validation helps with structure, but it does not replace authorization, trust separation, or tool gating.

## Inspect These Areas First

- `Agent(...)` construction and system prompt assembly
- tools and tool wrappers
- dependencies and context objects passed into runs
- message history and memory reuse
- output models and what happens after validation
- retries, fallbacks, and delegated actions

## Common Bad Patterns

- untrusted content inserted into system prompts or high-trust instructions
- typed outputs assumed to be safe for privileged actions without business-rule checks
- tools executed from model-selected data without authorization or approval
- dependencies carrying broad authority into every run
- message history or summaries replaying poisoned content across runs

## Safer Patterns

- keep system prompts static and separate from untrusted context
- treat validated outputs as structured data, not as security-approved authority
- validate authorization, ownership, and business rules after schema validation
- scope tools and dependencies to the minimum needed per run
- review what gets stored in message history or memory

## Search Hints

Start with queries like:

```bash
rg -n --glob '!node_modules' --glob '!dist' --glob '!__pycache__' --glob '!.venv' --glob '!venv' '(pydantic_ai|Agent\(|RunContext|system_prompt|tool|deps|message_history|result_type|output_type)'
```

```bash
rg -n --glob '!node_modules' --glob '!dist' --glob '!__pycache__' --glob '!.venv' --glob '!venv' '(BaseModel|model_validate|validate|authorize|approval|memory|history|retry|prompt)'
```

## How to Report Findings Here

Make the post-validation boundary explicit:

- source: user input, retrieval, tool output, or history
- transform: prompt assembly, agent run, or typed parsing
- sink: tool execution, external action, or persisted state

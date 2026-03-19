# Gemini SDK Notes

Use this file when the repository uses the Gemini SDK.

## Why This SDK Changes the Threat Model

Gemini integrations often blend `system_instruction`, `generate_content`, multimodal input, file uploads, and function calling. That creates risk where external content or model-selected calls cross into server authority without strong validation.

The main review question is whether untrusted multimodal or retrieved content can influence function calling, later turns, or privileged actions beyond what the application intended.

## Inspect These Areas First

- `generate_content` and chat/session wrappers
- `system_instruction` construction
- function calling and tool configuration
- uploaded files, images, and external content blocks
- streaming handlers and downstream action code
- saved history, summaries, and retrieval assembly

## Common Bad Patterns

- untrusted text or file-derived content merged into `system_instruction`
- function calls executed without server-side validation
- uploaded files or multimodal inputs promoted into trusted instructions
- streamed model output used before validation completes
- history or summaries replaying poisoned content into later Gemini calls

## Safer Patterns

- keep system instructions structurally separate from untrusted content
- validate function-call names and arguments outside the model
- treat file and multimodal content as untrusted input throughout the flow
- keep approval or authorization checks around high-impact actions
- review what gets persisted into summaries, history, or memory

## Search Hints

Start with queries like:

```bash
rg -n --glob '!node_modules' --glob '!dist' --glob '!__pycache__' --glob '!.venv' --glob '!venv' '(gemini|google\.genai|google\.generativeai|GenerativeModel|generate_content|system_instruction|function_call|tool_config|stream)'
```

```bash
rg -n --glob '!node_modules' --glob '!dist' --glob '!__pycache__' --glob '!.venv' --glob '!venv' '(file|image|upload|history|memory|validate|authorize|approval|retriev|prompt)'
```

## How to Report Findings Here

Keep the content type explicit:

- source: text, retrieved content, uploaded file, image, or tool output
- transform: request assembly, history merge, or function-call handling
- sink: Gemini request, function execution, or downstream action

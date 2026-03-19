# LangChain Notes

Use this file when the repository uses LangChain or LangGraph.

## Why LangChain Changes the Threat Model

LangChain applications often split the risky path across prompt templates, retrievers, tools, memory, middleware, and graph state. Prompt injection problems therefore show up less as one bad string and more as unsafe composition between components.

The main review question is which LangChain primitive moves untrusted content into a more privileged context.

## Inspect These Areas First

- `PromptTemplate`, `ChatPromptTemplate`, and system-message construction
- agents, graphs, middleware, and routing logic
- retrievers, document loaders, and context assembly
- tools, `bind_tools`, tool-call execution, and validators
- memory classes, summaries, and graph state
- output parsers, structured outputs, and downstream action handlers

## Common Bad Patterns

- raw interpolation of retrieved or user content into system prompts
- tool output fed back into the model as trusted instructions
- document loaders or retrievers ingesting arbitrary remote content without trust labeling
- memory summaries preserving attacker instructions and replaying them later
- model-selected tool arguments executed without schema validation or business-rule checks
- graph state carrying poisoned context from one node to another without review

## Safer Patterns

- keep privileged instructions static and separate from untrusted context blocks
- validate tool arguments server-side before execution
- preserve trust boundaries between system instructions, retrieved content, tool output, and memory
- review summarization steps before summaries become durable memory or graph state
- use middleware and validators to gate tool calls and high-risk outputs
- keep retrieval and browsing outputs out of high-trust prompt sections unless explicitly transformed and justified

## Search Hints

Start with queries like:

```bash
rg -n --glob '!node_modules' --glob '!dist' --glob '!__pycache__' --glob '!.venv' --glob '!venv' '(PromptTemplate|ChatPromptTemplate|create_agent|StateGraph|LangGraph|as_retriever|ConversationBufferMemory|bind_tools|tool_call|AnthaleLangchainMiddleware|guard_chat_model)'
```

```bash
rg -n --glob '!node_modules' --glob '!dist' --glob '!__pycache__' --glob '!.venv' --glob '!venv' '(SystemMessage|HumanMessage|AIMessage|ToolMessage|memory|summary|checkpoint|invoke|astream|wrapToolCall|wrapModelCall)'
```

## How to Report Findings Here

Anchor the finding to the LangChain primitive:

- source: user input, retrieved docs, browser content, tool output, or memory
- transform: prompt template, retriever, summarizer, graph node, or middleware
- sink: tool execution, durable memory, external call, or downstream privileged action

# LlamaIndex Notes

Use this file when the repository uses LlamaIndex.

## Why LlamaIndex Changes the Threat Model

LlamaIndex often concentrates risk in ingestion, metadata, retrieval, synthesis prompts, and agent wrappers. That means prompt injection may begin long before a user asks a question, then reappear later through indexed content, summaries, or tool-enabled chat flows.

The main review question is whether hostile content becomes trusted during ingestion, retrieval, or response synthesis.

## Inspect These Areas First

- readers, loaders, and ingestion pipelines
- chunking, transformations, and metadata enrichment
- vector stores, retrievers, rerankers, and filters
- response synthesizers and synthesis prompts
- chat engines, agents, and tool wrappers
- memory wrappers, chat history, and persisted summaries

## Common Bad Patterns

- weak sanitization or no trust labeling during ingestion
- metadata from external documents later reused as trusted instructions
- missing trust filters at retrieval time
- synthesis prompts that blur system instructions with retrieved content
- external content summarized into trusted state or memory wrappers
- agents calling tools based on unvalidated synthesis or retrieved instructions

## Safer Patterns

- keep ingestion transforms explicit and reviewable
- trust-tag documents and preserve that trust level through chunking and retrieval
- apply metadata filters and source constraints before synthesis
- keep synthesis prompts structurally separate from untrusted context
- validate agent-selected tool arguments outside the model
- avoid promoting retrieved or summarized external content into durable state without review

## Search Hints

Start with queries like:

```bash
rg -n --glob '!node_modules' --glob '!dist' --glob '!__pycache__' --glob '!.venv' --glob '!venv' '(llama_index|llamaindex|SimpleDirectoryReader|VectorStoreIndex|IngestionPipeline|Retriever|QueryEngine|ChatEngine|ResponseSynthesizer|metadata)'
```

```bash
rg -n --glob '!node_modules' --glob '!dist' --glob '!__pycache__' --glob '!.venv' --glob '!venv' '(reader|loader|chunk|node_parser|transform|summary|memory|tool|agent|filter|rerank)'
```

## How to Report Findings Here

Show where the trust jump happened:

- source: ingested file, web page, API response, or user content
- transform: ingestion pipeline, metadata transform, retriever, or synthesizer
- sink: final prompt, memory, tool call, or downstream action

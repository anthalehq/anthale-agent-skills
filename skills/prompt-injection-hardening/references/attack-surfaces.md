# Attack Surfaces

Use this checklist while mapping the system. Focus on the components that let untrusted content influence privileged behavior.

## 1. System and Developer Prompts

Inspect embedded prompt strings, prompt templates, config files, database-stored prompts, and environment-driven prompt fragments.

Ask:

- Can untrusted variables enter high-priority messages?
- Are retrieved docs, tool outputs, or browser content inserted into system or developer messages?
- Are secrets or internal-only instructions embedded in prompts?
- Are prompts logged, exported, or returned to clients?

## 2. User Input

Inspect chat handlers, API routes, forms, slash commands, CLI arguments, webhooks, and uploaded content.

Ask:

- Is user input separated from instructions?
- Can the user influence tenant IDs, scopes, recipients, destination URLs, or system fields?
- Is normalization or validation applied before model use?

## 3. Retrieval and Corpus Ingestion

Inspect ingestion jobs, loaders, chunkers, vector stores, retrievers, rerankers, citation pipelines, and tenant filters.

Ask:

- Can indexed content contain prompt-like instructions or hidden text?
- Is provenance tracked on stored and retrieved chunks?
- Are cross-tenant or cross-project retrieval leaks possible?
- Is retrieved content framed as data instead of instructions?

## 4. Tool Definitions and Tool Outputs

Inspect tool schemas, tool descriptions, parameter generation, auth checks, and post-tool prompt assembly.

Ask:

- Can tool descriptions or schemas be modified externally?
- Are tool outputs treated as trusted when reintroduced into prompts?
- Does the server validate model-proposed arguments independently of the model?
- Can the model choose sensitive identifiers, file paths, recipients, or scopes?

## 5. Memory Systems

Inspect long-term memory, summaries, scratchpads, session compression, profiles, and episodic stores.

Ask:

- What writes to memory?
- Is memory tagged with source, trust, and time?
- Can attacker content persist and later influence future actions?
- Are low-trust memories filtered or downgraded before reuse?

## 6. External APIs and Browser Results

Inspect fetchers, scrapers, browsers, email readers, webhook consumers, and third-party API adapters.

Ask:

- Is external content isolated as data?
- Are HTML and markdown cleaned or normalized before model use?
- Can external responses trigger actions directly?
- Are outbound destinations constrained by trusted server state?

## 7. Files and Serialization Pipelines

Inspect PDF or OCR readers, markdown or HTML parsers, CSV imports, attachment pipelines, repo indexing, and conversion jobs.

Ask:

- Does parsing preserve adversarial instructions?
- Do summarizers or serializers elevate attacker content into trusted state?
- Can file metadata or hidden content influence later actions?

## 8. MCP Integrations

Inspect allowed servers, server pinning, metadata loading, auth scopes, approval policies, and tool discovery behavior.

Ask:

- Are only trusted MCP servers allowed?
- Can tool metadata drift over time without review?
- Are scopes minimal and explicit?
- Can the server influence prompts or tool descriptions without invocation?

## 9. Planning Loops and Autonomy

Inspect planners, reflection loops, retries, self-healing logic, delegation, and background jobs.

Ask:

- Can untrusted content change the plan?
- Are intermediate steps validated before reuse?
- Are high-risk actions approval-gated?
- Are loops bounded, observable, and easy to interrupt?

# Taxonomy and Review Heuristics

Use this taxonomy to classify weaknesses and to keep reviews precise.

## Prompt Injection Classes

### Direct Prompt Injection

Malicious instructions placed directly in user input. Look for raw concatenation, high-priority interpolation, or missing role separation.

### Indirect Prompt Injection

Malicious instructions embedded in third-party content later read by the model. Common carriers include docs, markdown, HTML, tickets, emails, code comments, and browser results.

### RAG Injection and Context Poisoning

Malicious content enters the retrieval system or retrieved context. Look for unsanitized ingestion, weak tenant isolation, missing provenance, or retrieved chunks framed as instructions.

### Tool Injection

Tool metadata or tool outputs manipulate later model behavior. Look for dynamic remote descriptions, verbatim reuse of tool output, or model control over sensitive arguments without server-side enforcement.

### Agent-Chain Hijacking

Malicious instructions persist across planner steps, summaries, reflections, or handoffs. Look for attacker content reused without validation or trust downgrade.

### System Prompt Leakage

Hidden instructions, tool definitions, or secrets leak through traces, logs, debug endpoints, or prompt-return features.

### Memory Poisoning

Malicious content is written to persistent memory and later reused as if trustworthy.

### Data Exfiltration Prompting

Injected instructions attempt to leak secrets or sensitive data through outbound tools, model-controlled recipients, URLs, or logs.

### Multi-Agent Propagation

One agent steers another or routes through a more privileged one using shared state, delegation, or memory.

## High-Value Code Smells

Flag these aggressively:

- Raw concatenation of system prompts and user text
- Retrieved content inserted into system or developer messages
- Tool output appended directly to future prompts
- Sensitive tools exposed without approval or policy checks
- Model control over tenant IDs, scopes, recipients, destination URLs, or file paths
- Automatic memory writes from conversation or tool output
- Dynamic tool metadata fetched from remote systems
- LLM output passed into shell, eval, SQL, browser actions, HTTP clients, or messaging without validation
- Prompts, traces, or logs containing secrets

## Secure Patterns to Reward

Highlight these when present:

- Static high-priority instructions with strict data separation
- Narrow context windows per step
- Retrieval provenance, trust metadata, and tenant isolation
- Tool authorization enforced on the server independent of the model
- Approval gates for high-risk actions
- Memory writes gated by policy and labeled with source and trust
- Output validation before execution or external send
- Logging, tracing, and red-team coverage that make suspicious behavior observable

## Static-Analysis Mindset

Think in taint flows:

- Untrusted sources: user input, files, OCR output, browser content, API responses, tool results, retrieved chunks, memory entries, MCP metadata
- Dangerous transforms: summarization, chunking, template rendering, serialization, prompt composition, planner outputs, conversation compression, memory distillation
- Dangerous sinks: system or developer prompts, tool calls, memory writes, shell or code execution, SQL execution, browser actions, outbound messaging, cross-agent delegation, prompt logs

If untrusted content reaches a sink without a reliable control, write a finding.

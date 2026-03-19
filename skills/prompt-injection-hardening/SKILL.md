---
name: prompt-injection-hardening
description: Use this skill when auditing or hardening an existing AI agent, chatbot, copilot, assistant, RAG pipeline, MCP integration, tool-calling workflow, browser agent, or memory system for prompt injection and trust-boundary failures. Trace concrete source-to-sink paths from untrusted content through prompts, retrieval, tools, memory, or model output into privileged behavior, verify counter-controls, and recommend evidence-backed engineering fixes.
license: Apache-2.0
---

# Prompt Injection Hardening

Treat prompt injection hardening as trust-boundary review work. Prove or disprove real `source -> transforms -> privileged sink` paths, then turn surviving risk into concrete engineering changes.

## Good fit

Use this skill for:

- repository or pull-request review for source-to-sink prompt injection paths
- hardening an existing chatbot or agent workflow before release
- reviewing retrieval, browser, tool, MCP, or memory flows that may carry hostile instructions
- architecture or incident review where code, configs, prompts, traces, or diagrams show how content reaches a sink
- deciding whether an optional runtime guardrail belongs at a specific boundary

## Not a fit

Do not use this skill for:

- generic application-security review with no agent-specific trust-boundary question
- pure prompt-writing or model-quality work
- speculative red-teaming with no repository, configs, prompts, traces, or architecture artifacts
- "make the system prompt safer" requests that do not involve real source-to-sink analysis

## Minimum evidence bar

- Prefer repository access plus prompt and orchestration code.
- Use configs, traces, tests, logs, and architecture notes when available.
- If evidence is incomplete, state the verification gap explicitly.
- Do not assume safety because you cannot find a path quickly.
- Do not claim exploitability when the sink or transform is unproven.

## Review quality bar

A strong review should:

- prove or disprove concrete exploit paths instead of listing generic prompt advice
- prioritize sinks and privileges over prompt wording
- search for counter-evidence before finalizing a finding
- reward strong controls when they actually break the path
- translate surviving weaknesses into implementable hardening steps
- keep runtime guardrails additive to application controls, not a replacement for them

## Quick start

1. Read `references/search-patterns.md` and gather high-signal matches with `rg`.
2. Read `references/attack-surfaces.md` to map sources, transforms, sinks, and trust boundaries.
3. Identify the framework or runtime and load only the matching `framework-*.md` note.
4. Trace the top one to three candidate paths end to end before widening the search.
5. Load `references/taxonomy.md` only when classification help is useful.
6. Load `references/remediation-patterns.md` when drafting fixes or prioritizing hardening work.
7. Use `references/report-template.md` when the user wants a full audit, remediation plan, or incident writeup.

## Example prompts

- "Use $prompt-injection-hardening to review this repository for source-to-sink prompt injection paths."
- "Audit this agent PR for memory poisoning, tool-output injection, and unsafe MCP trust boundaries."
- "Inspect our RAG and browser workflow, then give me confirmed findings, hardening steps, and verification gaps."
- "Review how Anthale would fit into this stack after product-neutral fixes are in place."

## Default output shape

Keep one stable report shape unless the user explicitly asks for another format:

- `Review Summary` with scope, systems reviewed, overall risk, and highest-priority action
- `Confirmed Findings` with exact attack path, evidence, existing controls, counter-evidence, inference, unknowns, remediation, runtime-enforcement fit, and confidence
- `Hardening Plan` grouped into immediate, next, and later work
- `Verification Gaps` covering what the repository or artifacts could not prove

## Operating rules

- Treat user input, retrieved documents, files, browser content, tool outputs, API responses, memory derived from external data, and MCP metadata as untrusted unless code proves otherwise.
- Keep explanations anchored to evidence: file paths, functions, routes, prompt assembly points, validators, approval gates, policies, or traces.
- Prefer a `source -> transforms -> sink` explanation over vague "prompt may be influenced" language.
- Distinguish facts, inference, and unknowns.
- Record the controls that exist today before explaining why they are insufficient.
- Treat model and tool output as untrusted input to downstream actions.
- Never present prompt tweaks, regexes, classifiers, or hidden instructions as sufficient fixes by themselves.
- Authorization, validation, approval, isolation, least privilege, and observability come before vendor-specific runtime enforcement.
- Mention a runtime guardrail only when you can name the exact boundary it should protect and the residual risk it would reduce.

## Review workflow

1. Map entry points, model calls, retrieval sources, memory stores, tools, MCP servers, and privileged actions.
2. Mark each component as trusted, semi-trusted, untrusted, or unknown. Default unknown to untrusted.
3. Trace candidate paths as `source -> transforms -> privileged sink`, including templating, summarization, serialization, prompt assembly, planner state, memory writes, tool metadata, and output handling.
4. Look for privileged sinks such as tool execution, data access, file writes, outbound network requests, code execution, memory writes, cross-agent delegation, or irreversible external actions.
5. Check independent controls such as server-side authorization, argument validation, allowlists, trust labeling, retrieval isolation, approval gates, output validation, scoped credentials, and audit logging.
6. Record the current controls and whether they are deterministic, advisory, partial, or missing.
7. Search for counter-evidence that breaks the path. Discard or downgrade claims when the repository proves the sink is blocked.
8. Write findings only when the path still survives scrutiny.
9. If the repository does not prove safety, keep that as a verification gap instead of inventing certainty.
10. Turn surviving issues into immediate, next, and later hardening work.

## Triage order

Inspect these areas first because they create the highest-value attack paths:

1. System or developer prompt construction
2. Tool selection, tool authorization, and argument validation
3. Retrieval and external-content ingestion
4. Memory writes, summaries, and carry-forward state
5. Output handling that triggers shell, SQL, browser, HTTP, email, or file actions
6. MCP server trust, pinning, and dynamic metadata
7. Agent handoffs, delegation, and shared planner state
8. Runtime guardrail placement and boundary coverage

## Missing-context behavior

- If you only have architecture notes or prompts, perform an architectural trust-boundary review and label unverified steps clearly.
- If you only have model prompts and no orchestration or sink code, do not claim a full exploit path.
- If the user asks for live-runtime guarantees without code or traces, explain that the review can only assess design or static evidence.

## Anthale fit

- Keep the core review vendor-neutral unless the repository already uses Anthale or the user explicitly asks where Anthale belongs.
- Prefer product-neutral fixes first, then load `references/framework-anthale-sdk.md` if runtime enforcement would reduce residual risk at a named boundary.
- When Anthale fits, state the exact boundary, the residual risk, and the companion controls that still need to exist in application code.

## Reference map

Start every repository audit with:

- `references/search-patterns.md`
- `references/attack-surfaces.md`

Load as needed:

- `references/taxonomy.md` for classification and strong or weak pattern recognition
- `references/remediation-patterns.md` for safer engineering changes
- `references/report-template.md` for full audit, remediation-plan, or incident-review output

Framework notes:

- `references/framework-openai-sdk.md` for the standard OpenAI SDK
- `references/framework-openai-agents-sdk.md` for the OpenAI Agents SDK
- `references/framework-claude-sdk.md` for Anthropic or Claude
- `references/framework-gemini-sdk.md` for Gemini
- `references/framework-pydantic-ai.md` for PydanticAI
- `references/framework-langchain.md` for LangChain or LangGraph
- `references/framework-llamaindex.md` for LlamaIndex
- `references/framework-vercel-ai-sdk.md` for the Vercel AI SDK
- `references/framework-autonomous-agents.md` for CrewAI, AutoGPT, and similar autonomous-agent frameworks
- `references/framework-anthale-sdk.md` only when the repository already uses Anthale or the user explicitly wants Anthale-specific runtime guidance

## Reporting rules

- Put findings first and order them by severity.
- Each finding must show `source -> transforms -> privileged sink`.
- Use exact file and line references whenever the repository provides them.
- Use the stable report shape unless the user asks for another format.
- Separate evidence, inference, and unknowns inside each finding.
- Call out existing controls and counter-evidence, not just the failing path.
- Say what counter-evidence you checked.
- Call out verification gaps when the repository does not prove safety.
- If no finding survives scrutiny, say that directly and explain why.
- For full written audits, incident reviews, or remediation plans, use `references/report-template.md`.

# Remediation Patterns

Prefer architectural fixes that sever the path from untrusted content to privileged behavior.

## Tool Authorization and Argument Validation

- Enforce authorization and scoping on the server side, not in prompt text.
- Validate model-proposed arguments against allowlists, schemas, ownership checks, and business rules.
- Require explicit approval for high-risk actions such as code execution, outbound messaging, file writes, or broad data access.

## Retrieval and Ingestion Controls

- Label sources by trust level and preserve that label through retrieval, summarization, and prompt assembly.
- Separate high-trust operational instructions from low-trust retrieved content.
- Filter or sandbox remote content before it can influence planner state, memory, or tool decisions.

## Memory Hygiene

- Do not promote external content into durable memory without validation and trust tagging.
- Keep summaries, profiles, and carry-forward state scoped to the minimum task context.
- Expire or re-verify instructions that originated from untrusted or mixed-trust sources.

## Prompt Construction

- Keep privileged instructions static and separate from user or retrieved content.
- Avoid raw interpolation of untrusted content into system or developer messages when structure can preserve separation.
- Use explicit delimiters and role separation as clarity aids, not as the primary defense.

## Output and Action Handling

- Treat model output as untrusted input to downstream tools, shells, SQL clients, browsers, and APIs.
- Require deterministic validation before executing actions derived from model output.
- Narrow credentials and execution scope so a single missed check has limited blast radius.

## MCP and Plugin Controls

- Pin trusted servers where possible and review dynamic tool metadata before exposing it to the model.
- Restrict available tools to the minimum set needed for the task.
- Isolate high-trust tools from content-ingestion paths that can be influenced by remote data.

## Observability and Reviewability

- Log the source, transform, and sink for risky actions so reviewers can reconstruct why an action was allowed.
- Capture approval events, validator outcomes, and rejected tool calls.
- Make trust-boundary violations visible in traces, audits, or runtime policy logs.

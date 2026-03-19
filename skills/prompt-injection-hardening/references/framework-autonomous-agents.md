# Autonomous Agent Framework Notes

Use this file for CrewAI, AutoGPT, and similar autonomous agent frameworks.

## Why This Framework Family Changes the Threat Model

Autonomous agent frameworks usually increase prompt injection risk because they combine planning, delegation, memory, browsing, and repeated execution loops. A single poisoned instruction can survive across steps, get copied into shared state, or be handed to another agent with more privileges.

The key review question is not only whether one prompt can be influenced. It is whether hostile content can persist, spread, or trigger actions across an agent loop.

## Inspect These Areas First

- role prompts and agent backstories
- task definitions and planner output
- delegation logic and inter-agent messaging
- shared memory, task memory, and summaries
- browsing, scraping, and file ingestion
- code execution, shell access, and outbound actions
- long-running loops, retries, and autonomous background runs

## Common Bad Patterns

- uncontrolled delegation where one agent can route work to another without approval or scope checks
- shared-memory poisoning where untrusted content is stored and later replayed as if it were trusted state
- dynamic tool loading or broad tool exposure without per-task scoping
- planner output treated as executable authority instead of untrusted model output
- persistent poisoned instructions copied across loops, retries, or handoffs
- browser or retrieval content merged into task instructions without trust separation

## Safer Patterns

- keep delegation behind explicit policy, scope, or approval checks
- separate task instructions from retrieved or observed content
- scope tools and credentials to the minimum capability each agent needs
- treat planner output, delegated tasks, and shared memory as untrusted until validated
- expire, re-validate, or trust-tag memory before reusing it in later steps
- require deterministic validation before code execution, file writes, messaging, or network side effects

## Search Hints

Start with queries like:

```bash
rg -n --glob '!node_modules' --glob '!dist' --glob '!__pycache__' --glob '!.venv' --glob '!venv' '(agent|crew|task|delegate|handoff|planner|plan|memory|loop|retry|browse|execute|shell|tool)'
```

Then narrow to the framework-native objects, decorators, or config files used by the repository.

## How to Report Findings Here

Explain the full cross-step path, not just one prompt smell:

- source: where hostile content entered
- propagation: which agent, memory store, planner, or loop carried it forward
- sink: which later tool, delegated task, or privileged action it reached
- why controls failed: missing validation, trust separation, approval, or scoping

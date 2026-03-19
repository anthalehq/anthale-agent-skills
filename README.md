# Anthale Agent Skills

Anthale Agent Skills is a repository of reusable skills for AI coding agents that follow the [Agent Skills](https://agentskills.io) format.

These skills help an agent do repeatable work in real repositories. **They are not runtime detectors or inline enforcement services**. They are durable task guidance: activation rules, workflow, references, and stable output shapes that stay active while the agent works.

## Install

Add the skills package with:

```bash
npx skills add anthalehq/anthale-agent-skills
```

## Available skills

Use this table as the canonical catalog. Add one row per skill.

| Skill                        | Purpose                                                                            | Good fit                                                                                                  | Key assets                                                   |
| ---------------------------- | ---------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------ |
| `prompt-injection-hardening` | Review and harden AI systems against prompt injection and trust-boundary failures. | Agent review, RAG hardening, MCP hardening, tool-calling workflows, memory poisoning analysis, PR audits. | `references/report-template.md`, `references/framework-*.md` |

## What this repository contains

Each skill lives under `skills/<skill-name>/`.

```text
skills/
  <skill-name>/
    SKILL.md
    references/
      ...
```

Use each part for a specific job:

- `SKILL.md`: activation rules, scope, workflow, and reporting behavior
- `references/`: framework notes, remediation guidance, report templates, taxonomies, and other supporting material

## Quick example

Here is the shortest useful workflow for the current skill:

```text
Use $prompt-injection-hardening to audit the internal agent architecture and external repository state for prompt injection risks.
```

## Current skill notes

### `prompt-injection-hardening`

This skill is for reviewing existing AI agents, chatbots, copilots, RAG pipelines, MCP integrations, browser agents, tool-calling workflows, and memory systems for prompt injection and trust-boundary failures.

It is not a runtime detector. It helps an agent prove or disprove real `source -> transforms -> privileged sink` paths, evaluate existing controls, and recommend concrete engineering fixes.

## Anthale docs

Anthale documentation: https://anthale.com/docs

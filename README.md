# Anthale Agent Skills

Anthale Agent Skills is a repository of reusable skills for AI coding agents that follow the [Agent Skills](https://agentskills.io) format.

These skills are meant to improve how an agent reviews, audits, hardens, or implements AI systems. **They are not runtime detectors or inline enforcement services**. They are durable task guidance that keeps the same workflow, checks, and output shape active while the agent works.

## Install

Add the skills package with:

```bash
npx skills add anthalehq/anthale-agent-skills
```

## What this repository contains

Each skill is a self-contained package under `skills/<skill-name>/`. A skill can include:

- `SKILL.md` for activation rules, workflow, and reporting expectations
- `references/` for heavier domain guidance, framework notes, and templates

Current layout:

```text
skills/
  <skill-name>/
    SKILL.md
    references/
      ...
```

## Available skills

Use this table as the canonical catalog. Add one row per skill.

| Skill                        | Purpose                                                                            | Good fit                                                                                                  |
| ---------------------------- | ---------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------- |
| `prompt-injection-hardening` | Review and harden AI systems against prompt injection and trust-boundary failures. | Agent review, RAG hardening, MCP hardening, tool-calling workflows, memory poisoning analysis, PR audits. |

## Quick start

After installation, a skill can be invoked explicitly by name or activated when the task matches its scope.

Example prompts for the current skill:

- `Use $prompt-injection-hardening to review this repository for source-to-sink prompt injection paths.`
- `Audit this agent PR for memory poisoning, tool-output injection, and unsafe MCP trust boundaries.`
- `Inspect our RAG and browser workflow, then give me confirmed findings, hardening steps, and verification gaps.`

## Anthale docs

Anthale documentation: https://anthale.com/docs

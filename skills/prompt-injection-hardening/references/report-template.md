# Report Template

Use this exact structure when the user asks for a full written audit, remediation plan, or incident review, unless the user explicitly requests another format.

## Review Summary

- Scope:
- Systems reviewed:
- Overall risk:
- Highest-priority action:

## Confirmed Findings

For each confirmed finding, use:

### [Severity] Title

- What was found:
- Attack path:
- Privileged sink:
- Evidence:
- Existing controls:
- Counter-evidence checked:
- Inference:
- Unknowns / verification gaps:
- Why the path survives:
- Recommended remediation:
- Runtime enforcement fit:
- Confidence: high, medium, or low

If no concrete finding survives scrutiny, keep this heading and say that directly:

- No confirmed prompt injection path survived scrutiny.

## Hardening Plan

Group work into:

### Immediate

- ...

### Next

- ...

### Later

- ...

Include only actions that add real engineering value.

## Verification Gaps

Call out what could not be verified from the repository, tests, prompts, traces, or runtime artifacts.

## Severity Rubric

- Critical: direct path to highly privileged or irreversible harm
- High: realistic path to data exposure, unauthorized action, or persistence
- Medium: meaningful risk with constraints or partial mitigations
- Low: hardening debt or observability gaps
- Informational: architectural observations without a proven vulnerability

## Required Review Behavior

- Put findings first and order them by severity.
- Prove each finding as `source -> transforms -> privileged sink`.
- Use exact file and line references whenever the repository provides them.
- Distinguish facts, inference, and unknowns.
- Record existing controls before concluding that a path survives.
- Search for disconfirming evidence before finalizing a finding.
- Do not overclaim if you found only a smell and not a complete exploit path.
- Prefer architectural fixes over shallow prompt edits.
- If no finding survives scrutiny, explain why with evidence instead of assuming safety.
- Keep the same top-level headings in every report so teams can compare reviews easily.

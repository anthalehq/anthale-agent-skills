# How to Contribute

Thank you for improving `anthale-agent-skills`.

This repository is the canonical source of truth for Anthale-maintained skills. If a skill changes, update it here first.

## Before You Start

1. Follow our [`Code of Conduct`](./.github/CODE_OF_CONDUCT.md).
2. Search existing issues and pull requests before opening a new one.
3. Report vulnerabilities privately using our [`Security`](./.github/SECURITY.md) policy.
4. Keep skills focused on activation, workflow, and high-signal guidance. Put heavier material in `references/`.
5. Keep repository-level docs aligned with the skills that actually exist under `skills/`.

## Quick-Start Workflow

1. Fork the repository.
2. Clone your fork.

```bash
git clone git@github.com:<your-username>/anthale-agent-skills.git
cd anthale-agent-skills
```

3. Install the root tooling.

```bash
make install
```

4. Create a branch from the default branch.

```bash
git checkout -b feat/<short-name>
```

5. Make your changes.
6. Run the repository checks from the root of the project.

```bash
make format
make lint
make test
```

7. If you add or rename a skill, update the root `README.md` so the catalog stays accurate.
8. Commit your changes with a descriptive conventional commit message.

```bash
git add .
git commit -m "feat(prompt-injection-hardening): expand MCP review guidance"
```

9. Push your branch and open a pull request using the repository template.

## Repository Rules

- Each skill lives under `skills/<skill-name>/`.
- Each skill directory must include `SKILL.md`.
- Use `references/` for supporting material and `agents/` for agent-specific metadata when needed.
- Keep the root `README.md` in sync with the skills available in the repository.

## Commit Message Guidelines

This repository follows [Conventional Commits](https://www.conventionalcommits.org).

- Use a clear type such as `feat`, `fix`, `docs`, `refactor`, `test`, `build`, `ci`, or `security`.
- Use a scope when it helps identify the affected skill or repository area, for example `feat(prompt-injection-hardening): add memory poisoning checklist`.
- Write the description in the imperative mood, for example `add`, `update`, or `fix`.
- Mark incompatible behavior changes with `!` or a `BREAKING CHANGE:` note in the commit body.

## Pull Request Guidelines

- Keep pull requests focused. One logical skill or tooling change is easier to review and safer to merge.
- Use the pull request template and fill in the scope, related issues, and checklist.
- Reference related issues with `Closes #123` or `Related to #123` where appropriate.
- Update the root README and any affected skill references when behavior or supported workflows change.
- Do not add repository-level claims or examples that are not backed by a committed skill in `skills/`.

## Tooling

The root project exposes `make` wrappers for the core repository checks.

- `make install`: installs root repository tooling.
- `make format`: formats Markdown, YAML, JSON, and other Prettier-managed files.
- `make lint`: checks formatting without modifying files.
- `make test`: validates the repository structure for skills.

Thank you for keeping Anthale skills accurate, focused, and easy to activate.

# Contributing

This repository is for reusable `/goal` blueprints, especially for security engineering workflows that need clear scope, evidence, and stop rules.

## Adding A Blueprint

1. Start from [templates/goal-template.md](templates/goal-template.md).
2. Place the new file in the most relevant category under [blueprints](blueprints).
3. Use a short, descriptive filename such as `incident-replay-detection-validation.md`.
4. Add the blueprint to the index table in [README.md](README.md), [blueprints/README.md](blueprints/README.md), and the category README.
5. Keep the blueprint focused on one security question or workflow.

## Required Sections

Every blueprint should include:

- `GOAL`
- `CONTEXT`
- `CONSTRAINTS`
- `PRIORITY`
- `PLAN`
- `DONE WHEN`
- `VERIFY`
- `OUTPUT`
- `STOP RULES`

## Security Standards

Blueprints should:

- Prefer sandbox, staging, local, or read-only environments.
- Use synthetic users, tenants, documents, secrets, tokens, and data by default.
- Include stop rules for production risk, customer data, credentials, destructive actions, and missing authorization.
- Require evidence for findings.
- Avoid instructions that enable unauthorized testing or abuse of third-party systems.
- Avoid printing real secrets or sensitive data.

## Writing Style

- Be direct and operational.
- Use placeholders like `[repo]`, `[sandbox URL]`, `[test tenant]`, and `[policy docs]`.
- Define measurable completion criteria.
- Separate audit-only blueprints from implementation blueprints.
- Prefer exact verification steps over broad advice.

## Review Checklist

Before opening a PR or committing a new blueprint:

- The goal has one clear objective.
- The stop rules are explicit.
- The output format is useful to a security team.
- The verification section requires more than visual/UI-only checks when backend behavior matters.
- The blueprint can be copied and customized without leaking organization-specific information.

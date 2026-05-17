# Goal Blueprints for Security Engineering

Reusable `/goal` blueprints for security engineers, AppSec teams, platform teams, and security-minded developers using agentic coding tools.

`/goal` is useful when a task is larger than one prompt but still has a verifiable stopping condition. Instead of asking an agent to do one step, you give it a durable objective, a validation loop, and explicit stop rules.

For security work, that distinction matters. Good security engineering is rarely one action. It is a loop:

1. Map the surface.
2. Test assumptions.
3. Inspect code, APIs, data, logs, and product behavior.
4. Verify evidence.
5. Document risk and remediation.
6. Repeat until the security question is answered.

These blueprints turn that loop into a reusable operating contract.

## Where `/goal` Is Supported

- [Claude Code goals](https://code.claude.com/docs/en/goal): documented for Claude Code v2.1.139 or later.
- [Claude Code commands](https://code.claude.com/docs/en/commands): `/goal [condition|clear]` keeps Claude working across turns until the condition is met.
- [Codex CLI slash commands](https://developers.openai.com/codex/cli/slash-commands#set-an-experimental-goal-with-goal): documented as an experimental Codex CLI feature behind `features.goals`.
- [Codex follow goals use case](https://developers.openai.com/codex/use-cases/follow-goals): describes durable objectives for long-running work.

Always confirm the behavior in your installed tool and version before running unattended work.

## Blueprint Index

| Blueprint | Use When | Output |
| --- | --- | --- |
| [Product Security E2E Audit](blueprints/product-security-e2e-audit.md) | You need a sandbox-only, product-wide security and trust audit across UI, API, data, logs, docs, setup, and user workflows. | Audit report, coverage matrix, findings backlog, backend/data map. |
| [AuthZ and Tenant Boundary Audit](blueprints/authz-tenant-boundary-audit.md) | You need to validate role-based access control, object-level authorization, and tenant isolation. | Role matrix, route/API coverage, bypass findings, reproduction steps. |
| [Secrets and Supply Chain Hardening](blueprints/secrets-supply-chain-hardening.md) | You need to find exposed secrets, dependency risk, CI/CD over-permissioning, container issues, or provenance gaps. | Remediation-ready findings, safe patches, rotation checklist, residual risk register. |
| [AI App Security Eval](blueprints/ai-app-security-eval.md) | You need to test an AI feature for prompt injection, tool abuse, retrieval leakage, and unsafe agent behavior. | Eval cases, run results, failure analysis, mitigations, regression test plan. |

## Recommended `/goal` Structure

Use the same sections consistently:

```text
/goal

GOAL:
One durable objective with a verifiable end state.

CONTEXT:
Repository, environment, threat model, docs, systems, credentials, and assumptions.

CONSTRAINTS:
Hard boundaries such as sandbox-only, no production data, no destructive actions, no credential rotation.

PRIORITY:
How to trade off safety, evidence, depth, speed, and scope.

PLAN:
The expected execution sequence.

DONE WHEN:
Measurable completion criteria.

VERIFY:
Commands, logs, tests, API calls, database checks, browser checks, screenshots, or artifacts required as proof.

OUTPUT:
The final report, patches, matrix, backlog, or evidence package.

STOP RULES:
Conditions where the agent must stop instead of improvising.
```

## Gotchas

- `/goal` is not a sandbox. Permissions, credentials, network access, and environment isolation are still your job.
- The goal must have an observable stopping condition. "Make auth secure" is weak. "Every protected API route has an authZ test and the role matrix passes" is strong.
- The evaluator can only judge evidence surfaced in the session. Ask the agent to report commands, results, files changed, screenshots, logs, and unresolved gaps.
- Avoid mixing unrelated objectives. Use one blueprint per task, branch, worktree, issue, or investigation.
- For security work, stop rules matter as much as the goal. If production risk, credential ambiguity, or destructive action appears, the agent should stop.
- Use synthetic data and isolated accounts by default.

## How To Use

1. Pick the blueprint closest to the security question.
2. Replace placeholders like `[repo]`, `[sandbox URL]`, `[test tenant]`, and `[policy matrix]`.
3. Tighten the stop rules for your environment.
4. Run in a trusted workspace with the right sandbox and approval settings.
5. Review evidence before acting on findings.

## Writing Your Own

Start from [templates/goal-template.md](templates/goal-template.md), then read [docs/writing-effective-goals.md](docs/writing-effective-goals.md) for guidance on scope, evidence, and stop rules.

See [CONTRIBUTING.md](CONTRIBUTING.md) for the blueprint format and review checklist.

## License

No license has been selected yet. Add one before inviting broad external reuse.

## Safety Note

These prompts are designed for authorized security engineering work in systems you own or have explicit permission to test. Do not use them against third-party systems without authorization.

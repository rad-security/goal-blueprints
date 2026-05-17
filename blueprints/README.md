# Blueprints

Each file in this directory is a complete `/goal` blueprint that can be copied into Codex CLI or Claude Code, then customized for a specific repo, environment, or engagement.

## Selection Guide

| Security Question | Start With |
| --- | --- |
| "Can a real user complete the product journey safely, and do UI/API/data/logs agree?" | [Product Security E2E Audit](product-security-e2e-audit.md) |
| "Can one user, role, org, or tenant access data or actions they should not?" | [AuthZ and Tenant Boundary Audit](authz-tenant-boundary-audit.md) |
| "Are secrets, dependencies, CI, containers, or build paths exposing risk?" | [Secrets and Supply Chain Hardening](secrets-supply-chain-hardening.md) |
| "Can prompt injection, retrieval leakage, or tool abuse break this AI feature?" | [AI App Security Eval](ai-app-security-eval.md) |

## Before Running

- Use sandbox, staging, or isolated local environments unless the task is explicitly read-only.
- Create synthetic accounts, tenants, test data, and tokens.
- Confirm what tools the agent can use.
- Confirm what data the agent may read.
- Confirm what actions are destructive.
- Add environment-specific stop rules before starting.

## After Running

- Review the transcript and evidence, not just the final summary.
- Reproduce P0/P1 findings manually before escalation where practical.
- Convert validated findings into issues with owners, severity, evidence, and remediation direction.
- Preserve test data only when needed for reproducibility.

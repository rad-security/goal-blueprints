# Blueprints

Each file in this directory is a complete `/goal` blueprint that can be copied into Codex CLI or Claude Code, then customized for a specific repo, environment, or engagement.

## Categories

| Category | What It Covers |
| --- | --- |
| [Product Trust](product-trust) | Launch readiness, whole-product security audits, trust breaks, docs/setup realism, and user-visible safety. |
| [Access Boundaries](access-boundaries) | Auth, sessions, role boundaries, object-level authorization, tenant isolation, and permission edge cases. |
| [AI Security](ai-security) | Prompt injection, retrieval leakage, tool abuse, evals, traces, memory, and AI-agent trust boundaries. |
| [Software Supply Chain](software-supply-chain) | Secrets, dependencies, repository integrity, CI/CD, release controls, package publishing, and build provenance. |
| [Cloud Runtime](cloud-runtime) | Cloud/IaC exposure, IAM, runtime hardening, network boundaries, storage, secrets, and drift. |
| [Detection Response](detection-response) | Incident replay, detection validation, alerting, logs, runbooks, and regression planning. |
| [Data Protection](data-protection) | Privacy, logging, telemetry, retention, deletion, exports, analytics, and sensitive-data flow. |

## Selection Guide

| Security Question | Start With |
| --- | --- |
| "Can a real user complete the product journey safely, and do UI/API/data/logs agree?" | [Product Security E2E Audit](product-trust/product-security-e2e-audit.md) |
| "Are we safe enough to launch this feature?" | [Launch Readiness Security Review](product-trust/launch-readiness-security-review.md) |
| "Can one user, role, org, or tenant access data or actions they should not?" | [AuthZ and Tenant Boundary Audit](access-boundaries/authz-tenant-boundary-audit.md) |
| "Are login, logout, sessions, MFA, reset, and cookies behaving safely?" | [Session and Authentication Hardening](access-boundaries/session-authentication-hardening.md) |
| "Can prompt injection, retrieval leakage, or tool abuse break this AI feature?" | [AI App Security Eval](ai-security/ai-app-security-eval.md) |
| "Are secrets, dependencies, CI, containers, or build paths exposing risk?" | [Secrets and Supply Chain Hardening](software-supply-chain/secrets-supply-chain-hardening.md) |
| "Can untrusted code, CI workflows, or release automation abuse this repo?" | [Repository Integrity and CI Hardening](software-supply-chain/repository-integrity-ci-hardening.md) |
| "Are our cloud resources, IAM, storage, or IaC exposing something unsafe?" | [Cloud and IaC Exposure Audit](cloud-runtime/cloud-iac-exposure-audit.md) |
| "Would we detect and respond correctly if this incident happened again?" | [Incident Replay and Detection Validation](detection-response/incident-replay-detection-validation.md) |
| "Where does sensitive data appear in logs, analytics, exports, and telemetry?" | [Privacy, Logging, and Telemetry Audit](data-protection/privacy-logging-telemetry-audit.md) |

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

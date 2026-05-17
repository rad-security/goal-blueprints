# Writing Effective Security Goals

`/goal` works best when the prompt defines a contract the agent can keep checking against. For security engineering, that contract needs clear scope, strong evidence requirements, and explicit safety boundaries.

## Good Scope

A strong goal is:

- Larger than one prompt.
- Smaller than an open-ended backlog.
- Focused on one security question.
- Measurable from evidence the agent can produce.
- Bound by environment, data, and action constraints.

Weak:

```text
/goal Make this app secure.
```

Stronger:

```text
/goal Audit every tenant-scoped API in staging for cross-tenant read/write access and produce a role-action-resource matrix with reproduction steps for every failure.
```

## The Core Sections

### GOAL

State one durable outcome. Include the target, scope, and stopping condition.

### CONTEXT

Give the agent the operational map: repo, environment, docs, credentials, test accounts, policy docs, database access, logs, feature flags, and known risks.

### CONSTRAINTS

This is where security teams should be strict. Say what the agent must not touch, mutate, delete, rotate, publish, or assume.

### PRIORITY

Security work involves tradeoffs. Priority ordering tells the agent what to protect when scope gets large.

### PLAN

Describe the investigation or implementation sequence you expect. This reduces drift and makes progress easier to audit.

### DONE WHEN

Use objective criteria. Examples:

- Every protected route is covered in the matrix.
- `npm test` exits 0.
- All P0/P1 findings have reproduction steps.
- No confirmed secrets remain in the current tree.
- Every eval case has expected behavior, actual behavior, and evidence.

### VERIFY

Require proof. For security work, UI-only verification is rarely enough. Combine browser behavior, direct API checks, logs, database state, test results, traces, and source inspection.

### OUTPUT

Tell the agent exactly what artifact you want: findings backlog, patch summary, coverage matrix, executive summary, eval report, or remediation plan.

### STOP RULES

Stop rules keep autonomy inside the trust boundary. Use them for:

- Production risk.
- Real customer data.
- Credential handling.
- Destructive actions.
- Missing environment clarity.
- Unclear authorization to test.
- Any verification gap that would lower confidence.

## Security-Specific Tips

- Use synthetic users, tenants, documents, secrets, and resources by default.
- Tell the agent to test direct API access, not just UI visibility.
- Require evidence for every issue.
- Require severity labels and user impact.
- Require unknowns to be listed instead of hidden.
- Keep one goal per branch, worktree, ticket, or investigation.
- Ask for a coverage matrix when completeness matters.
- Ask for a residual risk register when the task cannot be fully verified.

## Common Failure Modes

### Vague completion

If the agent cannot prove it is done, the goal will drift or stop too early.

### Unsafe autonomy

`/goal` does not replace sandboxing, approvals, or credential hygiene.

### Mixed objectives

Do not combine an authZ audit, dependency update, and product UX audit in the same goal unless the target is explicitly a broad audit.

### Missing evidence

The evaluator can only judge what appears in the session. Require the agent to surface commands, results, logs, diffs, and unresolved gaps.

### Silent assumption changes

Add stop rules that prevent the agent from lowering the verification bar when access is missing.

## Recommended Review Loop

1. Draft the goal.
2. Ask the agent to critique the goal before running it.
3. Tighten the stopping condition.
4. Tighten the stop rules.
5. Run in sandbox or read-only mode first.
6. Review evidence.
7. Convert validated findings into issues, fixes, or follow-up goals.

## Choosing A Category

Use categories to make the security question obvious before someone opens the file:

- `product-trust`: user journeys, launch readiness, trust breaks, and whole-product audits.
- `access-boundaries`: auth, sessions, authorization, tenant isolation, and permission edges.
- `ai-security`: AI features, agents, retrieval, tool use, memory, and evals.
- `software-supply-chain`: secrets, dependencies, CI/CD, repo integrity, packages, and releases.
- `cloud-runtime`: IaC, IAM, runtime hardening, network exposure, storage, and cloud drift.
- `detection-response`: incident replay, alerting, logs, runbooks, and recurrence prevention.
- `data-protection`: privacy, logging, telemetry, retention, deletion, exports, and data flow.

# Launch Readiness Security Review

Use this before a major feature launch, product release, customer beta, public demo, or production rollout. It focuses on whether the feature is safe enough to ship, whether the team can observe it after launch, and whether rollback or containment paths are credible.

Minimum useful inputs:

- Release scope, PRs, design doc, or launch checklist
- Target environment and deployment path
- Feature flags and rollout plan
- Known risks and non-goals
- Test accounts and synthetic data
- Monitoring, logs, and incident owner information

```text
/goal

GOAL:
Perform a launch readiness security review for [feature/release/product area] until every security-sensitive launch risk is either verified as acceptable, documented as a blocker, assigned to an owner, or explicitly marked unknown with the missing evidence.

This is a review and documentation pass. Do not ship, promote, deploy, or modify production configuration unless explicitly instructed later.

CONTEXT:
Use [repo], [release branch/PRs], [design doc], [threat model], [feature flags], [sandbox/staging URL], [test users], [deployment docs], [monitoring dashboards], [runbooks], and [rollback plan].
The review should cover product behavior, auth/authZ, tenant/data boundaries, sensitive data handling, external integrations, logging, alerting, rollout controls, rollback, operational ownership, and customer-facing documentation.
Treat "ready to launch" as a security and operations claim, not just a passing test suite.

CONSTRAINTS:
No production changes.
No customer data.
No real external side effects unless the environment is explicitly sandboxed.
Do not approve launch readiness based on tests alone. Verify behavior through the product/API/logs where practical.
Do not silently ignore missing rollback, observability, ownership, or feature-flag controls.
Do not implement fixes unless explicitly asked after the review.

PRIORITY:
1. Launch blockers that could expose data, bypass permissions, or cause irreversible customer impact
2. Rollback and containment credibility
3. Observability and alerting for security-relevant failure modes
4. Auth, authZ, tenant isolation, and sensitive-data handling
5. External integration and webhook safety
6. Documentation and support readiness
7. Residual risk clarity

PLAN:
Map the release:
- Identify every user-facing and API-facing change in scope.
- Identify affected roles, tenants, resources, integrations, background jobs, migrations, flags, and deployment steps.
- Identify what is new, what changed, what was removed, and what could fail open.
- Identify launch assumptions and convert them into verification checks.

Review security-sensitive behavior:
- Test happy-path and failure-path behavior in sandbox/staging.
- Test permission boundaries, tenant isolation, and direct API access for new or changed surfaces.
- Test sensitive data entry, display, masking, export, deletion, logging, and persistence.
- Test webhooks, callbacks, queued jobs, imports, exports, and external integrations using synthetic data.
- Test rate limits, validation, duplicate submissions, idempotency, and recovery from partial failure.

Review launch controls:
- Inspect feature flags, rollout segmentation, kill switches, migration ordering, and rollback steps.
- Check whether rollback is realistic after database migrations, schema changes, queue jobs, external side effects, or customer-visible state changes.
- Check whether security-relevant dashboards, logs, alerts, and ownership are ready before launch.
- Check whether support and customer-facing docs set accurate expectations.

DONE WHEN:
Every in-scope release surface has a pass/fail/unknown readiness status.
Every launch blocker has severity, owner, reproduction steps, evidence, and recommended action.
Every unknown has the exact missing access or missing evidence.
Rollback and containment paths are documented and assessed.
Observability gaps are documented.
No production systems were changed.

VERIFY:
Use browser automation, direct API calls, logs, network inspection, database checks, and source inspection as needed.
Run relevant tests/builds only as supporting evidence.
Verify feature flag behavior for enabled, disabled, and partially rolled out states.
Verify rollback/kill-switch steps from docs or config where possible without changing production.
Verify security-relevant logs and alerts exist for high-risk actions.
State any verification that could not be performed and why.

OUTPUT:
Produce:

1. Launch readiness verdict
   - Ready, ready with risks, blocked, or unknown
   - Top launch blockers
   - Top residual risks

2. Release surface matrix
   - Surface
   - Risk
   - Verification performed
   - Result
   - Evidence
   - Owner

3. Security findings
   - Severity P0-P3
   - Reproduction steps
   - Expected vs actual behavior
   - User/customer impact
   - Suggested fix direction

4. Rollback and containment assessment
   - Feature flags
   - Kill switches
   - Migration rollback constraints
   - External side-effect constraints
   - Operational owner

5. Observability assessment
   - Logs
   - Alerts
   - Dashboards
   - Audit trails
   - Missing signals

STOP RULES:
Stop immediately if verification would touch production data, production infrastructure, or real customer integrations.
Stop before deploying, promoting, changing feature flags, running migrations, or altering production configuration.
Stop if rollback or ownership is unclear for a high-risk launch path.
Do not mark launch ready while any P0/P1 blocker or critical unknown remains unresolved.
```

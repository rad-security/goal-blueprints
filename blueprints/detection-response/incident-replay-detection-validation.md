# Incident Replay and Detection Validation

Use this after an incident, near miss, bug bounty report, red-team finding, or tabletop exercise. It validates whether the organization can reproduce, detect, triage, contain, and prevent recurrence.

Minimum useful inputs:

- Incident summary or finding
- Timeline and known indicators
- Logs, alerts, dashboards, and runbooks
- Sandbox/staging reproduction path
- Detection rules or SIEM queries
- Owners for affected systems

```text
/goal

GOAL:
Replay [incident/finding/scenario] safely in [sandbox/staging/log-only mode] and validate whether detection, logging, alerting, triage, containment, and regression controls are sufficient to prevent or quickly catch recurrence.

This is a validation and documentation pass. Do not attack production, trigger real incidents, or change live detection rules unless explicitly instructed later.

CONTEXT:
Use [incident report], [timeline], [affected repo/service], [sandbox reproduction steps], [logs], [SIEM/detection queries], [alerting rules], [dashboards], [runbooks], [tickets], and [postmortem actions].
The goal is not only to reproduce the technical bug. Validate whether the organization would notice, understand, and respond to it.

CONSTRAINTS:
No production exploitation.
No real customer data.
No disruptive tests.
No disabling alerts, controls, or access restrictions.
Do not generate noisy security events outside the approved environment.
Do not publish sensitive incident details in final output beyond what is required for authorized internal remediation.
Do not implement fixes unless explicitly asked.

PRIORITY:
1. Safe reproduction or faithful simulation
2. Detection and alerting coverage
3. Log quality and forensic usefulness
4. Containment and rollback paths
5. Regression tests and preventive controls
6. Runbook clarity and ownership
7. Residual recurrence risk

PLAN:
Understand the incident:
- Summarize the original trigger, affected assets, impact, timeline, root cause, and existing remediation claims.
- Identify what must be replayed exactly, what can be simulated, and what must not be repeated.
- Identify expected logs, alerts, dashboards, tickets, and owner notifications.

Replay or simulate safely:
- Reproduce in sandbox/staging if safe.
- If reproduction is unsafe, create a log-only or code-path simulation with synthetic data.
- Capture application logs, audit logs, security logs, API responses, database changes, queue/job behavior, and user-visible behavior.
- Compare observed signals to the original incident timeline and expected detection points.

Validate detection and response:
- Check whether detection rules fire with enough context.
- Check alert routing, severity, ownership, and deduplication.
- Check whether dashboards show the right leading and trailing indicators.
- Check runbooks for exact triage, containment, customer impact assessment, and rollback steps.
- Check whether regression tests or guardrails now cover the root cause.

DONE WHEN:
The incident scenario is safely reproduced, simulated, or explicitly marked unreproducible with reason.
Detection coverage is verified or gaps are documented.
Logs and alerts are assessed for forensic usefulness.
Runbooks and ownership are reviewed.
Regression/prevention controls are documented.
No production systems or real customer data were touched.

VERIFY:
Use sandbox reproduction, synthetic events, logs, SIEM queries, alert dashboards, runbooks, and source inspection.
Verify whether alerts fire, whether they route correctly, and whether they contain enough context.
Verify whether expected logs exist at each stage of the scenario.
Verify whether current code/config contains preventive controls.
State any SIEM, log, or alerting access that was unavailable.

OUTPUT:
Produce:

1. Incident replay summary
   - Scenario
   - Reproduction mode
   - Detection result
   - Biggest recurrence risks

2. Timeline comparison
   - Step
   - Expected signal
   - Observed signal
   - Gap
   - Evidence

3. Detection and response findings
   - Severity P0-P3
   - Missing/weak signal
   - Impact
   - Suggested remediation

4. Runbook assessment
   - Triage steps
   - Containment steps
   - Rollback steps
   - Ownership
   - Missing decisions

5. Regression plan
   - Tests to add
   - Detection rules to update
   - Dashboards/alerts to improve
   - Follow-up validation

STOP RULES:
Stop immediately if reproduction could affect production, customers, real credentials, or live integrations.
Stop before disabling, modifying, or suppressing alerts and detection rules.
Stop before generating high-volume security events.
Stop if incident details are too sensitive to include in the requested output. Redact and summarize.
Do not implement fixes.
```

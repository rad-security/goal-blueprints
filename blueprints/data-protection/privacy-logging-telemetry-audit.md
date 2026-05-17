# Privacy, Logging, and Telemetry Audit

Use this when you need to verify that sensitive data is collected, displayed, logged, exported, retained, and deleted appropriately across product, backend, analytics, and observability paths.

Minimum useful inputs:

- Data classification or privacy requirements
- Product flows involving sensitive data
- Logging/analytics/telemetry destinations
- Retention and deletion policy
- Sandbox/test data
- API and database access if available

```text
/goal

GOAL:
Audit [app/service] privacy, logging, and telemetry behavior until sensitive data collection, display, storage, logging, analytics, exports, retention, deletion, and user-facing privacy expectations are mapped and verified with evidence.

This is an audit and documentation pass. Do not access real customer data or change production telemetry configuration.

CONTEXT:
Use [repo], [sandbox/staging URL], [data classification docs], [privacy policy], [logging config], [analytics events], [telemetry pipelines], [database access], [API docs], [retention/deletion docs], and [synthetic sensitive data].
Focus on where sensitive values travel: UI, URLs, local storage, cookies, API payloads, databases, logs, traces, metrics, analytics events, emails, exports, screenshots, support tooling, and third-party processors.

CONSTRAINTS:
No production data.
No real personal data, secrets, tokens, health data, financial data, or customer content.
Use synthetic canary values to trace data flow.
Do not print full sensitive values in output.
Do not change retention, analytics, logging, or privacy settings unless explicitly approved.
Do not assume privacy policy statements are accurate. Verify behavior.

PRIORITY:
1. Sensitive data leakage to logs, analytics, URLs, traces, or third parties
2. Broken deletion, retention, or export promises
3. Overcollection or unclear user consent
4. Insecure display, masking, copying, and download behavior
5. Support/admin exposure
6. Missing auditability for sensitive access
7. Privacy copy that misleads users

PLAN:
Map data classes and flows:
- Define sensitive data classes relevant to the product.
- Identify where each class is collected, validated, stored, displayed, copied, exported, logged, traced, analyzed, retained, deleted, and shared.
- Identify third-party processors, analytics tools, observability platforms, support tools, and webhook destinations.

Create synthetic canaries:
- Use distinctive fake values for names, emails, tokens, API keys, documents, secrets, and other sensitive fields.
- Exercise product flows that create, edit, export, delete, search, filter, invite, and share those values.
- Track whether canaries appear in URLs, request bodies, responses, local/session storage, cookies, logs, traces, metrics, analytics events, emails, exports, and database records.

Verify privacy promises:
- Compare observed behavior to privacy policy, docs, UI copy, consent text, retention claims, deletion claims, and export claims.
- Test deletion, account closure, data export, audit logs, masking, and access revocation where supported.
- Check whether support/admin-like surfaces expose sensitive values unnecessarily.

DONE WHEN:
Sensitive data classes and flows are mapped.
Canary values have been traced through major product paths.
Logging, telemetry, analytics, export, retention, and deletion behavior are verified or marked unknown.
Every confirmed privacy/security issue has severity, evidence, impact, and remediation direction.
No real customer data or production telemetry config was touched.

VERIFY:
Use browser automation, API calls, network inspection, database queries, logs, traces, metrics, analytics debug views, exports, and source inspection where available.
Search logs and telemetry for synthetic canary values.
Check URLs, local storage, cookies, clipboard/copy flows, downloadable files, emails, screenshots, and rendered pages.
Verify deletion and retention behavior after refresh/relogin where practical.
State any telemetry, analytics, or log destination that could not be inspected.

OUTPUT:
Produce:

1. Data protection summary
   - Highest-risk leakage paths
   - Privacy promise mismatches
   - Unknowns and missing access

2. Data-flow matrix
   - Data class
   - Collection point
   - Storage
   - Logging/telemetry
   - Export/sharing
   - Retention/deletion
   - Evidence

3. Canary trace results
   - Canary type
   - Expected locations
   - Unexpected locations
   - Evidence
   - Severity

4. Findings backlog
   - Severity P0-P3
   - Category: logging, analytics, URL leak, export, deletion, retention, masking, overcollection, third-party sharing, privacy copy
   - Reproduction steps
   - Evidence
   - User impact
   - Suggested remediation

STOP RULES:
Stop immediately if real personal data, customer content, secrets, or regulated data appears.
Stop before changing production logging, retention, analytics, or privacy configuration.
Stop before exporting or downloading real customer data.
Stop if a third-party telemetry destination cannot be safely inspected.
Do not implement fixes.
```

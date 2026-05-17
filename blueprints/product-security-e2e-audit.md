# Product Security E2E Audit

Use this when you need a sandbox-only, product-wide audit that combines real user journey testing with security verification across UI, API, data, logs, docs, and setup paths.

```text
/goal

GOAL:
Execute a sandbox-only product-wide security and trust audit of [product/app] that mimics a real user's journey from having the underlying problem, discovering the product, reading the docs, getting set up, and using the product deeply enough to expose security bugs, trust breaks, usability failures, broken functionality, confusing copy, backend/data inconsistencies, and missed opportunities for user value.

This is an audit and documentation pass only. Do not implement fixes unless explicitly instructed later.

CONTEXT:
[Product/app] has been changing quickly and needs a reality check in [sandbox/staging/local].
The audit must cover UX, functionality, usability, docs, onboarding, setup, product value, auth/authZ, backend behavior, database state, API behavior, logs, edge cases, and real work completion.
Use [sandbox/staging/local] only. Do not touch production data or production infrastructure.
Use the repo instructions in [AGENTS.md/CLAUDE.md/project docs] if present.
If code inspection is needed, inspect only. Do not make code edits.
Use browser automation, logs, network inspection, database inspection, API calls, accessibility checks, and source reading as needed.

CONSTRAINTS:
Audit and document only. No product code changes.
Use [sandbox/staging/local] only.
Do not mutate real customer data.
Create isolated test accounts, organizations, tenants, users, agents, workflows, integrations, and test data where needed.
Record every meaningful action, expected result, actual result, and evidence.
Do not assume UI behavior is correct just because the UI appears successful. Verify backend, database, API, logs, and persisted state.
Do not assume docs are correct. Reproduce setup steps exactly from the docs.
Do not skip small controls: click every button, table filter, sort, dropdown, toggle, switch, empty state CTA, destructive action, invite flow, settings control, and navigation path.
Do not stop at happy paths. Test failure paths, permission boundaries, empty states, loading states, validation, retries, duplicate actions, malformed inputs, long text, mobile viewport, slow network, refresh/back-button behavior, and interrupted flows.
Do not report vague issues. Every finding needs reproduction steps, severity, observed behavior, expected behavior, user impact, and evidence.

PRIORITY:
1. Real user task completion and trust
2. Sandbox safety and evidence quality
3. Broken security boundaries and backend/data correctness
4. Auth, authZ, tenant isolation, and permission behavior
5. Docs/setup accuracy
6. UX clarity, usability, accessibility, and responsiveness
7. Product value and reduction of user anxiety
8. Comprehensive coverage without noise

PLAN:
Start by mapping the product surface:
- Public entry points, marketing pages, docs, auth, onboarding, dashboard, settings, tables, core workflows, integrations, billing-like surfaces if present, admin-like surfaces if present, and support/help paths.
- List all roles, permissions, feature flags, environment assumptions, and sandbox credentials needed.
- Identify backend services, API routes, database tables, queues, jobs, webhooks, logs, and external integrations involved in each major flow.

Create a real user narrative:
- Define the user's problem before using [product/app].
- Simulate discovery from a clean browser context.
- Read docs like a first-time operator.
- Follow setup exactly as written.
- Complete the first meaningful job the product promises to solve.
- Continue into advanced and repeated-use workflows.

Execute testing in layers:
1. Discovery and docs audit
2. Signup/auth/onboarding audit
3. Setup and configuration audit
4. Core product workflow audit
5. AuthZ, tenant-boundary, and permission audit
6. Every route/control/table/filter/settings audit
7. Error, edge case, destructive-action, and recovery audit
8. Backend/API/database/log verification
9. Responsive, accessibility, performance, and UX polish audit
10. Product value and user trust assessment

For every major flow:
- State the user intent.
- Execute it in the browser.
- Capture network/API behavior.
- Verify database state and persistence.
- Check logs for hidden errors.
- Refresh and revisit to confirm state durability.
- Test cancel/back/retry/failure paths.
- Test access as lower-privilege users and unrelated tenants where applicable.
- Rate the UX against confidence, clarity, speed, trust, and user anxiety.

DONE WHEN:
Every user-facing page, major backend-backed action, table control, form, filter, setting, onboarding step, docs setup step, security-sensitive permission boundary, and critical product workflow has been tested or explicitly marked unreachable with a reason.
Every finding is documented with severity P0-P3.
Every critical or major issue has exact reproduction steps.
The audit includes a coverage matrix showing what was tested, what passed, what failed, and what remains unknown.
The docs/setup journey has been reproduced from scratch.
The backend/database/API behavior for core workflows is mapped to user-visible outcomes.
The final report clearly separates bugs, security issues, UX issues, docs issues, backend/data issues, performance/a11y issues, and product-value observations.
No production systems were touched.

VERIFY:
Use browser automation for real UI interaction.
Use network inspection, API responses, console logs, server logs, database queries, and source inspection to verify what actually happened.
Check desktop and mobile viewports.
Check keyboard navigation and focus behavior.
Check loading, empty, error, success, and permission states.
Check persistence after refresh, logout/login, and time gaps where relevant.
Check authZ behavior by attempting actions with lower-privilege roles and unrelated tenants.
Run existing tests only if useful for context, but do not treat passing tests as a substitute for manual product audit.
State any verification that could not be performed and why.

OUTPUT:
Produce a comprehensive audit package with:

1. Executive summary
   - Overall product readiness
   - Biggest trust risks
   - Biggest security risks
   - Biggest user-value wins
   - Top P0/P1 issues

2. Real user journey report
   - Problem discovery
   - Product discovery
   - Docs/setup experience
   - First successful value moment
   - Repeated-use workflow
   - Emotional highs/lows and confidence breaks

3. Coverage matrix
   - Pages/routes
   - Controls/buttons/toggles/filters/tables
   - Forms and validation
   - Roles/permissions
   - AuthZ and tenant-boundary checks
   - Backend/API/database touchpoints
   - Tested/pass/fail/unknown status

4. Findings backlog
   - Severity P0-P3
   - Category: security, bug, UX, docs, backend/data, a11y, responsive, performance, copy, product value
   - Reproduction steps
   - Expected vs actual behavior
   - Evidence
   - User impact
   - Suggested fix direction, without implementing it

5. Backend and data map
   - Which UI actions hit which APIs
   - Which database records change
   - Which logs/jobs/events are involved
   - Any mismatch between UI state and persisted truth

6. Docs/setup audit
   - Missing steps
   - Incorrect assumptions
   - Broken links
   - Ambiguous language
   - Places where a real user would stall

7. Security and trust assessment
   - Auth/authZ concerns
   - Tenant isolation concerns
   - Sensitive data exposure concerns
   - Destructive action and recovery concerns
   - Trust and anxiety moments

8. Final recommendation
   - Ranked fix order
   - Suggested next validation pass
   - Open questions that block certainty

STOP RULES:
Stop immediately if sandbox access is unavailable or unclear. Surface exactly what access is missing.
Stop immediately if an action risks production data or production infrastructure.
Stop before destructive actions unless they are sandbox-only and the expected blast radius is understood.
Stop if credentials, environment variables, or database access are missing for backend verification. Document the partial audit boundary.
Stop if docs/setup cannot be followed because required information is missing. Document the failure as a finding and continue only with clearly labeled assumptions.
Do not silently lower the verification bar.
Do not implement fixes.
Do not expand into production promotion or deployment changes.
Do not mark the audit complete while any major surface remains untested or unaccounted for.
```

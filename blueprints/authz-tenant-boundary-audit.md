# AuthZ and Tenant Boundary Audit

Use this when you need to validate role-based access control, object-level authorization, tenant isolation, invitation flows, admin boundaries, and API bypass resistance.

```text
/goal

GOAL:
Audit [app/service] for authorization, role, and tenant-isolation failures in [sandbox/staging/local] until every sensitive route, API, object access path, and privileged action is tested or explicitly marked unreachable with a reason.

This is an audit and documentation pass only. Do not implement fixes unless explicitly instructed later.

CONTEXT:
Use [repo], [sandbox URL], [API base URL], [test tenants/orgs], [test users], [role matrix], [policy docs], [API docs], and [logs/database access].
The audit must cover UI access control, direct API access, object-level authorization, cross-tenant data access, invitations, membership changes, admin actions, destructive actions, billing-like surfaces if present, and background jobs or webhooks that can mutate tenant-scoped data.
Use synthetic tenants and users only.

CONSTRAINTS:
No production systems.
No real customer data.
No destructive action unless performed against clearly isolated test objects.
Do not change auth provider settings, production roles, SSO configuration, or live customer memberships.
Do not assume hiding a UI control is authorization. Verify server-side enforcement through direct API calls.
Do not assume a successful API response is correct. Verify response body, database state, logs, and persistence after refresh/relogin.
Every finding must include actor role, tenant/org context, object ID pattern, API route or UI path, expected behavior, actual behavior, evidence, and severity.

PRIORITY:
1. Cross-tenant data exposure or mutation
2. Privilege escalation
3. Broken object-level authorization
4. Destructive action abuse
5. Invitation, membership, and admin boundary failures
6. UI/API mismatch
7. Missing tests or ambiguous policy docs

PLAN:
Map the authorization model:
- Identify tenants/orgs/workspaces/accounts/projects and their data ownership model.
- List all user roles, permission levels, admin types, service accounts, and special cases.
- Build a role-action-resource matrix from docs, code, API definitions, and observed UI behavior.
- Identify sensitive resources, privileged actions, and destructive actions.

Create test identities:
- At least two unrelated tenants/orgs.
- Owner/admin, editor/member, viewer/read-only, invited/pending, removed user, disabled user, unauthenticated user, and expired session where supported.
- Record all user IDs, tenant IDs, resource IDs, and role assignments used.

Test each major surface:
1. UI navigation and hidden controls
2. Direct API requests for every sensitive action
3. Object ID guessing, swapping, and replay
4. Cross-tenant reads, writes, deletes, exports, invites, and settings changes
5. Role downgrade/upgrade behavior
6. Removed-user and pending-invite behavior
7. Session refresh, logout/login, token expiry, and back-button behavior
8. Background jobs, webhooks, async tasks, and exports if present
9. Error messages for information disclosure
10. Logs and database state for hidden failures

For every protected action:
- Test allowed role succeeds.
- Test lower-privilege role fails.
- Test unrelated tenant fails.
- Test unauthenticated request fails.
- Test stale or removed access fails.
- Verify API response status, response body, database state, logs, and UI persistence.

DONE WHEN:
Every sensitive route, API endpoint, role boundary, tenant-scoped resource, destructive action, invite/membership path, admin setting, and export/import path has pass/fail/unknown coverage.
A role-action-resource matrix exists and is reconciled against observed behavior.
Every confirmed issue has severity P0-P3, exact reproduction steps, affected actor, affected resource, expected behavior, actual behavior, evidence, and user impact.
All unknowns are explicitly listed with the missing access, missing docs, or blocking ambiguity.
No production data or production infrastructure was touched.

VERIFY:
Use browser automation for UI behavior.
Use direct API calls for server-side enforcement.
Inspect network requests for tenant IDs, object IDs, role claims, and authorization headers.
Query database state where available to verify ownership, membership, and persisted permissions.
Inspect server logs for forbidden requests, unexpected 500s, sensitive identifiers, and policy decision details.
Refresh pages and relogin to confirm persistence.
Attempt ID substitution and replay using synthetic IDs only.
Run existing authZ tests if useful, but do not treat them as sufficient coverage.

OUTPUT:
Produce:

1. Executive summary
   - Overall authorization posture
   - Top privilege and tenant-isolation risks
   - P0/P1 findings
   - Recommended fix order

2. Role-action-resource matrix
   - Role
   - Tenant/org context
   - Resource
   - Allowed actions
   - Observed UI behavior
   - Observed API behavior
   - Pass/fail/unknown

3. Endpoint and route coverage
   - UI route
   - API route
   - Method
   - Resource scope
   - Tested actors
   - Result
   - Evidence

4. Findings backlog
   - Severity P0-P3
   - Category: cross-tenant read, cross-tenant write, privilege escalation, BOLA/IDOR, UI/API mismatch, information disclosure, missing tests, ambiguous policy
   - Reproduction steps
   - Expected vs actual behavior
   - Evidence
   - User impact
   - Suggested fix direction

5. Residual risk
   - Untested paths
   - Missing access
   - Assumptions
   - Recommended next validation pass

STOP RULES:
Stop immediately if there is any risk of touching production data, real customer tenants, real users, or live auth provider configuration.
Stop before changing roles, memberships, SSO settings, billing settings, or destructive resources unless the objects are confirmed synthetic and isolated.
Stop if test tenants, test identities, API access, or logs are insufficient to verify server-side enforcement.
Do not silently replace direct API verification with UI-only testing.
Do not implement fixes.
```

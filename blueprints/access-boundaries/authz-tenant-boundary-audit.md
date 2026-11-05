# AuthZ and Tenant Boundary Audit

Use this when you need to validate role-based access control, object-level authorization, tenant isolation, invitation flows, admin boundaries, and API bypass resistance.

This blueprint is built for finding the failures that usually do not show up in happy-path QA: ID swapping, stale membership, hidden-but-callable APIs, cross-tenant exports, invite edge cases, and role transitions that leave old permissions behind.

Minimum useful inputs:

- At least two synthetic tenants, organizations, accounts, or workspaces
- Multiple synthetic users with different roles
- API access or a way to replay browser requests
- Policy docs or expected role matrix
- Database/log access if available

```text
/goal

GOAL:
Audit [app/service] for authorization, role, and tenant-isolation failures in [sandbox/staging/local] until every sensitive route, API, object access path, and privileged action is tested or explicitly marked unreachable with a reason.

This is an audit and documentation pass only. Do not implement fixes unless explicitly instructed later.

CONTEXT:
Use [repo], [sandbox URL], [API base URL], [test tenants/orgs], [test users], [role matrix], [policy docs], [API docs], and [logs/database access].
The audit must cover UI access control, direct API access, object-level authorization, cross-tenant data access, invitations, membership changes, admin actions, destructive actions, billing-like surfaces if present, and background jobs or webhooks that can mutate tenant-scoped data.
Use synthetic tenants and users only.
Assume the UI may be incomplete evidence. Treat the server-side authorization decision as the source of truth.
Pay special attention to resource identifiers passed in URLs, JSON bodies, GraphQL variables, headers, cookies, local storage, websocket messages, exports, imports, and async job payloads.

EVIDENCE SOURCES:
Completeness in this audit is measured against an external enumeration, not against what the agent happens to discover.
Before testing begins, the operator must supply an authoritative enumeration of protected routes, API endpoints, roles, tenant-scoped resource types, and destructive or state-changing actions.
Acceptable enumeration sources:
- An OpenAPI or Swagger specification for the service.
- A framework route table dump, such as `rails routes`, `manage.py show_urls`, the FastAPI `/openapi.json` document, or Express router introspection.
- An IAM or RBAC role export, such as `aws iam list-roles`, `gcloud iam roles list`, or RBAC role manifests held in the repo.
- An explicit role-action-resource policy matrix maintained in the repo.
Name each enumeration source by file path, command, or document. Reject vague inputs such as "from the documentation" or "from the codebase" that do not identify a specific artifact.
If no authoritative enumeration is available, treat that as a blocking gap: report it and do not certify completeness from agent discovery alone.

CONSTRAINTS:
No production systems.
No real customer data.
No state-changing or destructive call without explicit human approval obtained immediately before that call. State-changing includes create, update, delete, role change, membership change, invite, ownership transfer, archive, restore, and any export or job that mutates state. The agent does not decide whether an object is isolated or synthetic enough to mutate; a human approves each such call.
Do not change auth provider settings, production roles, SSO configuration, or live customer memberships.
Do not assume hiding a UI control is authorization. Verify server-side enforcement through direct API calls.
Do not assume a successful API response is correct. Verify response body, database state, logs, and persistence after refresh/relogin.
Do not stop after testing only one resource type. Apply the same actor/resource/action pattern to every sensitive resource class.
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
- Identify ownership transitions: invite, accept invite, remove user, role downgrade, role upgrade, transfer ownership, leave org, disable account, delete resource, archive resource, restore resource.
- Identify non-UI entry points: API routes, GraphQL operations, background jobs, exports, webhooks, CLI commands, integration callbacks, and deep links.

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
11. Bulk operations, pagination, search, filters, autocomplete, and analytics endpoints
12. File downloads, preview URLs, signed URLs, and share links if present

For every protected action:
- Test allowed role succeeds.
- Test lower-privilege role fails.
- Test unrelated tenant fails.
- Test unauthenticated request fails.
- Test stale or removed access fails.
- Test direct object ID substitution where safe.
- Test duplicate submission and replay where safe.
- Verify API response status, response body, database state, logs, and UI persistence.

DONE WHEN:
Every protected route, API endpoint, role boundary, tenant-scoped resource, destructive action, invite/membership path, admin setting, and export/import path listed in the external enumeration source has pass/fail/unknown coverage. Completeness is measured against that enumeration, not against the agent's own discovery.
A coverage diff between the agent-discovered set and the external enumeration is reported. Routes, endpoints, roles, resources, or actions the agent exercised but that are absent from the enumeration are reported as drift findings. Entries present in the enumeration but not tested are coverage gaps, and while any coverage gap remains the goal is not complete.
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
Check that denied requests do not leak resource names, counts, existence, metadata, or signed URLs.
Check that audit logs record privileged and denied security-relevant actions where the product promises auditability.
Run existing authZ tests if useful, but do not treat them as sufficient coverage.

OUTPUT:
Assign every severity rating using the SEVERITY RUBRIC at the end of this blueprint. Do not assign severity ad hoc.

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
Stop before any state-changing call to roles, memberships, SSO settings, billing settings, or destructive resources, and proceed only after a human approves that specific call.
Stop if test tenants, test identities, API access, or logs are insufficient to verify server-side enforcement.
Do not silently replace direct API verification with UI-only testing.
Do not implement fixes.

SEVERITY RUBRIC:
Assign every severity rating from demonstrated impact and exploitability, not from intuition.
Severity reflects what an unauthorized actor can do, not how the weakness was found.

P0: Cross-tenant data read or mutation, privilege escalation to admin or owner, or a destructive action reachable by an actor who should not reach it, demonstrated with synthetic-equivalent access.
P1: Broken object-level authorization, a sensitive route with no server-side enforcement, or a destructive or state-changing action reachable by a lower-privilege role inside the same tenant.
P2: UI and API authorization mismatch, information disclosure through error messages, counts, existence checks, or metadata, or a stale-access window that persists after a role or membership change.
P3: Ambiguous or undocumented policy, missing authZ test coverage, or a hardening gap with no demonstrated bypass.

A finding that chains into a higher-impact path takes the severity of the resulting impact, not the entry point.
If exploitability is not demonstrated, record the finding at the lower severity and list the missing proof as an unknown.
A coverage gap against the external enumeration is at least P3 until the gap is closed or the path is shown unreachable.
```

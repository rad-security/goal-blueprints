# Session and Authentication Hardening

Use this to review login, logout, session lifetime, MFA, password reset, magic links, OAuth/OIDC flows, cookies, CSRF defenses, and account recovery behavior.

Minimum useful inputs:

- Auth provider and app auth architecture
- Sandbox or local app URL
- Synthetic users with MFA/reset coverage
- API routes and auth middleware
- Cookie/session/token configuration
- Logs for auth events if available

```text
/goal

GOAL:
Audit [app/service] authentication and session behavior in [sandbox/staging/local] until login, logout, session lifetime, account recovery, token handling, CSRF protection, and auth event logging are tested or explicitly marked unknown with the missing evidence.

This is an audit and documentation pass. Do not modify auth provider settings or production authentication configuration.

CONTEXT:
Use [repo], [auth provider docs], [sandbox URL], [test users], [MFA settings], [OAuth/OIDC configuration], [session middleware], [API routes], [cookie settings], [logs], and [database access if available].
The review should cover browser sessions, API sessions, cookies, JWTs or opaque tokens, refresh behavior, logout behavior, password reset, invitation or magic-link flows, MFA enrollment/challenge/recovery, CSRF, and auth event observability.

CONSTRAINTS:
No production accounts.
No real user credentials.
No brute force, credential stuffing, password spraying, or abuse of external auth systems.
Do not change live auth provider configuration.
Do not print tokens, cookies, reset links, magic links, or secrets in the final output.
Do not weaken authentication controls to complete testing.
Use synthetic users and low-rate manual tests only.

PRIORITY:
1. Account takeover risk
2. Session fixation, token leakage, and stale session access
3. MFA, reset, and recovery bypass
4. CSRF and unsafe state-changing requests
5. Logout and revocation correctness
6. Sensitive auth data in logs, URLs, storage, or redirects
7. Auth event visibility and auditability

PLAN:
Map the auth system:
- Identify auth provider, session store, token types, cookie names, auth middleware, protected routes, public routes, and API authentication paths.
- Identify login, logout, signup, invite, password reset, magic-link, OAuth/OIDC, MFA, device/session management, and account recovery flows.
- Identify where tokens, codes, session IDs, CSRF tokens, and redirect URLs are created, stored, transmitted, logged, and invalidated.

Test session behavior:
- Verify cookie attributes: Secure, HttpOnly, SameSite, domain, path, max age, and session/refresh behavior.
- Verify logout invalidates the server-side session or token where applicable.
- Verify back-button, refresh, multi-tab, and relogin behavior after logout.
- Verify role or membership changes affect existing sessions appropriately.
- Verify expired, revoked, malformed, and missing tokens fail closed.

Test auth flows:
- Verify signup, login, failed login, password reset, magic link, invite acceptance, OAuth/OIDC callback, MFA enrollment, MFA challenge, and recovery behavior.
- Check redirect URI validation, state/nonce handling, open redirect risk, and error messages.
- Check rate limits and lockout behavior without performing abusive high-volume testing.
- Check whether auth events are logged with useful metadata and without sensitive values.

Test CSRF and state changes:
- Identify browser-authenticated state-changing routes.
- Verify CSRF defenses or SameSite assumptions.
- Confirm unsafe methods, content types, and missing tokens fail as expected.

DONE WHEN:
Every auth flow and session state listed in the plan has pass/fail/unknown status.
Cookie/session/token handling is documented.
Confirmed issues have severity P0-P3, reproduction steps, evidence, impact, and suggested remediation.
Unknowns list missing access, missing logs, or missing environment support.
No production auth settings or real accounts were touched.

VERIFY:
Use browser automation for real auth flows.
Inspect network requests, redirects, cookies, response headers, local/session storage, and API responses.
Use direct API calls for protected route and CSRF verification where safe.
Inspect logs for auth event visibility and sensitive data leakage.
Inspect code/config for session middleware, token validation, CSRF middleware, and provider configuration.
Refresh, logout/login, use multiple tabs, and test expired/removed sessions where possible.

OUTPUT:
Produce:

1. Auth posture summary
   - Biggest account takeover risks
   - Biggest session integrity risks
   - Missing observability

2. Flow coverage matrix
   - Flow
   - Expected behavior
   - Observed behavior
   - Evidence
   - Pass/fail/unknown

3. Token and cookie assessment
   - Storage location
   - Attributes
   - Lifetime
   - Revocation behavior
   - Leakage concerns

4. Findings backlog
   - Severity P0-P3
   - Reproduction steps
   - Expected vs actual behavior
   - Evidence
   - User impact
   - Suggested fix direction

STOP RULES:
Stop immediately if testing risks real user accounts, production auth provider settings, or production credentials.
Stop before high-volume rate-limit testing, brute force, password spraying, or credential stuffing.
Stop if reset links, tokens, cookies, or secrets may be exposed in logs or output. Redact and document handling.
Do not implement fixes.
```

# Secrets and Supply Chain Hardening

Use this when you need to review a repo or service for exposed secrets, risky dependencies, CI/CD over-permissioning, container issues, insecure build practices, or provenance gaps.

This blueprint is designed to produce owner-ready remediation, not just scanner output. It asks the agent to classify findings, avoid leaking secrets, distinguish reachable risk from generic advisory noise, and stop before actions that require human approval.

Minimum useful inputs:

- Local checkout of the repo
- Package manager and lockfiles
- CI/CD workflow files
- Container and IaC files if present
- Known deployment/release path
- Permission to run read-only scanners and local tests

```text
/goal

GOAL:
Review [repo/service] for secrets exposure and software supply-chain risk until all high-confidence issues are fixed where safe or documented with owner-ready remediation, evidence, severity, and residual risk.

CONTEXT:
Use [repo], package manifests, lockfiles, CI/CD configuration, container files, IaC, deployment manifests, secret scanning output, dependency advisories, build scripts, and release documentation.
The target environment is [local/sandbox/staging]. Production credential rotation, production CI secret changes, and destructive repository history changes are out of scope unless explicitly approved later.
Treat this as a hardening pass across source, dependency graph, build path, release path, and runtime packaging.
If the repo is a monorepo, map each package/app/service and avoid assuming one dependency manifest represents the whole system.

CONSTRAINTS:
Do not rotate live credentials.
Do not print secret values in the final output. Redact secrets to a safe fingerprint such as first 4 and last 4 characters only if needed.
Do not rewrite git history.
Do not delete dependencies or change build behavior unless the change is clearly safe, tested, and minimal.
Do not alter production CI/CD secrets, environments, deploy keys, package publishing credentials, or release settings.
Do not rely on one scanner only. Cross-check high-risk findings manually.
Prefer minimal patches that reduce risk without broad refactors.
Do not mark dependency advisories as irrelevant without explaining reachability, package usage, environment exposure, and compensating controls.
Do not add broad ignore rules. Any ignore must be narrow, justified, and tied to a verified false positive.

PRIORITY:
1. Live or plausibly live secret exposure
2. CI/CD credentials, token scopes, deploy keys, and package publishing paths
3. Critical and high exploitable dependency issues
4. Malicious or typosquatted dependency risk
5. Container and build provenance risks
6. IaC defaults that expose sensitive services or data
7. Missing guardrails and tests

PLAN:
Map the supply-chain surface:
- Identify languages, package managers, lockfiles, workspace layout, build scripts, containers, IaC, CI systems, release jobs, artifact registries, and deployment paths.
- Identify where secrets enter the system: env files, CI secrets, cloud credentials, deploy keys, package tokens, webhooks, local dev docs, and test fixtures.
- Identify trust boundaries: forked PRs, external contributors, generated code, vendored code, install scripts, build artifacts, cache restores, downloaded binaries, container base images, and release signing.
- Identify what can publish, deploy, sign, upload artifacts, or access secrets.

Run and inspect secret exposure checks:
- Scan current tree with available secret scanners.
- Inspect common high-risk paths: `.env*`, config files, CI files, docs, examples, fixtures, tests, scripts, notebooks, logs, and generated artifacts.
- If git history scanning is available and safe, run it read-only and summarize suspected historical exposures.
- Manually classify findings as confirmed secret, test/dummy, false positive, or unknown.
- Check whether example credentials are obviously fake and documented as examples.
- Check whether secret-like values are embedded in Docker layers, generated bundles, committed lockfiles, screenshots, docs, or test snapshots.

Run and inspect dependency checks:
- Use package-manager audit tools where available.
- Inspect direct dependencies, transitive criticals, abandoned packages, install scripts, Git URL dependencies, local path dependencies, broad version ranges, and lockfile drift.
- Check whether advisories are exploitable in this repo's usage where practical.
- Identify dependencies with lifecycle scripts, native extensions, postinstall downloads, unpinned Git references, or unusually broad permissions.
- Check whether security-critical packages are pinned, maintained, and consistent across workspaces.

Review CI/CD and build paths:
- Inspect workflow permissions, token scopes, pull_request_target usage, untrusted input execution, artifact upload/download, cache keys, secret availability to forks, and package publishing steps.
- Inspect Dockerfiles and build scripts for pinned versions, remote curl/bash patterns, root usage, secret leakage into layers, and unsafe build args.
- Inspect IaC and deployment manifests for overly broad IAM, public exposure, insecure defaults, and sensitive outputs.
- Inspect provenance and release controls: branch protection assumptions, required checks, artifact signing, SBOM generation, release notes, image tags, package publishing, and deploy approvals.

Patch only when safe:
- Remove committed dummy secrets only if replacement is straightforward.
- Add or update secret scanning ignore rules only for verified false positives.
- Tighten CI permissions when behavior is clearly preserved.
- Update vulnerable dependencies only when tests/build can validate compatibility.
- Otherwise document the issue with remediation steps.

DONE WHEN:
Current-tree secret exposure has been scanned, manually classified, and documented.
Dependency and lockfile risk has been reviewed and documented.
CI/CD permission and release paths have been reviewed.
Container, IaC, and build scripts have been reviewed where present.
Safe minimal patches have been applied and tested, or issues have been documented for owner action.
No secret values are printed in the final output.
No production credentials, CI secrets, or repository history were mutated.

VERIFY:
Run available secret scanning tools.
Run available dependency audit tools.
Run tests, build, lint, or package-manager checks after any patch.
Inspect diffs for accidental secret disclosure.
Inspect CI config for effective permissions and secret exposure patterns.
Inspect Docker build and IaC files manually where scanners are not available.
Check whether generated artifacts, bundles, and container layers could include secrets or source maps that expose sensitive code.
Check whether supply-chain fixes preserve lockfile integrity and do not introduce unexpected dependency churn.
State any scanner, registry, or advisory source that was unavailable.

OUTPUT:
Produce:

1. Executive summary
   - Overall secrets and supply-chain posture
   - Confirmed high-risk items
   - Safe patches applied
   - Items requiring human owner action

2. Secret findings
   - Severity
   - File/path
   - Secret type
   - Classification: confirmed, dummy/test, false positive, unknown
   - Evidence without revealing the secret
   - Recommended action
   - Rotation needed: yes/no/unknown

3. Dependency findings
   - Package
   - Current version
   - Advisory or risk
   - Exploitability assessment
   - Fix version or mitigation
   - Tests run

4. CI/CD and build findings
   - Workflow or script
   - Risk
   - Evidence
   - Suggested hardening

5. Patch summary
   - Files changed
   - Why each change is safe
   - Verification commands and results

6. Residual risk register
   - Unknowns
   - Blocked checks
   - Required manual rotations
   - Recommended next pass

STOP RULES:
Stop before rotating, revoking, editing, or exposing any live credential.
Stop before rewriting git history.
Stop before changing package publishing, release, deployment, or production CI secret configuration.
Stop if a suspected real secret is found and its safe handling path is unclear. Redact it, identify the owner/action needed, and wait.
Stop if dependency updates require product decisions or broad compatibility changes.
Do not silently mark unknown secrets as false positives.
```

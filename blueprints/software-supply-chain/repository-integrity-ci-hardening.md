# Repository Integrity and CI Hardening

Use this to review whether a repository's collaboration, CI, and release workflows can be abused to run untrusted code, exfiltrate secrets, bypass review, publish packages, or deploy unsafe artifacts.

Minimum useful inputs:

- GitHub/GitLab repository settings if available
- CI workflow files
- Branch protection and environment rules
- Package publishing and deployment workflows
- Contributor model and PR policy
- Local repo checkout

```text
/goal

GOAL:
Audit [repo] repository integrity, CI/CD workflows, and release controls until high-risk paths for secret exposure, untrusted code execution, review bypass, artifact tampering, and unauthorized deploy/publish are documented with evidence and prioritized remediation.

This is a review and documentation pass. Apply only low-risk local documentation or config suggestions unless explicitly instructed to implement changes.

CONTEXT:
Use [repo], `.github` or equivalent CI config, branch protection documentation, environment rules, CODEOWNERS, dependency update bots, package publishing workflows, deploy workflows, release docs, and repository settings available through [GitHub/GitLab/other].
Treat PRs from forks, generated code, dependency bots, release automation, and artifact handoff between jobs as high-risk trust boundaries.

CONSTRAINTS:
Do not change repository settings, branch protection, secrets, environments, deploy keys, or package publishing configuration unless explicitly approved.
Do not trigger production deploys or package publishes.
Do not expose CI secrets or tokens in output.
Do not run workflows with side effects unless the environment is clearly isolated.
Do not assume repository settings from files alone. Distinguish verified settings from inferred settings.

PRIORITY:
1. CI secret exposure to untrusted code
2. Unauthorized production deploy or package publish
3. Review and branch-protection bypass
4. Artifact/cache poisoning
5. Overbroad workflow token permissions
6. CODEOWNERS and required-check gaps
7. Dependabot or automation trust-boundary issues

PLAN:
Map the repository trust model:
- Identify who can open PRs, approve PRs, merge, release, publish packages, deploy, edit workflows, and manage secrets.
- Identify protected branches, required reviews, required checks, CODEOWNERS, environments, and manual approvals.
- Identify workflows triggered by pull_request, pull_request_target, workflow_run, push, release, schedule, and manual dispatch.
- Identify jobs that receive secrets, write tokens, deploy credentials, package tokens, cloud credentials, or signing keys.

Inspect CI/CD workflows:
- Check default and job-level permissions.
- Check untrusted checkout patterns, especially pull_request_target and workflow_run.
- Check shell injection risks from PR titles, branch names, commit messages, labels, issue comments, and matrix values.
- Check cache keys, artifact upload/download, dependency install scripts, and generated files crossing trust boundaries.
- Check whether forked PRs, bot PRs, and external contributors can influence privileged jobs.

Inspect release controls:
- Review package publishing, image build/push, deploy jobs, release notes, tag creation, signing, provenance, and SBOM generation.
- Check whether release artifacts are built from reviewed source and immutable inputs.
- Check whether deploy environments require approvals and restrict secrets appropriately.
- Check rollback and revocation paths for bad releases.

DONE WHEN:
Every workflow and release path has been reviewed or marked unknown.
Every high-risk trust boundary has a pass/fail/unknown assessment.
Confirmed findings have severity, evidence, impact, and remediation direction.
Repository settings are separated into verified, inferred, and unavailable.
No repository settings, secrets, deploys, or publishes were changed.

VERIFY:
Read CI workflow files and repository policy files.
Use repository settings or CLI/API checks where access is available.
Inspect workflow trigger types, permissions, secrets usage, environment gates, and artifact/cache flows.
Run local static checks where safe.
Do not rely only on workflow names; inspect effective triggers and permissions.
State any settings that could not be verified.

OUTPUT:
Produce:

1. Repository integrity summary
   - Highest-risk CI/CD paths
   - Release/deploy exposure
   - Review bypass risk

2. Workflow risk matrix
   - Workflow
   - Trigger
   - Secrets/token access
   - Untrusted input exposure
   - Risk rating
   - Evidence

3. Release control assessment
   - Artifact build path
   - Publish/deploy path
   - Approval gates
   - Provenance/signing/SBOM status
   - Rollback path

4. Findings backlog
   - Severity P0-P3
   - Reproduction or reasoning steps
   - Evidence
   - Impact
   - Suggested remediation

STOP RULES:
Stop before changing repository settings, secrets, branch protection, environments, deploy keys, or package publishing settings.
Stop before running any workflow or command that could deploy, publish, sign, or upload privileged artifacts.
Stop if secrets or tokens are exposed. Redact and document safe handling.
Do not implement changes unless explicitly asked.
```

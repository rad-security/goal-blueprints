# `/goal` Blueprint Template

Copy this template, replace the placeholders, and delete any lines that do not apply.

```text
/goal

GOAL:
[One durable objective with a verifiable end state.]

CONTEXT:
[Repo, environment, product, service, data sources, docs, threat model, tools, credentials, logs, and assumptions.]

CONSTRAINTS:
[Hard boundaries: sandbox-only, no production, no real customer data, no destructive actions, no credential rotation, no code edits, or patching allowed only within a specific scope.]

PRIORITY:
1. [Highest-priority outcome]
2. [Second-priority outcome]
3. [Third-priority outcome]

PLAN:
[Step-by-step execution path. Include mapping, testing, verification, reporting, and iteration.]

DONE WHEN:
[Measurable completion criteria. Define what must be true before the agent can stop.]

VERIFY:
[Commands, checks, logs, API calls, database queries, screenshots, tests, browser interactions, or artifacts required as proof.]

OUTPUT:
[Final report, patch summary, coverage matrix, findings backlog, evidence package, or PR-ready changes.]

STOP RULES:
[Conditions where the agent must stop and ask instead of improvising.]
```

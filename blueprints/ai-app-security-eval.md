# AI App Security Eval

Use this when you need to test an AI feature, agent, chatbot, workflow, or retrieval/tool system for prompt injection, data exfiltration, unsafe tool use, and policy bypasses.

```text
/goal

GOAL:
Build and run a security evaluation pass for [AI feature/agent/app] until prompt-injection, tool-abuse, retrieval leakage, cross-tenant data exposure, and unsafe output behaviors are measured with reproducible tests and evidence.

This is an evaluation and documentation pass. Do not weaken guardrails or implement fixes unless explicitly instructed later.

CONTEXT:
Use [repo], [local/sandbox URL], [system prompts or prompt files], [tool definitions], [retrieval sources], [policy requirements], [eval framework], [test accounts], [test tenant data], traces, logs, and existing test suites.
The AI feature can access [list tools/data sources]. Treat those tools and data sources as the primary security boundary.
Use synthetic documents, synthetic users, and synthetic secrets only.

CONSTRAINTS:
No real secrets.
No production data.
No production tools.
No customer data.
Do not create eval cases that call real external systems, send real emails, make real payments, modify real infrastructure, or perform destructive actions.
Do not weaken policies, prompts, tool schemas, retrieval filters, or permission checks to make tests pass.
Do not assume a safe final answer means safe behavior. Inspect retrieved context, tool calls, intermediate traces, and logs where available.
Every failure must include the prompt/input, expected safe behavior, observed behavior, trace/tool evidence, user impact, and severity.

PRIORITY:
1. Sensitive data disclosure
2. Cross-tenant retrieval or memory leakage
3. Unsafe tool invocation or argument manipulation
4. Instruction hierarchy failure
5. Policy bypass and jailbreak susceptibility
6. Hallucinated security-sensitive claims
7. Missing regression coverage

PLAN:
Map the AI system:
- Identify user inputs, system/developer prompts, policies, retrieval sources, memory stores, tool definitions, tool permission checks, output rendering, logging, and external side effects.
- Identify trust boundaries between user text, retrieved content, tool outputs, model reasoning, and rendered UI.
- Identify data classes that must never be disclosed.

Create synthetic test fixtures:
- Synthetic tenants/users with clearly labeled private documents.
- Synthetic secrets and canary strings that must never appear in outputs.
- Synthetic tools with harmless side effects where possible.
- Documents containing direct prompt injections, indirect prompt injections, malformed instructions, hidden text, markdown/HTML tricks, and conflicting authority claims.

Build eval coverage:
1. Direct prompt injection
2. Indirect prompt injection through retrieved documents
3. Tool name/argument injection
4. Cross-tenant retrieval attempts
5. Memory leakage attempts
6. Sensitive data extraction attempts
7. Policy refusal and allowed-helpfulness boundaries
8. Unsafe action requests
9. Output rendering injection if the app renders markdown/HTML
10. Regression cases for known prior issues

Run and inspect:
- Execute evals through the real application path where possible.
- Capture prompts, model outputs, retrieved context, tool calls, API responses, traces, logs, and screenshots when useful.
- Classify each case as pass, fail, inconclusive, or blocked.
- Add or update regression tests only if explicitly allowed for this run; otherwise document exact test cases for later implementation.

DONE WHEN:
The AI system's trust boundaries are mapped.
The eval suite covers the major attack classes listed in the plan.
Every eval case has expected behavior, observed behavior, pass/fail/inconclusive status, and evidence.
Confirmed failures are assigned severity P0-P3 with user impact and suggested mitigation.
No production data, real secrets, or real external side effects were used.
The final output includes a prioritized mitigation plan and a recommended regression suite.

VERIFY:
Run evals through the application, API, or eval framework.
Inspect traces and logs for hidden unsafe behavior.
Inspect tool call names, arguments, permissions, and side effects.
Inspect retrieval results for cross-tenant or sensitive-data leakage.
Inspect output rendering for injected links, scripts, unsafe markdown, or misleading UI.
Use canary strings to detect leakage.
State any trace, log, or eval infrastructure that was unavailable.

OUTPUT:
Produce:

1. Executive summary
   - Overall AI security posture
   - Highest-risk failure modes
   - What is protected well
   - Top mitigations

2. Trust boundary map
   - User input
   - System/developer prompts
   - Retrieval sources
   - Tools and permissions
   - Memory/state
   - Output rendering
   - Logs/traces

3. Eval matrix
   - Case ID
   - Attack class
   - Input/prompt
   - Expected behavior
   - Observed behavior
   - Evidence
   - Pass/fail/inconclusive
   - Severity

4. Findings backlog
   - Severity P0-P3
   - Category: data leakage, cross-tenant retrieval, prompt injection, tool abuse, unsafe action, output rendering, policy bypass, hallucination
   - Reproduction steps
   - Evidence
   - User impact
   - Suggested mitigation

5. Regression plan
   - Cases to add to automated evals
   - Required fixtures
   - Required assertions
   - Blocking gaps in test infrastructure

STOP RULES:
Stop immediately if real sensitive data, real secrets, or customer data appears in prompts, retrieval results, traces, logs, or outputs.
Stop before invoking tools that can send messages, make payments, change infrastructure, delete data, or affect production systems.
Stop if tool permissions, tenant boundaries, or data source scope are ambiguous.
Stop if the only way to continue requires weakening guardrails.
Do not implement fixes.
Do not silently downgrade an inconclusive result to pass.
```

# Cloud and IaC Exposure Audit

Use this to review infrastructure-as-code, runtime configuration, cloud permissions, public exposure, network boundaries, storage, secrets, and deployment manifests.

Minimum useful inputs:

- Terraform/Pulumi/CloudFormation/Kubernetes manifests
- Deployment manifests and environment config
- Cloud account or read-only inventory access if available
- Network and data-flow assumptions
- Service ownership and environment names
- Logs or cloud posture output if available

```text
/goal

GOAL:
Audit [service/platform/environment] cloud and infrastructure-as-code exposure until public access, IAM permissions, network paths, secrets handling, storage exposure, runtime hardening, and drift from declared config are reviewed with evidence and prioritized remediation.

This is a review and documentation pass. Do not change cloud resources, production infrastructure, or deployment configuration unless explicitly instructed later.

CONTEXT:
Use [repo], [IaC directory], [Kubernetes manifests], [deployment docs], [cloud provider], [environment names], [read-only cloud inventory], [security group/network policy docs], [secrets manager], [container images], [logs], and [known architecture diagrams].
Focus on the difference between intended architecture, declared infrastructure, and actual runtime exposure.

CONSTRAINTS:
No production changes.
No destructive cloud operations.
No credential rotation.
No public internet scanning unless explicitly authorized and scoped.
Do not print secrets, tokens, private keys, or sensitive cloud identifiers unnecessarily.
Do not assume IaC equals reality. Mark live verification as unavailable if read-only cloud inventory is missing.

PRIORITY:
1. Public exposure of sensitive services, storage, admin panels, databases, queues, or dashboards
2. Overbroad IAM, service accounts, roles, and trust policies
3. Secret leakage through env vars, manifests, logs, images, or outputs
4. Network boundary failures and unintended ingress/egress
5. Container/runtime hardening gaps
6. Missing encryption, logging, backup, and deletion controls
7. Drift between IaC and live environment

PLAN:
Map infrastructure:
- Identify environments, cloud accounts/projects, regions, VPCs/networks, clusters, services, databases, object stores, queues, caches, secrets, IAM roles, service accounts, and public endpoints.
- Identify data flows and trust boundaries between frontend, API, workers, databases, third-party integrations, and admin paths.
- Identify which resources are declared in IaC and which are created manually or externally.

Review exposure and permissions:
- Inspect ingress rules, load balancers, DNS, security groups, firewalls, Kubernetes ingress, network policies, and public storage settings.
- Inspect IAM policies, trust relationships, service account bindings, workload identity, deploy roles, and cross-account access.
- Inspect secrets handling in IaC, env vars, CI, manifests, container images, logs, and runtime config.
- Inspect storage encryption, backup/restore, retention, deletion, public ACLs, and bucket policies.

Review runtime hardening:
- Inspect containers for root user, privileged mode, host mounts, capabilities, seccomp/AppArmor, resource limits, image pinning, and base image risk.
- Inspect Kubernetes RBAC, namespaces, service accounts, pod security, secrets, configmaps, and network policies.
- Inspect logging and auditability for high-risk cloud actions and data access.

DONE WHEN:
Every declared infrastructure area has pass/fail/unknown status.
Every public endpoint and externally reachable resource is documented.
Every high-risk permission path is documented.
Secret handling and storage exposure are reviewed.
Live-vs-IaC drift is assessed where access allows.
No cloud resources or production infrastructure were changed.

VERIFY:
Inspect IaC and deployment manifests.
Use read-only cloud inventory, CLI, or posture reports where available.
Inspect DNS, endpoint configuration, security groups, IAM policies, resource policies, and Kubernetes manifests.
Inspect container/image configuration where available.
Cross-check declared config against live inventory where possible.
State which checks are source-only because live access was unavailable.

OUTPUT:
Produce:

1. Cloud exposure summary
   - Publicly reachable resources
   - Highest-risk IAM paths
   - Secret handling concerns
   - Drift/unknowns

2. Infrastructure coverage matrix
   - Resource
   - Environment
   - Declared config
   - Live verification
   - Risk
   - Evidence

3. Findings backlog
   - Severity P0-P3
   - Category: public exposure, IAM, secrets, network, runtime, storage, logging, drift
   - Evidence
   - Impact
   - Suggested remediation

4. Residual risk
   - Missing cloud access
   - Untested environments
   - Assumptions
   - Recommended next pass

STOP RULES:
Stop before changing, deleting, restarting, redeploying, or rotating any cloud resource or credential.
Stop before public internet scanning unless explicitly authorized.
Stop if the environment boundary is unclear.
Stop if live cloud access reveals sensitive data. Redact and document handling.
Do not implement fixes.
```

---
title: "Policy Reports & Cleanup"
weight: 87
---

## View Policy Reports in Nirmata

Nirmata Policy Reports provide a centralized audit trail of all policy decisions — both admissions and denials — across your clusters. This is critical for compliance teams who need to demonstrate that governance controls are active and effective.

Navigate to **Policies → Policy Reports** in the Nirmata dashboard.

Filter by: **Namespace = ai-workloads**

You should see entries for:
- `bad-ai` — **Fail** — denied by the pod standards policy (missing probes, resources, node selector, token hardening)
- `good-ai` — **Pass** — admitted because it satisfies all policy requirements
- `expensive-ai` — **Fail** — denied by the cost guardrail policy (exceeded CPU/memory limits)

### Drill into a Finding

Click on any finding to see:
- **Policy name** — which ClusterPolicy triggered the result
- **Rule name** — the specific rule within the policy
- **Severity** — high/medium/low classification
- **Message** — human-readable explanation of what was wrong
- **Impacted resource** — the Pod name and namespace
- **Pass/fail status** — whether the resource was admitted or denied

These reports can be exported for audits and integrated into compliance dashboards.

## Cleanup

Remove the resources created in this module:

```bash
kubectl delete pod good-ai bad-ai expensive-ai -n ai-workloads --ignore-not-found
kubectl delete svc good-ai -n ai-workloads --ignore-not-found
kubectl delete cpol ai-workloads-pod-standards restrict-ai-workloads-resource-limits --ignore-not-found
kubectl delete namespace ai-workloads --ignore-not-found
```

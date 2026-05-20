---
title: "Block Non-Compliant Workloads"
weight: 83
---

## Test Policy Enforcement with a Non-Compliant Workload

With the policy in `Enforce` mode, Kyverno will block any Pod in the `ai-workloads` namespace that doesn't meet all four requirements. Let's prove this by deploying a deliberately non-compliant workload.

This Pod is missing:
- CPU and memory requests/limits
- Liveness/readiness probes
- The required `nodeSelector` for approved compute
- `automountServiceAccountToken: false`

```bash
cat << 'EOF' | kubectl apply -f - || true
apiVersion: v1
kind: Pod
metadata:
  name: bad-ai
  namespace: ai-workloads
spec:
  containers:
  - name: app
    image: hashicorp/http-echo:1.0.0
    args:
      - "-listen=:8000"
      - "-text=bad ai workload"
EOF
```

**Expected result:** The Pod is denied. You will see an error message from Kyverno listing which rules were violated.

::alert[The `|| true` at the end prevents the shell from exiting on the expected error.]{type="info"}

### View the Denial Events

In the **Nirmata Dashboard**, navigate to **Monitor → Events**.

You should see denial events from the `ai-workloads-pod-standards` policy showing exactly which rules the Pod violated.

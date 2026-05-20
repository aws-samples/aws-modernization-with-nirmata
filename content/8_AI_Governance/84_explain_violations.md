---
title: "Explain Violations with AI"
weight: 84
---

## Use Agentic AI to Explain the Violation

When a workload is denied, developers need to understand *why* and *how to fix it*. Instead of manually reading policy YAML, you can use `nctl ai` to get a plain-language explanation and a corrected manifest.

Start nctl ai:

```bash
nctl ai
```

Enter the following prompt (paste the denied Pod spec so the AI can analyze it):

```text
Explain why this Kubernetes Pod was denied by Kyverno and generate a compliant version but do not deploy

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
```

The AI will explain each violation:
- **Missing health probes** — Kubernetes cannot detect if the AI service is hung or crashed
- **Missing CPU/memory requests and limits** — the scheduler cannot make placement decisions and the pod could starve other workloads
- **Missing node selector** — the pod could land on any node, bypassing cost and security controls
- **Missing ServiceAccount token hardening** — the pod would get unnecessary access to the Kubernetes API

It will also generate a corrected Pod manifest you can use as a reference.

Exit from nctl AI:

```text
exit
```

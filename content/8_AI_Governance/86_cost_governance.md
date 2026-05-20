---
title: "Cost Governance"
weight: 86
---

## Enforce Cost Guardrails for AI Workloads

AI and ML workloads are notorious for consuming excessive compute — a single misconfigured training job or inference pod can request dozens of CPUs and hundreds of gigabytes of memory. In this step, you will create a policy that caps resource requests, preventing runaway costs.

### Generate the Policy with nctl ai

```bash
nctl ai
```

Enter the following prompt:

```text
Generate a Kyverno ClusterPolicy for Pods in the ai-workloads namespace that denies containers requesting more than 2 CPU or more than 4Gi memory.
```

`nctl ai` will generate a policy and save it to a file. Note the filename it outputs (typically `restrict-ai-workloads-resource-limits.yaml`).

Exit nctl AI:

```text
exit
```

Review the generated file:

```bash
cat restrict-ai-workloads-resource-limits.yaml
```

::alert[If `nctl ai` saved the file with a different name, use that name instead. If you prefer a known-good reference, expand the section below.]{type="info"}

<details>
<summary>Reference Policy (click to expand)</summary>

```bash
cat << 'EOF' > restrict-ai-workloads-resource-limits.yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  annotations:
    pod-policies.kyverno.io/autogen-controllers: none
    policies.kyverno.io/category: Resource Management
    policies.kyverno.io/description: AI workloads can consume excessive cluster resources
      if left unconstrained. This policy enforces upper bounds on CPU and memory limits
      for all containers (including initContainers) in the ai-workloads namespace.
      CPU limits must not exceed 2 cores (2000m) and memory limits must not exceed
      4Gi.
    policies.kyverno.io/minversion: 1.11.0
    policies.kyverno.io/severity: high
    policies.kyverno.io/subject: Pod
    policies.kyverno.io/title: Restrict Resource Limits for AI Workloads
  name: restrict-ai-workloads-resource-limits
spec:
  background: true
  rules:
  - match:
      any:
      - resources:
          kinds:
          - Pod
          namespaces:
          - ai-workloads
          operations:
          - CREATE
          - UPDATE
    name: restrict-cpu-limit
    validate:
      cel:
        expressions:
        - expression: variables.allContainers.all(c, !c.?resources.?limits.?cpu.hasValue()
            || quantity(c.resources.limits.cpu).compareTo(variables.maxCPU) <= 0)
          messageExpression: '''Container ['' + variables.allContainers.filter(c,
            c.?resources.?limits.?cpu.hasValue() && quantity(c.resources.limits.cpu).compareTo(variables.maxCPU)
            > 0)[0].name + ''] exceeds the CPU limit of 2 cores. Requested: '' + variables.allContainers.filter(c,
            c.?resources.?limits.?cpu.hasValue() && quantity(c.resources.limits.cpu).compareTo(variables.maxCPU)
            > 0)[0].resources.limits.cpu'
        variables:
        - expression: object.spec.containers + object.spec.?initContainers.orValue([])
          name: allContainers
        - expression: quantity('2')
          name: maxCPU
  - match:
      any:
      - resources:
          kinds:
          - Pod
          namespaces:
          - ai-workloads
          operations:
          - CREATE
          - UPDATE
    name: restrict-memory-limit
    validate:
      cel:
        expressions:
        - expression: variables.allContainers.all(c, !c.?resources.?limits.?memory.hasValue()
            || quantity(c.resources.limits.memory).compareTo(variables.maxMemory)
            <= 0)
          messageExpression: '''Container ['' + variables.allContainers.filter(c,
            c.?resources.?limits.?memory.hasValue() && quantity(c.resources.limits.memory).compareTo(variables.maxMemory)
            > 0)[0].name + ''] exceeds the memory limit of 4Gi. Requested: '' + variables.allContainers.filter(c,
            c.?resources.?limits.?memory.hasValue() && quantity(c.resources.limits.memory).compareTo(variables.maxMemory)
            > 0)[0].resources.limits.memory'
        variables:
        - expression: object.spec.containers + object.spec.?initContainers.orValue([])
          name: allContainers
        - expression: quantity('4Gi')
          name: maxMemory
  validationFailureAction: Enforce
EOF
```

</details>

### Apply the Policy

```bash
kubectl apply -f restrict-ai-workloads-resource-limits.yaml
```

### Test with an Oversized Workload

Deploy a Pod that requests 8 CPUs and 32Gi memory — far exceeding the 2 CPU / 4Gi limits:

```bash
cat << 'EOF' | kubectl apply -f - || true
apiVersion: v1
kind: Pod
metadata:
  name: expensive-ai
  namespace: ai-workloads
spec:
  automountServiceAccountToken: false
  nodeSelector:
    karpenter.sh/nodepool: general-purpose
  containers:
  - name: app
    image: hashicorp/http-echo:1.0.0
    args:
      - "-listen=:8000"
      - "-text=expensive workload"
    resources:
      requests:
        cpu: "8"
        memory: "32Gi"
      limits:
        cpu: "8"
        memory: "32Gi"
    livenessProbe:
      httpGet:
        path: /
        port: 8000
    readinessProbe:
      httpGet:
        path: /
        port: 8000
EOF
```

**Expected result:** Kyverno denies the workload. The error message will indicate which container exceeded the CPU or memory limit.

Notice that this Pod satisfies the *first* policy (it has probes, node selector, resource fields, and token hardening) but is blocked by the *cost guardrail* policy. Multiple policies compose together — a workload must pass all of them.

---
title: "Generate Policies with Agentic AI"
weight: 82
---

## Use Agentic AI to Generate Kyverno Policies

Instead of writing Kyverno policies by hand, you will use `nctl ai` — Nirmata's agentic AI assistant — to generate governance policies from natural language descriptions. This demonstrates how AI can accelerate policy authoring while ensuring correctness.

### Start nctl ai

```bash
nctl ai
```

### Run Quickstart

Inside the `nctl ai` session, run:

```text
quickstart
```

This scans your cluster and provides an overview of existing resources, policies, and configuration. It gives the AI context about your environment.

### Generate the AI Workload Policy

Still inside the `nctl ai` session, enter the following prompt:

```text
Generate a Kyverno ClusterPolicy for Pods in the ai-workloads namespace that requires:
1. CPU and memory requests and limits
2. livenessProbe and readinessProbe
3. nodeSelector karpenter.sh/nodepool=general-purpose
4. automountServiceAccountToken set to false
Use validationFailureAction Enforce.
```

The AI will generate a complete Kyverno ClusterPolicy with four rules:
- **Resource requests/limits** — prevents unbounded resource consumption
- **Health probes** — ensures Kubernetes can detect and restart unhealthy AI pods
- **Node placement** — forces AI workloads onto approved, cost-controlled instance types
- **Token hardening** — blocks unnecessary Kubernetes API access

Review the generated output to confirm it matches your intent.

::alert[If the AI-generated policy doesn't look right, or you want a known-good reference, expand the section below and use that YAML instead.]{type="info"}

<details>
<summary>Reference Policy (click to expand)</summary>

```bash
cat << 'EOF' > ai-workload-standards.yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  annotations:
    pod-policies.kyverno.io/autogen-controllers: none
    policies.kyverno.io/category: Best Practices
    policies.kyverno.io/description: Enforces production-grade standards for Pods
      in the ai-workloads namespace. Requires all containers to define CPU and memory
      requests and limits, liveness and readiness probes, a nodeSelector targeting
      the general-purpose Karpenter node pool, and disables automatic service account
      token mounting.
    policies.kyverno.io/minversion: 1.11.0
    policies.kyverno.io/severity: high
    policies.kyverno.io/subject: Pod
    policies.kyverno.io/title: AI Workloads Pod Standards
  name: ai-workloads-pod-standards
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
    name: require-resource-requests-and-limits
    validate:
      cel:
        expressions:
        - expression: object.spec.containers.all(c, has(c.resources) && has(c.resources.requests)
            && has(c.resources.requests.cpu) && has(c.resources.requests.memory) &&
            has(c.resources.limits) && has(c.resources.limits.cpu) && has(c.resources.limits.memory))
            && object.spec.?initContainers.orValue([]).all(c, has(c.resources) &&
            has(c.resources.requests) && has(c.resources.requests.cpu) && has(c.resources.requests.memory)
            && has(c.resources.limits) && has(c.resources.limits.cpu) && has(c.resources.limits.memory))
          message: All containers (including init containers) must define CPU and
            memory requests and limits.
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
    name: require-liveness-and-readiness-probes
    validate:
      cel:
        expressions:
        - expression: object.spec.containers.all(c, has(c.livenessProbe) && has(c.readinessProbe))
          messageExpression: '''All containers must define livenessProbe and readinessProbe.
            Containers missing probes: '' + object.spec.containers.filter(c, !has(c.livenessProbe)
            || !has(c.readinessProbe)).map(c, c.name).join('', '')'
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
    name: require-karpenter-nodepool-selector
    validate:
      cel:
        expressions:
        - expression: has(object.spec.nodeSelector) && 'karpenter.sh/nodepool' in
            object.spec.nodeSelector && object.spec.nodeSelector['karpenter.sh/nodepool']
            == 'general-purpose'
          message: 'Pod spec must include nodeSelector with ''karpenter.sh/nodepool:
            general-purpose''.'
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
    name: require-no-automount-service-account-token
    validate:
      cel:
        expressions:
        - expression: object.spec.?automountServiceAccountToken.orValue(true) == false
          message: Pod spec must explicitly set automountServiceAccountToken to false.
  validationFailureAction: Enforce
EOF
```

</details>

### Deploy the Policy

When `nctl ai` finishes generating the policy, it will prompt you to run tests against the policy. Press **yes** to confirm — this validates the policy logic before deployment.

After tests pass, `nctl ai` will ask if you want to deploy the policy to your cluster. Press **yes** to apply it.

Exit the nctl AI session:

```text
exit
```

### Verify

Confirm the policy is active:

```bash
kubectl get cpol
```

You should see `ai-workloads-pod-standards` in the list with status `Ready`.

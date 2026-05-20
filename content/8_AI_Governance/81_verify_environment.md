---
title: "Verify Environment"
weight: 81
---

## Verify Your Environment

Before creating AI governance policies, confirm your EKS cluster and Kyverno policy engine are operational.

```bash
kubectl get nodes
kubectl get pods -n kyverno
kubectl get cpol
```

You should see:
- EKS nodes in `Ready` state
- Kyverno pods running in the `kyverno` namespace
- Existing Kyverno ClusterPolicies listed

### Create the AI Workload Namespace

Create a dedicated namespace to isolate AI workloads. This allows policies to target only AI-related pods without affecting other applications.

```bash
kubectl create namespace ai-workloads --dry-run=client -o yaml | kubectl apply -f -
```

Disable automatic ServiceAccount token mounting on the default service account. This is a security best practice — AI workloads should not have access to the Kubernetes API unless explicitly granted:

```bash
kubectl patch serviceaccount default -n ai-workloads \
  -p '{"automountServiceAccountToken": false}'
```

### Verify the Standard Compute NodePool

EKS Auto Mode uses Karpenter NodePools to provision compute. We need a `general-purpose` NodePool that our AI governance policy will require workloads to target — ensuring AI pods land on approved, cost-controlled instance types.

```bash
kubectl get nodepools
```

You should see a `general-purpose` NodePool. If it does not exist, create it:

```bash
cat << 'EOF' | kubectl apply -f -
apiVersion: karpenter.sh/v1
kind: NodePool
metadata:
  name: general-purpose
spec:
  template:
    spec:
      nodeClassRef:
        group: eks.amazonaws.com
        kind: NodeClass
        name: default
      requirements:
        - key: eks.amazonaws.com/instance-category
          operator: In
          values: ["c","m","t"]
        - key: node.kubernetes.io/instance-type
          operator: In
          values: ["c6a.large","c6i.large","m6i.large","t3.large"]
        - key: karpenter.sh/capacity-type
          operator: In
          values: ["on-demand","spot"]
        - key: kubernetes.io/arch
          operator: In
          values: ["amd64"]
EOF
```

Verify:

```bash
kubectl get nodepools
kubectl get nodeclaims
```

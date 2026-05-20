---
title: "Deploy Compliant AI Workload"
weight: 85
---

## Deploy a Compliant Simulated AI Workload

Now deploy a workload that satisfies all governance requirements. This uses `hashicorp/http-echo` as a lightweight mock inference service — it returns a static JSON response simulating an AI model endpoint.

::alert[In production, this would be a GPU-backed model server (e.g., vLLM, Triton, TGI). The governance patterns are identical regardless of the underlying model.]{type="info"}

```bash
cat << 'EOF' | kubectl apply -f -
apiVersion: v1
kind: Pod
metadata:
  name: good-ai
  namespace: ai-workloads
  labels:
    app: good-ai
    team: ml-platform
spec:
  automountServiceAccountToken: false
  nodeSelector:
    karpenter.sh/nodepool: general-purpose
  containers:
  - name: app
    image: hashicorp/http-echo:1.0.0
    args:
      - "-listen=:8000"
      - "-text={\"model\":\"demo-ai-mock\",\"response\":\"Hello from a governed AI inference workload on Amazon EKS.\"}"
    ports:
      - containerPort: 8000
    resources:
      requests:
        cpu: "100m"
        memory: "64Mi"
      limits:
        cpu: "200m"
        memory: "128Mi"
    securityContext:
      runAsNonRoot: true
      allowPrivilegeEscalation: false
      privileged: false
      readOnlyRootFilesystem: true
      capabilities:
        drop:
          - ALL
      seccompProfile:
        type: RuntimeDefault
    livenessProbe:
      httpGet:
        path: /
        port: 8000
      initialDelaySeconds: 3
      periodSeconds: 10
    readinessProbe:
      httpGet:
        path: /
        port: 8000
      initialDelaySeconds: 3
      periodSeconds: 5
---
apiVersion: v1
kind: Service
metadata:
  name: good-ai
  namespace: ai-workloads
spec:
  selector:
    app: good-ai
  ports:
  - name: http
    port: 8000
    targetPort: 8000
EOF
```

This Pod passes all four policy rules:
- ✅ CPU/memory requests and limits defined
- ✅ Liveness and readiness probes configured
- ✅ `nodeSelector` targets the `general-purpose` NodePool
- ✅ `automountServiceAccountToken: false`

Additionally, it follows security best practices (`securityContext`) even though the policy doesn't require them — defense in depth.

Wait for the workload to become ready:

```bash
kubectl wait --for=condition=Ready pod/good-ai -n ai-workloads --timeout=180s
```

Verify:

```bash
kubectl get pods -n ai-workloads -o wide
```

## Test the Simulated Inference Endpoint

Forward the service port to your local terminal:

```bash
kubectl port-forward svc/good-ai -n ai-workloads 8000:8000 &
```

Send a request mimicking an AI inference API call:

```bash
curl -s http://localhost:8000/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
        "model": "demo-ai-mock",
        "messages": [
          {
            "role": "user",
            "content": "Explain why Kubernetes policy-as-code matters for AI workloads"
          }
        ]
      }'
```

Expected response:

```json
{"model":"demo-ai-mock","response":"Hello from a governed AI inference workload on Amazon EKS."}
```

::alert[The mock service returns the same static response regardless of input. In production, this endpoint would route to an actual model. The key takeaway is that the workload was admitted by Kyverno because it meets all governance requirements.]{type="info"}

Stop the port-forward:

```bash
pkill -f "kubectl port-forward svc/good-ai"
```

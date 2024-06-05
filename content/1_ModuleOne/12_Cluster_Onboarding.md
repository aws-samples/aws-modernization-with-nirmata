---
title: "Cluster On-boarding" # MODIFY THIS TITLE
chapter: true
weight: 2 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES IF APPLICABLE
---

To onboard a cluster with Nirmata,
 Click on the `Add Cluster` button on the `Clusters` panel. If you are trying out NPM for the first time, it is highly recommended to use the default onboarding process instead of the manual onboarding flow.

## Cluster Onboarding
This workflow requires `nctl`. Refer to the [documentation](../../nctl/gettingstarted/#installing-the-cli) for installation.

Enter the cluster name (required) and labels (optional).

![image](/images/add_cluster_1.png)
<!-- <img src="/images/add_cluster_1.png" alt="adding cluster to NPM" /> -->

After entering the cluster information, click on `Select Compliance Standards` to proceed to the next step.
The Pod Security Standards Baseline is added by default. It is highly recommended to opt for Pod Security Standards Restricted and RBAC Best Practices to improve the overall security posture of the cluster.
Select the set of policies to be configured on the cluster as default policy sets. These policies will be deployed in audit mode. After selecting the policy sets, click on `Add Cluster` to proceed to the final step.

![image](/images/add_cluster_2.png)
<!-- <img src="/images/add_cluster_2.png" alt="adding cluster to NPM" /> -->
<!-- <img src="../../images/add_cluster_2.png" width="500" /> -->

Use the `nctl login` command to login to NPM. If the token is not auto generated, visit the profile page and click on `Generate API Key` button to generate the token.

![image](/images/add_cluster_3.png)
<!-- <img src="/images/add_cluster_3.png" alt="adding cluster to NPM" /> -->
<!-- <img src="../../images/add_cluster_3.png" width="500" /> -->

Once the command has run successfully, it will display a message notifying that:
```bash
Validating user credentials...done!
Wrote configuration to /home/username/.nirmata/config
```
Next, copy the `nctl clusters add` command displayed in the final step from the web UI. Run this command to add your cluster to NPM.

After running the above command, a confirmation message will be displayed, notifying that Nirmata Operator has been deployed successfully  on the cluster. Following this, the policy sets selected in the previous step will become ready.
Next, you can click on  `I Have Run the Command` in the web UI to complete the onboarding process and navigate to the Clusters dashboard. The new cluster added can be seen in the dashboard.

![image](/images/onboarding_confirmation.png)
<!-- <img src="/images/onboarding_confirmation.png"> -->

## Technical Information

Nirmata Kube Controller is used to register the cluster with the Nirmata platform.

The following resources will be deployed to the target cluster:

### Deployment

<details>
<summary>nirmata-kube-controller</summary>

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nirmata-kube-controller
  namespace: nirmata
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nirmata-kube-controller
      nirmata.io/container.type: system
      app.kubernetes.io/name: nirmata
      app.kubernetes.io/instance: nirmata
  template:
    metadata:
      labels:
        app: nirmata-kube-controller
        nirmata.io/container.type: system
        app.kubernetes.io/name: nirmata
        app.kubernetes.io/instance: nirmata
    spec:
      containers:
        - args:
            - -token
            - $(TOKEN)
            - -url
            - $(URL)
            - -event-aggregation
          command:
            - /nirmata-kube-controller
          env:
            - name: TOKEN
              value: 6fcee39e-44dc-43a6-9792-468b82fd5a24
            - name: URL
              value: wss://www.nirmata.io/tunnels
          image: ghcr.io/nirmata/nirmata-kube-controller:v3.9.8
          imagePullPolicy: IfNotPresent
          livenessProbe:
            exec:
              command:
                - /nirmata-kube-controller
          name: nirmata-kube-controller
          readinessProbe:
            exec:
              command:
                - /nirmata-kube-controller
          resources:
            limits:
              memory: 512Mi
            requests:
              memory: 200Mi
              cpu: 250m
          securityContext:
            allowPrivilegeEscalation: false
            capabilities:
              drop:
                - ALL
            readOnlyRootFilesystem: true
            runAsNonRoot: true
            seccompProfile:
              type: RuntimeDefault
      hostNetwork: false
      imagePullSecrets:
        - name: nirmata-controller-registry-secret
      securityContext:
        seccompProfile:
          type: RuntimeDefault
      serviceAccountName: nirmata
      tolerations:
        - effect: NoSchedule
          key: node-role.kubernetes.io/master
          operator: Exists
```
</details>

<details>
<summary>otel-agent</summary>

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: otel-agent
  namespace: nirmata
  labels:
    app: opentelemetry
    component: otel-agent
    app.kubernetes.io/instance: nirmata
    app.kubernetes.io/name: nirmata
spec:
  selector:
    matchLabels:
      app: opentelemetry
      component: otel-agent
      app.kubernetes.io/instance: nirmata
      app.kubernetes.io/name: nirmata
  template:
    metadata:
      labels:
        app: opentelemetry
        component: otel-agent
        app.kubernetes.io/instance: nirmata
        app.kubernetes.io/name: nirmata
    spec:
      containers:
        - name: otel-agent
          image: ghcr.io/nirmata/metrics-agent:0.38.3
          resources:
            limits:
              memory: 512Mi
            requests:
              cpu: 100m
              memory: 200Mi
          securityContext:
            allowPrivilegeEscalation: false
            capabilities:
              drop:
                - ALL
            readOnlyRootFilesystem: true
            runAsNonRoot: true
            seccompProfile:
              type: RuntimeDefault
          livenessProbe:
            httpGet:
              path: /metrics
              port: 8888
              scheme: HTTP
          readinessProbe:
            httpGet:
              path: /metrics
              port: 8888
              scheme: HTTP
          volumeMounts:
            - mountPath: /etc/otel/config.yaml
              name: data
              subPath: config.yaml
              readOnly: true
      terminationGracePeriodSeconds: 30
      volumes:
        - name: data
          configMap:
            name: otel-agent-config
```
</details>

### ServiceAccount

<details>
<summary>nirmata</summary>

```
apiVersion: v1
kind: ServiceAccount
metadata:
  name: nirmata
  namespace: nirmata
secrets:
  - name: nirmata-sa-secret
```
</details>

<details>
<summary>nirmata-controller</summary>

```
apiVersion: v1
kind: ServiceAccount
metadata:
  name: nirmata-controller
  namespace: nirmata
```
</details>

### ConfigMap

<details>
<summary>nirmata-kube-controller-config</summary>

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: nirmata-kube-controller-config
  namespace: nirmata
data:
  IgnoreFields: metadata.managedFields
  FilterPatches: |-
    /metadata/resourceVersion
    /metadata/generation
    /results/*/timestamp/*
  IgnoreEvents: Normal.PolicyApplied.*
  WatchedResources: |-
    events.v1.
    policyreports.v1alpha2.wgpolicyk8s.io
    clusterpolicyreports.v1alpha2.wgpolicyk8s.io
    policies.v1.kyverno.io
    clusterpolicies.v1.kyverno.io
    policyexceptions.v2alpha1.kyverno.io
  FilterEvents: Warning.PolicyViolation.*,Normal.PolicySkipped.*
```
</details>

<details>
<summary>otel-agent-config</summary>

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: otel-agent-config
  namespace: nirmata
data:
  config.yaml: >-
    receivers:
      prometheus:
        config:
          scrape_configs:
          - job_name: "kyverno"
            scrape_interval: 1m
            static_configs:
            - targets: ["kyverno-svc-metrics.kyverno.svc.cluster.local:8000"]
            metric_relabel_configs:
            - source_labels: [__name__]
              regex: "(kyverno_admission_review_duration_seconds.*|kyverno_policy_execution_duration_seconds.*|kyverno_policy_results_total|kyverno_policy_rule_info_total|kyverno_admission_requests_total|kyverno_controller_reconcile_total|kyverno_controller_requeue_total|kyverno_controller_drop_total)"
              action: keep
    exporters:
      prometheusremotewrite:
        endpoint: https://www.nirmata.io/host-gateway/metrics-receiver
        external_labels:
          clusterId: 6fcee39e-44dc-43a6-9792-468b82fd5a24
        remote_write_queue:
          queue_size: 2000
          num_consumers: 1
        timeout: 300s
    service:
      pipelines:
        metrics:
          receivers: [prometheus]
          exporters: [prometheusremotewrite]
```
</details>


### ClusterRole
<details>
<summary>nirmata:nirmata-privileged</summary>
Note: This ClusterRole is only needed for NDP

```
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  annotations: {}
  name: nirmata:nirmata-privileged
rules:
  - apiGroups:
      - kyverno.io
      - operator.kyverno.io
      - security.nirmata.io
    nonResourceURLs: []
    resourceNames: []
    resources:
      - policies
      - clusterpolicies
      - reportchangerequests
      - clusterreportchangerequests
      - kyvernooperators/status
      - kyvernooperators
      - imagekeys
      - imagekeys/finalizers
      - imagekeys/status
      - admissionreports
      - clusteradmissionreports
      - backgroundscanreports
      - clusterbackgroundscanreports
      - policyexceptions
      - cleanuppolicies
      - clustercleanuppolicies
      - kyvernoes
      - kyvernoes/status
    verbs:
      - "*"
  - apiGroups: []
    nonResourceURLs:
      - /metrics
    resourceNames: []
    resources: []
    verbs:
      - get
  - apiGroups:
      - "*"
    nonResourceURLs: []
    resourceNames: []
    resources:
      - tokenreviews
      - subjectaccessreviews
    verbs:
      - get
      - create
  - apiGroups:
      - wgpolicyk8s.io/v1alpha1
      - wgpolicyk8s.io/v1alpha2
    nonResourceURLs: []
    resourceNames: []
    resources:
      - policyreports
      - clusterpolicyreports
    verbs:
      - "*"
  - apiGroups:
      - "*"
    nonResourceURLs: []
    resourceNames: []
    resources:
      - policies
      - policies/status
      - clusterpolicies
      - clusterpolicies/status
      - policyreports
      - policyreports/status
      - clusterpolicyreports
      - clusterpolicyreports/status
      - generaterequests
      - generaterequests/status
      - reportchangerequests
      - reportchangerequests/status
      - clusterreportchangerequests
      - clusterreportchangerequests/status
      - updaterequests
      - updaterequests/status
      - admissionreports
      - clusteradmissionreports
      - backgroundscanreports
      - clusterbackgroundscanreports
    verbs:
      - create
      - delete
      - get
      - list
      - patch
      - update
      - watch
      - deletecollection
  - apiGroups:
      - apiextensions.k8s.io
    nonResourceURLs: []
    resourceNames: []
    resources:
      - customresourcedefinitions
    verbs:
      - delete
      - create
      - get
      - list
      - patch
      - update
      - watch
  - apiGroups:
      - "*"
    nonResourceURLs: []
    resourceNames: []
    resources:
      - namespaces
      - networkpolicies
      - secrets
      - configmaps
      - resourcequotas
      - limitranges
      - deployments
      - services
      - serviceaccounts
      - roles
      - rolebindings
      - clusterroles
      - clusterrolebindings
      - events
      - mutatingwebhookconfigurations
      - validatingwebhookconfigurations
      - certificatesigningrequests
      - certificatesigningrequests/approval
      - poddisruptionbudgets
      - ingresses
      - ingressclasses
    verbs:
      - create
      - update
      - delete
      - list
      - get
      - patch
      - watch
  - apiGroups:
      - "*"
    nonResourceURLs: []
    resourceNames: []
    resources:
      - "*"
    verbs:
      - get
      - list
      - watch
      - update
  - apiGroups:
      - certificates.k8s.io
    nonResourceURLs: []
    resourceNames:
      - kubernetes.io/legacy-unknown
    resources:
      - certificatesigningrequests
      - certificatesigningrequests/approval
      - certificatesigningrequests/status
    verbs:
      - create
      - delete
      - get
      - update
      - watch
  - apiGroups:
      - certificates.k8s.io
    nonResourceURLs: []
    resourceNames:
      - kubernetes.io/legacy-unknown
    resources:
      - signers
    verbs:
      - approve
  - apiGroups:
      - coordination.k8s.io
    nonResourceURLs: []
    resourceNames: []
    resources:
      - leases
    verbs:
      - create
      - delete
      - get
      - patch
      - update
```
</details>

<details>
<summary>nirmata:policyexception-manager</summary>

```
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: nirmata:policyexception-manager
rules:
- apiGroups:
  - kyverno.io
  resources:
  - policies
  - clusterpolicies
  - policyexceptions
  verbs:
  - '*'
```
</details>

### ClusterRoleBindings

<details>
<summary>nirmata-cluster-admin-binding</summary>
Note: This ClusterRoleBinding is only needed for NDP

```
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: nirmata-cluster-admin-binding
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: nirmata:nirmata-privileged
subjects:
  - kind: ServiceAccount
    name: nirmata
    namespace: nirmata
```
</details>

<details>
<summary>nirmata-controller-binding</summary>

```
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: nirmata-controller-binding
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: view
subjects:
  - kind: ServiceAccount
    name: nirmata-controller
    namespace: nirmata
```
</details>

<details>
<summary>nirmata:policyexception-manager</summary>

```
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: nirmata:policyexception-manager
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: nirmata:policyexception-manager
subjects:
  - kind: ServiceAccount
    name: nirmata
    namespace: nirmata
```
</details>

### RoleBinding
<details>
<summary>nirmata-admin-binding</summary>

```
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: nirmata-admin-binding
  namespace: nirmata
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: admin
subjects:
  - kind: ServiceAccount
    name: nirmata
    namespace: nirmata
```
</details>

### Secret

<details>
<summary>nirmata-sa-secret</summary>

```
apiVersion: v1
kind: Secret
metadata:
  name: nirmata-sa-secret
  namespace: nirmata
  annotations:
    kubernetes.io/service-account.name: nirmata
type: kubernetes.io/service-account-token
```
</details>

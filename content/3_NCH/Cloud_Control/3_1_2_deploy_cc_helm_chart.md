---
title: "Deploy the Cloud Control Helm Chart"
chapter: false
weight: 32
---

Deploy the cloud controller Helm chart into your EKS cluster:

Create a `values.yaml` file with the following information:
```bash
scanner:
  primaryAWSAccountConfig:
    accountID: "your-account-id"
    accountName: "cloud-control-demo"
    regions: ["us-west-1","us-east-1"] # insert any other regions
    services: ["EKS","ECS","Lambda"] # insert services to scan
```

Refer to the complete list of fields for Helm values [here](https://github.com/nirmata/kyverno-charts/tree/main/charts/cloud-controller#values).

{{% notice note %}}
The services provided in values.yaml should be accessible by the cloud controller. Make sure to attach appropriate IAM policy when creating the IAM Role above.
{{% /notice %}}

```bash
helm repo add nirmata https://nirmata.github.io/kyverno-charts
helm repo update nirmata
helm install cloud-controller nirmata/cloud-controller --create-namespace --namespace nirmata -f values.yaml
```

### Verify Installation

Verify that the cloud controller pods are running in the ***nirmata*** namespace:

```bash
kubectl get pods -n nirmata
```

The output should display the running pods:
![Nirmata Cloud Control Pods](/images/nirmata-cloud-control-pods.png)
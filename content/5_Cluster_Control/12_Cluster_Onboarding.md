---
title: "Cluster onboarding" 
chapter: false
weight: 52 
---

To onboard a cluster with Nirmata,
 Click on the **Add Cluster** button on the *Clusters* panel.


Enter the cluster name (required) and labels (optional).

![image](/images/add_cluster_1.png)

After entering the cluster information, click on **Select Compliance Standards** to proceed to the next step.

Click on **Next** and run the below helm commands 

```bash
helm repo add nirmata https://nirmata.github.io/kyverno-charts/
helm repo update nirmata

```

```bash
helm install kyverno nirmata/kyverno -n kyverno --create-namespace --set features.policyExceptions.namespace="kyverno" --set features.policyExceptions.enabled=true --set admissionController.replicas=3 --version 3.3.9


helm install kyverno-operator nirmata/nirmata-kyverno-operator -n nirmata-system --create-namespace --devel --set enablePolicyset=true --version v0.5.8 --set "policies.policySets=[]"


helm install nirmata-kube-controller nirmata/nirmata-kube-controller -n nirmata --create-namespace \
  --set nirmataURL=wss://nirmata.io/tunnels \
  --set cluster.name=<cluster-name> \
  --set namespace=nirmata \
  --set apiToken=7n**H**CAAW9DA== \
  --set features.policyExceptions.enabled=true \
  --set features.policySets.enabled=true

```
  
You can now log in to the cluster view page and see the cluster connected in Nirmata 


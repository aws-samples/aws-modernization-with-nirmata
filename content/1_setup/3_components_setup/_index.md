---
title: "Components setup" 
chapter: false
weight: 30
---

Next, let's make sure you are all setup with Cloud9 or your own IDE.

- Get familiar how to setup and work with IDE following [Navigating Labs](https://www.eksworkshop.com/docs/introduction/navigating-labs) instructions
- Setup Cloud9\IDE and add additional components and EKS cluster following [Getting Started](https://www.eksworkshop.com/docs/introduction/getting-started/). Installing the sample application is optional!
- Install Helm CLI following [Helm lab](https://www.eksworkshop.com/docs/introduction/helm/)

Install Helm

```sh
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
```

Verify Helm Install

```sh
helm version --short
```

Let's check if our cluster is ready!

- Confirm your nodes
```
kubectl get nodes # if we see our 3 nodes, we know we have authenticated correctly
```

- Verify AWS auth map
```
kubectl describe configmap -n kube-system aws-auth
```

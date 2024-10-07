---
title: "Components setup" 
chapter: false
weight: 30
---

Next, let's make sure you are all setup with Cloud9 or your own IDE.

- Get familiar how to setup and work with Cloud IDE following [Navigating Labs](https://www.eksworkshop.com/docs/introduction/navigating-labs) instructions and prepare the environment
- Install Helm CLI following [Helm lab](https://www.eksworkshop.com/docs/introduction/helm/)

#### Verify Helm Install

```sh
helm version --short
```

#### Check your cluster is ready and confirm your nodes

```
kubectl get nodes # if we see our 3 nodes, we know we have authenticated correctly
```

{{% notice note %}}
If you receive _"couldn't get current server API group"_ please make sure: 1\ you completed eksctl section and see _"EKS cluster eks-workshop in xxxxx region is ready"_ 2\ you ran `prepare-environment` command in the end of Navigating labs section
{{% /notice %}}


#### Verify AWS auth map
```
kubectl describe configmap -n kube-system aws-auth
```

---
title: "Components setup" 
chapter: false
weight: 30
---

Next, let's make sure you are all setup with Cloud9 or your own IDE.

- Get familiar how to setup and work with IDE following [Navigating Labs](https://www.eksworkshop.com/docs/introduction/navigating-labs) instructions
- Setup Cloud9\IDE and add additional components and EKS cluster following [Getting Started](https://www.eksworkshop.com/docs/introduction/getting-started/). Installing the sample application is optional!
- Install Helm CLI following [Helm lab](https://www.eksworkshop.com/docs/introduction/helm/)

Let's check we are ready!

- Confirm your nodes
```
kubectl get nodes # if we see our 3 nodes, we know we have authenticated correctly
```

- Verify AWS auth map
```
kubectl describe configmap -n kube-system aws-auth`
```
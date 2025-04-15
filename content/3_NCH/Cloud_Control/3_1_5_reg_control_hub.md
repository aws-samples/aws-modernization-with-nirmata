---
title: "Registering to Nirmata Control Hub"
chapter: false
weight: 35
---

Install the Nirmata kube-controller Helm chart to send policy reports to Control Hub.
```bash
helm install nirmata-kube-controller nirmata/nirmata-kube-controller -n nirmata --create-namespace --set cluster.name=cloud-control --set namespace=nirmata --set apiToken=<api-token>
```

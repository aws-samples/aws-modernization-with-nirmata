---
title: "Registering to Nirmata Control Hub"
chapter: false
weight: 35
---

Install the Nirmata kube-controller Helm chart to send policy reports to Control Hub.
```bash
helm install nirmata-kube-controller nirmata/nirmata-kube-controller -n nirmata --create-namespace --set cluster.name=cloud-control --set namespace=nirmata --set apiToken=<api-token>
```

Once registered, Nirmata Control Hub will onboard the cloud account and all policy reports will be displayed in a single dashboard.

Go to _Nirmata Control Hub > Policy Reports > Cloud > cloud-control-workshop_

![Nirmata Control Hub - CCP](/images/nch-ccp-workshop.png)


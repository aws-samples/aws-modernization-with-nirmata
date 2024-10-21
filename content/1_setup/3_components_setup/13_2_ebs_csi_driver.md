---
title: "Deploy Amazon EBS CSI driver"
chapter: false
weight: 60
---

### Deploy the Amazon EBS CSI Driver

{{% notice note %}}
The [Amazon Elastic Block Store (Amazon EBS) Container Storage Interface (CSI) driver](https://www.eksworkshop.com/beginner/170_statefulset/ebs_csi_driver/#:~:text=Amazon%20Elastic%20Block%20Store%20(Amazon%20EBS)%20Container%20Storage%20Interface%20(CSI)%20driver) provides a [CSI interface](https://www.eksworkshop.com/beginner/170_statefulset/ebs_csi_driver/#:~:text=The%20Container%20Storage%20Interface) that allows Amazon Elastic Kubernetes Service (Amazon EKS) clusters to manage the lifecycle of Amazon EBS volumes for persistent volumes. This step is required if your Amazon EKS cluster version is 1.23 and above.
{{% /notice %}}


In order to utilize Amazon EBS volumes with dynamic provisioning on our EKS cluster, we need to confirm that we have the EBS CSI Driver installed. The Amazon Elastic Block Store (Amazon EBS) Container Storage Interface (CSI) driver allows Amazon Elastic Kubernetes Service (Amazon EKS) clusters to manage the lifecycle of Amazon EBS volumes for persistent volumes.

To improve security and reduce the amount of work, you can manage the Amazon EBS CSI driver as an Amazon EKS add-on. The IAM role needed by the addon was created for us so we can go ahead and install the addon:

```bash

aws eks create-addon --cluster-name $EKS_CLUSTER_NAME --addon-name aws-ebs-csi-driver \
  --service-account-role-arn $EBS_CSI_ADDON_ROLE \
  --configuration-values '{"defaultStorageClass":{"enabled":true}}' &&
aws eks wait addon-active --cluster-name $EKS_CLUSTER_NAME --addon-name aws-ebs-csi-driver
```
Now we can take a look at what has been created in our EKS cluster by the addon. For example, a DaemonSet will be running a pod on each node in our cluster:

```bash
kubectl get daemonset ebs-csi-node -n kube-system
```
Example Output:
![Example kubectl output for EBS CSI node](/images/eks-example-kubectl-output-ebs-csi.png)

Great! Lets move on to setting up our [Nirmata Account](../14_nirmata_account.html).

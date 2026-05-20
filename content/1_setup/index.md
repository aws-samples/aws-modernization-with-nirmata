---
title: "Prerequisites"
chapter: true
weight: 1
---

## Preparing for the Workshop

The workshop infrastructure will be automatically deployed using AWS CloudFormation for AWS Guided Events or can be manually deployed using provided Cloud Formation (CFN) templates in your own account. It includes the EKS cluster and all foundational services needed to provision EKS cluster with all pre-deployed resources for the Nirmata solution on the demo cluster.

**Choose your setup path:**

- **AWS Guided Event**: If you're attending an AWS-hosted workshop, follow the setup instructions [here](/1-setup/1-aws-event.html).
- **Self-Paced Learning**: If you're running this workshop independently, follow the setup instructions [here](/1-setup/2-self-paced.html).

## What gets deployed during the workshop?

During the workshop the following infrastructure will be provisioned:

✅ **[Amazon Elastic Kubernetes Service](https://aws.amazon.com/eks/) (Amazon EKS)** cluster with EKS Auto Mode enabled  
✅ **VS Code on EC2** for AWS events, optional for self-paced lab

## What will you configure during the workshop?

🔧 **Nirmata Account** and generate new API token  
🔧 **Nirmata Command Line Interface** with your Nirmata API token  
🔧 **Nirmata Controller** for k8s resources validation hosted on EKS 

::alert[**Cost Management**: If you are running this workshop on your own AWS account, remember to delete all resources by following the [Clean Up Resources](/8_cleanup.html) section to avoid unnecessary charges. The workshop is designed to use cost-effective instance types and auto-scaling to minimize costs during the learning experience.]{type="warning"}

### Ready to Begin?
Select your setup path above to start securing your workloads!

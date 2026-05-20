---
title: "Self-paced"
chapter: false
weight: 30
---

If you prefer to run the workshop in your own account and\or at your own pace, please follow below instructions. 

:::alert 
Provisioning this workshop environment in your AWS account will create resources and **there will be cost associated with them**. The cleanup section provides a guide to remove them, preventing further charges.
:::

::alert[If you don't have AWS account and want to create one, please follow [Create AWS account](12_new_aws_account.html)]{type="info"}

Please follow the instructions at EKS workshop to:
- [Setup in your AWS account](https://www.eksworkshop.com/docs/introduction/setup/your-account/) to provision resources needed for current workshop (permissions for EKS etc)
- Provision EKS cluster using [Cloud Formation Teplate](https://ws-assets-prod-iad-r-cmh-8d6e9c21a4dec77d.s3.us-east-2.amazonaws.com/6b581be8-653d-4ead-9635-58dc2e247ab0/eks-cluster.yaml)
- Provision VS Code with pre-setup tools using [Cloud Formation Teplate](https://ws-assets-prod-iad-r-cmh-8d6e9c21a4dec77d.s3.us-east-2.amazonaws.com/6b581be8-653d-4ead-9635-58dc2e247ab0/code-editor.yaml) - optional, you are welcome to use local setup and tools

::alert[**Estimated Deployment Time**: The CloudFormation stack takes approximately 15-20 minutes to deploy all infrastructure components. You can monitor progress in the AWS CloudFormation console.]{type="info"}

Once completed, please navigate to the [Getting started with Nirmata](../../3_nch.html) for next steps!


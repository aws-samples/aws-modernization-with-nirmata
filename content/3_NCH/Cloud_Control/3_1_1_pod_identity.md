---
title: "Enable Pod Identity"
chapter: false
weight: 31
---

### Enable Pod Identity Add-on
Enable the EKS Pod Identity Addon to allow seamless access to AWS resources without using explicit AWS credentials. You can enable this addon through either the AWS Management Console or the AWS CLI.

* Via AWS Management Console
  * Select your cluster
  * Go to the _Add-ons_ tab
  * Choose _Add Add-on_, search for **Pod Identity** and select it
  * Follow the prompts to complete the setup

* Via AWS CLI
Run the following command to enable the Pod Identity Addon:
```bash
aws eks create-addon --cluster-name eks-workshop --addon-name eks-pod-identity-agent --addon-version v1.0.0-eksbuild.1
```

### Create an IAM Role for Scanner Pods

- Make sure your assumed role is **WSParticipantRole/Participant**. If it's not, grab AWS CLI credentials from the event page (*Get AWS CLI Credentials* on the left panel and copy bash commands to your IDE terminal)
```bash
aws sts get-caller-identity
```

- Create `nirmatarole.json` file for IAM role CloudFormation stack. Insert the IAM role definition which has trust policy to allow the EKS Pod Identity Agent to assume the role and necessary permissions required for scanning AWS resources with Nirmata Control Hub.
```json
{
  "Resources": {
      "NirmataControlHubRole": {
          "Type": "AWS::IAM::Role",
          "Properties": {
              "RoleName": "NirmataControlHubRole",
              "AssumeRolePolicyDocument": {
                  "Version": "2012-10-17",
                  "Statement": [
                      {
                          "Sid": "AllowEksAuthToAssumeRoleForPodIdentity",
                          "Effect": "Allow",
                          "Principal": {
                              "Service": "pods.eks.amazonaws.com"
                          },
                          "Action": [
                              "sts:AssumeRole",
                              "sts:TagSession"
                          ]
                      }
                  ]
              },
              "Policies": [
                  {
                      "PolicyName": "root",
                      "PolicyDocument": {
                          "Version": "2012-10-17",
                          "Statement": [
                              {
                                  "Effect": "Allow",
                                  "Action": [
                                      "ecs:Describe*",
                                      "ecs:List*",
                                      "ecs:Get*"
                                  ],
                                  "Resource": "*"
                              },
                              {
                                  "Effect": "Allow",
                                  "Action": [
                                      "lambda:List*",
                                      "lambda:Get*"
                                  ],
                                  "Resource": "*"
                              },
                              {
                                  "Effect": "Allow",
                                  "Action": [
                                      "eks:Describe*",
                                      "eks:List*"
                                  ],
                                  "Resource": "*"
                              },
                              {
                                  "Effect": "Allow",
                                  "Action": [
                                      "cloudformation:ListResources",
                                      "cloudformation:GetResource"
                                  ],
                                  "Resource": "*"
                              }
                          ]
                      }
                  }
              ]
          }
      }
  }
}
```

- Create CFN stack via AWS CLI
```bash
aws cloudformation create-stack --stack-name nirmatarole --template-body file://nirmatarole.json --capabilities CAPABILITY_IAM CAPABILITY_NAMED_IAM
```

- Retrieve NirmataControlHubRole ARN
```bash
aws iam get-role --role-name NirmataControlHubRole
```

Depending on which services you want to scan, provide the necessary read access (List* & Get*). For example, to scan all Lambda services, provide the following read permissions to the cloud controller. You can add similar permissions for S3, SQS, EKS, etc.

{{% notice note %}}
In this workshop, we will scan ECS clusters, ECS TaskDefinition, Lambda functions, and EKS Clusters.
{{% /notice %}}

### Create Pod Identity Association

Associate the IAM role created earlier with the existing service account ***cloud-controller-scanner*** in the ***nirmata*** namespace. Replace **<IAM_ROLE_ARN>** with the ARN of the IAM role created earlier. Use the AWS CLI:

```bash
aws eks create-pod-identity-association \
  --cluster-name eks-workshop \
  --role-arn <IAM_ROLE_ARN> \
  --namespace nirmata \
  --service-account cloud-controller-scanner
```

{{% notice info %}}
The pod-identity association must be done before installing the Helm chart. If the Helm chart is already installed, restart the pods to ensure Pod Identity works correctly.
{{% /notice %}}
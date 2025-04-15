---
title: "Setup Cloud Control Point"
chapter: true
weight: 30
---

## Setup Cloud Controller
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

- Make sure your assumed role is *WSParticipantRole/Participant*. If it's not, grab AWS CLI credentials from the event page (*Get AWS CLI Credentials* on the left panel and copy bash commands to your IDE terminal)
```bash
aws sts get-caller-identity
```

- Create JSON file for CloudFormation stack
```bash
touch nirmatarole.json
```

- Open nirmatarole.json file in IDE or terminal and insert the IAM role definition which has trust policy to allow the EKS Pod Identity Agent to assume the role and necessary permissions required for scanning AWS resources with Nirmata Control Hub.
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

{{% notice note %}}
Depending on which services you want to scan, provide the necessary read access (List* & Get*). For example, to scan all Lambda services, provide the following read permissions to the cloud controller. You can add similar permissions for S3, SQS, EKS, etc.

>Note: In this workshop, we will scan ECS clusters, ECS TaskDefinition, Lambda functions, and EKS Clusters.
{{% /notice %}}

- Create CFN stack via AWS CLI
```bash
aws cloudformation create-stack --stack-name nirmatarole --template-body file://nirmatarole.json --capabilities CAPABILITY_IAM CAPABILITY_NAMED_IAM
```

- Retrieve NirmataControlHubRole ARN
```bash
aws iam get-role --role-name NirmataControlHubRole
```

### Create Pod Identity Association

Associate the IAM role created earlier with the existing service account `cloud-controller-scanner` in the `nirmata` namespace. Use the AWS CLI:

```bash
aws eks create-pod-identity-association \
  --cluster-name eks-workshop \
  --role-arn <IAM_ROLE_ARN> \
  --namespace nirmata \
  --service-account cloud-controller-scanner
```

Replace `<IAM_ROLE_ARN>` with the ARN of the IAM role created earlier.

> **Important:** The pod-identity association must be done before installing the Helm chart. If the Helm chart is already installed, restart the pods to ensure Pod Identity works correctly.

## Deploy the Cloud Control Helm Chart

Deploy the cloud controller Helm chart into your EKS cluster:

Create a `values.yaml` file with the following information:
```bash
scanner:
  primaryAWSAccountConfig:
    accountID: "your-account-id"
    accountName: "cloud-control-demo"
    regions: ["us-west-1","us-east-1"] # insert any other regions
    services: ["EKS","ECS","Lambda"] # insert services to scan
```

Refer to the complete list of fields for Helm values [here](https://github.com/nirmata/kyverno-charts/tree/main/charts/cloud-controller#values).

>NOTE: The services provided in values.yaml should be accessible by the cloud controller. Make sure to attach appropriate IAM policy when creating the IAM Role above.

```bash
helm repo add nirmata https://nirmata.github.io/kyverno-charts
helm repo update nirmata
helm install cloud-controller nirmata/cloud-controller \ 
  --create-namespace \ 
  --namespace nirmata \ 
  -f values.yaml
```

## Verify Installation

Verify that the cloud controller pods are running in the `nirmata` namespace:

```bash
kubectl get pods -n nirmata
```

The output should display the running pods:

```bash
NAME                                                  READY   STATUS    RESTARTS   AGE
cloud-control-admission-controller-dfd7f69fd-jjhn5   1/1     Running   0          17d
cloud-control-reports-controller-7954bb477d-lb2ld    1/1     Running   0          17d
cloud-control-scanner-7756dc6ddf-qljbb               1/1     Running   0          17d
```

## Cloud Admission Controller

This section provides a step-by-step guide on how to use the admission controller to intercept AWS requests and apply policies to them.

### Setting up a Proxy

To intercept AWS requests, we need a proxy server that listens on a specific port. The proxy server will apply the policies to the requests and forward them to the AWS cloud if they are compliant.

As part of the helm installation, this proxy server is automatically created for us and runs within the EKS Cluster, listening on port 8443. To use this proxy from your local machine, you need to establish a connection between your local port 8443 and the proxy server's port 8443 within the cluster. This is achieved using port forwarding.

```bash
kubectl port-forward svc/cloud-controller-admission-controller-svc 8443:8443 -n nirmata
```

By running this command, any traffic sent to `localhost:8443` on your machine will be forwarded to the proxy server in the cluster. 
This allows you to interact with the proxy and, consequently, enforce your policies on AWS requests as if the proxy server was running locally.

### Using the AWS CLI

You need to configure your AWS CLI to route requests through the proxy server.
This involves setting two environment variables:

1. `HTTPS_PROXY`: This tells the AWS CLI to send all requests through the controller acting as a local proxy.

    ```bash
    export HTTPS_PROXY=http://localhost:8443
    ```

2. `AWS_CA_BUNDLE`: The controller uses a self-signed security certificate. This variable tells the AWS CLI to trust that certificate. 
   
    First, download the certificate:
    
    ```bash
    kubectl get secrets -n nirmata cloud-control-admission-controller-svc.nirmata.svc.tls-ca -o jsonpath="{.data.tls\.crt}" | base64 --decode > ca.crt
    ```
    
    Then, set the environment variable:

    ```bash
    export AWS_CA_BUNDLE=ca.crt
    ```

    It tells the AWS CLI which Certificate Authority (CA) to trust for verifying the proxy's SSL certificate. 
    Because the cloud admission controller is using a self-signed certificate (not issued by a publicly trusted CA), 
    the AWS CLI won't trust it by default. By setting `AWS_CA_BUNDLE` to the path of the controller's CA certificate (ca.crt), 
    you're explicitly telling the AWS CLI that this certificate is valid and should be used to establish a secure connection with the proxy. 
    Without this, the AWS CLI would reject the connection due to the untrusted certificate.

Once configured, your AWS CLI commands will be checked against the defined policies before being sent to AWS.

### Example: Creating an ECS Cluster

The following examples demonstrate how the admission controller enforces a policy requiring all ECS clusters to have a `group` tag.

1. Create an ECS cluster without the `group` tag:

    ```bash
    aws ecs create-cluster --cluster-name bad-cluster
    ```

    The output should be similar to the following:

    ```
    An error occurred (406) when calling the CreateCluster operation: ecs-cluster.check-tags bad-cluster: -> A 'group' tag is required
    -> all[0].check.data.(tags[?key=='group'] || `[]`).(length(@) > `0`): Invalid value: false: Expected value: true
    ```

    As expected, the request was blocked since it violates the ValidatingPolicy that requires all ECS clusters to have the `group` tag.

2. Create an ECS cluster with the `group` tag:

    ```bash
    aws ecs create-cluster --cluster-name good-cluster --tags key=group,value=test key=owner,value=test
    ```

    The output should be similar to the following:

    ```
    {
        "cluster": {
            "clusterArn": "arn:aws:ecs:us-east-1:844333597536:cluster/good-cluster",
            "clusterName": "good-cluster",
            "status": "ACTIVE",
            "registeredContainerInstancesCount": 0,
            "runningTasksCount": 0,
            "pendingTasksCount": 0,
            "activeServicesCount": 0,
            "statistics": [],
            "tags": [
                {
                    "key": "owner",
                    "value": "test"
                },
                {
                    "key": "group",
                    "value": "test"
                }
            ],
            "settings": [
                {
                    "name": "containerInsights",
                    "value": "disabled"
                }
            ],
            "capacityProviders": [],
            "defaultCapacityProviderStrategy": []
        }
    }
    ```

    The request was successful since it complies with the ValidatingPolicy that requires all ECS clusters to have the `group` tag.

### ECS Clusters and Task Definitions

To test the scanner, we will create ECS resources, both compliant and non-compliant with the ValidatingPolicies that check the `group` tag, by creating ECS clusters and task definitions, some with and some without the required `group` tag.

1. Create an ECS cluster named `bad-cluster` without the `group` tag:

    ```bash
    aws ecs create-cluster --cluster-name bad-cluster
    ```

2. Register a task definition named `bad-task` without the `group` tag:

    ```bash
    aws ecs register-task-definition \
    --family bad-task \
    --container-definitions '[{"name": "my-app", "image": "nginx:latest", "essential": true, "portMappings": [{"containerPort": 80, "hostPort": 80}]}]' \
    --requires-compatibilities FARGATE \
    --cpu 256 \
    --memory 512 \
    --network-mode awsvpc
    ```

3. Create an ECS cluster named `good-cluster` with the `group` tag:

    ```bash
    aws ecs create-cluster --cluster-name good-cluster --tags key=group,value=development
    ```

4. Register a task definition named `good-task` with the `group` tag:

    ```bash
    aws ecs register-task-definition \
    --family good-task \
    --container-definitions '[{"name": "my-app", "image": "nginx:latest", "essential": true, "portMappings": [{"containerPort": 80, "hostPort": 80}]}]' \
    --requires-compatibilities FARGATE \
    --cpu 256 \
    --memory 512 \
    --network-mode awsvpc \
    --tags '[{"key": "group", "value": "production"}]'
    ```

## Registering to Nirmata Control Hub
Install the Nirmata kube-controller Helm chart to send policy reports to Control Hub.
```bash
helm install nirmata-kube-controller nirmata/nirmata-kube-controller -n nirmata --create-namespace  --set cluster.name=cloud-control --set namespace=nirmata --set apiToken=<api-token>
```

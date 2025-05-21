---
title: "Cluster onboarding" 
chapter: false
weight: 52 
---

# Onboarding Your EKS Cluster to Nirmata

In this section, you'll onboard your EKS cluster to Nirmata for policy management and compliance monitoring.

## Step 1: Add Your Cluster in Nirmata

Click on the **Add Cluster** button on the *Clusters* panel.

Enter the cluster name as `eks-workshop` (required) and labels (optional).

![image](/images/add_cluster_1.png)

After entering the cluster information, click on **Select Compliance Standards** to proceed to the next step.

## Step 2: Install Required Helm Charts

Click on **Next** and run the following Helm commands in your VS Code terminal:

```bash
helm repo add nirmata https://nirmata.github.io/kyverno-charts/
helm repo update nirmata

helm install kyverno nirmata/kyverno -n kyverno --create-namespace \
  --set features.policyExceptions.namespace="kyverno" \
  --set features.policyExceptions.enabled=true \
  --set admissionController.replicas=3 \
  --version 3.3.9

helm install kyverno-operator nirmata/nirmata-kyverno-operator -n nirmata-system \
  --create-namespace --devel \
  --set enablePolicyset=true \
  --version v0.5.8 \
  --set "policies.policySets=[]"
```

## Step 3: Set Your API Token

First, you need to get your API token from the Nirmata UI. Click on your profile icon in the top-right corner and select "My Profile" > "API Tokens".

![Copy API Token](/images/api_token_copy.png)

### **🔹 Prompting for Nirmata API Token**

Run the following command to set up your API token:

```bash
export CLUSTER_NAME="eks-workshop"
export NIRMATA_TOKEN=$(read -s -p "Enter your Nirmata API token: " token; echo $token)

# Display first and last few characters of the token for verification
echo "Cluster name set to: $CLUSTER_NAME"
echo "API token stored securely (first/last chars: ${NIRMATA_TOKEN:0:3}...${NIRMATA_TOKEN: -3})"
```

{{% notice tip %}}
When pasting your API token, you won't see any characters appear in the terminal due to the secure input mode. Simply paste your token and press Enter once. The command will then show the first and last 3 characters of your token for verification.
{{% /notice %}}

{{% notice warning %}}
Make sure to use `eks-workshop` as your cluster name in the Nirmata UI to match the configuration above. Using a different name will cause errors in the connection process.
{{% /notice %}}

## Step 4: Install the Nirmata Controller

Now, install the Nirmata controller with the following command that uses your API token from the environment variable:

```bash
helm install nirmata-kube-controller nirmata/nirmata-kube-controller -n nirmata --create-namespace \
  --set nirmataURL=wss://nirmata.io/tunnels \
  --set cluster.name=${CLUSTER_NAME} \
  --set namespace=nirmata \
  --set apiToken=${NIRMATA_TOKEN} \
  --set features.policyExceptions.enabled=true \
  --set features.policySets.enabled=true
```

## Step 5: Verify Cluster Connection

After running the commands above, you can now log in to the cluster view page in Nirmata and verify that your cluster is connected successfully.

You should see your cluster status as "Connected" and the Kyverno components installed and running.

---
title: "Cluster onboarding" 
chapter: false
weight: 52 
---

In this section, you'll onboard your EKS cluster to Nirmata for policy management and compliance monitoring.

## Step 1: Add Your Cluster in Nirmata

Click on the **Add Cluster** button on the *Clusters* panel.

Enter the cluster name as `demo-aws-cluster` (required) and labels (optional).

![image](/static/images/add_cluster_1.png)

After entering the cluster information, click on **Add Cluster** to proceed to the next step.

## Step 2: Onboard cluster 

There are 2 ways to onboard the cluster - using NCTL tool or using helm chart (both tools are pre-installed on VS Code environment). We will use helm. Simply copy commands and run it in VS Code terminal. Once completed, click **I Have Run The Commands - Verify Kyverno**

![image](/static/images/add_cluster_2.png)

## Step 3: Verify Kyverno setup

You should receive the message Kyverno detection is complete. Now you can move to **Select Policy Sets**

![image](/static/images/add_cluster_3.png)

## Step 4: Select recommended policy sets

Nirmata detects the minimum recommended policy sets which you can leave selected for the workshop. For your cluster setup, please follow your organization security and compliance guidelines. Click **Apply Policy Sets** to complete onboarding.

![image](/static/images/add_cluster_4.png)

## Step 5: Verify policy status

NCH will perform policy sets deployment and verification of policy sets status. 

![image](/static/images/add_cluster_5.png)
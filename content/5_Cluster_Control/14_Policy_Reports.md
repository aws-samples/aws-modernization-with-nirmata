---
title: "Policy Reports" 
chapter: false
weight: 54 
---

Policy Reports are automatically generated for clusters and namespaces. For this we will first deploy policies using Policy set in Nirmata

Navigate to Policy set in Nirmata and click on **Add Policy set**.  It is not mandatory to use Nirmata’s curated policy sets. Create a custom policy set by clicking on Add Custom Policy Set. This option provides more control over the lifecycle of the underlying policies.

The page displays two options to create a policy set: Git(recommended) and YAML

Click on Git and add the settings for the repo like below: 

**Repo**: https://github.com/nirmata/demo-ws-nirmata 

**Branch**: main

**Path**: /Docs-and-Guides/Nirmata-policies/restricted-psp-audit/

Rest can be default, and click on **ok**

You can now add a cluster to the policy set by clicking and selecting **Add cluster** in the top right corner. Monitor the policies deployed, and we are all set to view the policy reports.

**Note** The polices are configured for nginx and demo namespace and do not show the violation for all the resources.. Create both the namespace and deploy some [sample insecure workloads](https://github.com/nirmata/demo-ws-nirmata/tree/main/Docs-and-Guides/Nirmata-policies/insecure%20workloads) to test the violations 

**To access the Policy Reports:**
1. Go to **Policies**>**Policy Reports**. The Policy Reports can be viewed based on Categories, Clusters, or Namespaces.

![image](/images/policy_reports.png)

2. To view the Policy Reports in any one category, click on the Category Name (example, **RBAC Best Practices**) link. The findings in this category will be displayed with information related to *Severity, Findings, Impact (Clusters and Resources), and Status (%Pass or Fail)*.

![image](/images/rbac_best_practices.png)

3. To view detailed report of a particular finding, click on any **Findings** link to view the details and its impact. The details contain violation and policy information as the policy name, rule name, severity of the violation, and other metadata. The page also lists the impacted resources for this finding.

![image](/images/policy_findings.png)

You can also **Schedule report** or assign it to a user using the **Jira integration** 

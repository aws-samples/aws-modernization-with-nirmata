---
title: "Policy Sets and Policy Reports" 
chapter: false
weight: 54 
---

## Adding Policy Set

It is not mandatory to use Nirmata’s curated policy sets. Navigate to *Policy Sets* and click on **Add Policy set** -> *Add Custom Policy Set*. This option provides more control over the lifecycle of the underlying policies.

![image](/static/images/policy_sets.png)

The page displays two options to create a policy set: **Git**(recommended) and **YAML**

![image](/static/images/policy_set_source.png)

Click on Git and add the settings for the repo like below: 
* **Name**: ``custom-demo-policies``
* **Repository**: https://github.com/nirmata/demo-ws-nirmata 
* **Branch**: `main`
* **Path**: `/Docs-and-Guides/Nirmata-policies/restricted-psp-audit/`

You can leave the rest as default, and click on **Create**.


You can now add a cluster to the policy set by clicking on the top right corner of the policy set, and navigate to **Clusters**, and selecting **Add cluster** in the top right corner. 

![image](/static/images/policy_sets_clusters.png)

![image](/static/images/policy_set_add_cluster.png)

![image](/static/images/policy_set_add_cluster_selection.png)

Monitor the policies deployed, and we are all set to view the policy reports.

![image](/static/images/policy_set_cluster_deployment_status.png)

::alert[The polices are configured for nginx and demo namespace and do not show the violation for all the resources.. Create both the namespace and deploy some [sample insecure workloads](https://github.com/nirmata/demo-ws-nirmata/tree/main/Docs-and-Guides/Nirmata-policies/insecure%20workloads) to test the violations.]{type="note"}

**To access the Policy Reports:**
1. Go to **Policies**>**Policy Reports**. The Policy Reports can be viewed based on Categories, Clusters, Namespaces, or Repositories.

![image](/static/images/policy_reports.png)

2. To view the Policy Reports in any one category, click on the Category Name (example, **RBAC Best Practices**) link. The findings in this category will be displayed with information related to *Severity, Findings, Impact (Clusters and Resources), and Status (%Pass or Fail)*.

![image](/static/images/rbac_best_practices.png)

3. To view detailed report of a particular finding, click on any **Findings** link to view the details and its impact. The details contain violation and policy information as the policy name, rule name, severity of the violation, and other metadata. The page also lists the impacted resources for this finding.

![image](/static/images/policy_findings.png)

You can also:
- [Schedule report](https://docs.nirmata.io/docs/npmk/workflows/auto_report_scheduling/)
- Assign a violation to a user using the [Jira integration](https://docs.nirmata.io/docs/npmk/policy_reports/create_jira_tickets/)
- Or start working on [AI Remediations](https://docs.nirmata.io/docs/npmk/remediations/).

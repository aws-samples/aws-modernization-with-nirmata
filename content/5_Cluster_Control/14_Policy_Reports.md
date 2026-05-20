---
title: "Policy Sets and Policy Reports" 
chapter: false
weight: 54 
---

Policy Sets are collections of Kyverno policies that define governance rules for your clusters. They can be curated by Nirmata or custom-built from your own Git repositories. Policy Reports provide visibility into compliance status by showing which resources pass or violate the applied policies.

## Adding Policy Set

In addition to the recommended policy sets applied during onboarding, you can create custom policy sets for more granular control. Navigate to **Policies** > **Policy Sets** in the left navigation pane and click on **Add Policy set** -> *Add Custom Policy Set*. This option provides more control over the lifecycle of the underlying policies.

:image[Add Custom Policy Set]{src="/static/images/policy_sets.png" width=900}

The page displays two options to create a policy set: **Git**(recommended) and **YAML**

:image[Policy Set source selection - Git or YAML]{src="/static/images/policy_set_source.png" width=900}

Click on Git and add the settings for the repo like below: 
* **Name**: ``custom-demo-policies``
* **Repository**: https://github.com/nirmata/demo-ws-nirmata 
* **Branch**: `main`
* **Path**: `/Docs-and-Guides/Nirmata-policies/restricted-psp-audit/`

You can leave the rest as default, and click on **Create**.


You can now add a cluster to the policy set by clicking on the top right corner of the policy set, and navigate to **Clusters**, and selecting **Add cluster** in the top right corner. 

:image[Policy Sets - Clusters tab]{src="/static/images/policy_sets_clusters.png" width=900}

---

:image[Add Cluster to Policy Set]{src="/static/images/policy_set_add_cluster.png" width=900}

---

:image[Select cluster for policy set]{src="/static/images/policy_set_add_cluster_selection.png" width=900}

::alert[Policy deployment may take a few minutes to complete. Wait for all policies to show a ready status before proceeding.]{type="info"}

Monitor the policies deployed, and we are all set to view the policy reports.

:image[Policy set cluster deployment status]{src="/static/images/policy_set_cluster_deployment_status.png" width=900}

::alert[The polices are configured for nginx and demo namespace and do not show the violation for all the resources.. Create both the namespace and deploy some [sample insecure workloads](https://github.com/nirmata/demo-ws-nirmata/tree/main/Docs-and-Guides/Nirmata-policies/insecure%20workloads) to test the violations.]{type="note"}

**To access the Policy Reports:**
1. Go to **Policies**>**Policy Reports**. The Policy Reports can be viewed based on Categories, Clusters, Namespaces, or Repositories.

:image[Policy Reports overview]{src="/static/images/policy_reports.png" width=900}

2. To view the Policy Reports in any one category, click on the Category Name (example, **RBAC Best Practices**) link. The findings in this category will be displayed with information related to *Severity, Findings, Impact (Clusters and Resources), and Status (%Pass or Fail)*.

:image[RBAC Best Practices policy report]{src="/static/images/rbac_best_practices.png" width=900}

3. To view detailed report of a particular finding, click on any **Findings** link to view the details and its impact. The details contain violation and policy information as the policy name, rule name, severity of the violation, and other metadata. The page also lists the impacted resources for this finding.

:image[Policy finding details]{src="/static/images/policy_findings.png" width=900}

You can also:
- [Schedule report](https://docs.nirmata.io/docs/npmk/workflows/auto_report_scheduling/)
- Assign a violation to a user using the [Jira integration](https://docs.nirmata.io/docs/npmk/policy_reports/create_jira_tickets/)
- Or start working on [AI Remediations](https://docs.nirmata.io/docs/npmk/remediations/).

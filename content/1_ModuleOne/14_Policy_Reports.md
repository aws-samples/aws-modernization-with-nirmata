---
title: "Policy Reports" # MODIFY THIS TITLE IF APPLICABLE
chapter: true
weight: 4 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES IF APPLICABLE
---

## Cluster Policy Reports

Policy Reports are automatically generated for clusters and namespaces. There is no additional configuration required for this functionality.

**To access the Policy Reports:**
1. Go to **Policies**>**Policy Reports**. The Policy Reports can be viewed based on Categories, Clusters, or Namespaces.

![image](/images/policy_reports.png)

2. To view the Policy Reports in any one category, click on the Category Name (example, **RBAC Best Practices**) link. The findings in this category will be displayed with information related to *Severity, Findings, Impact (Clusters and Resources), and Status (%Pass or Fail)*.

![image](/images/rbac_best_practices.png)

3. To view detailed report of a particular finding, click on any **Findings** link to view the details and its impact. The details contains violation and policy information as the policy name, rule name, severity of the violation, and other metadata. The page also lists the impacted resources for this finding.

![image](/images/policy_findings.png)

4. Click on the **Scheduled Reports** button to receive the periodic email on the policy violations. To do so:<br>
   a. Click on the **+** symbol. The Schedule Email page opens.<br>
   b. Select a cluster and click **Next**.<br>
   c. Select the scope by clicking on the radio button either **Cluster** or **Namespaces**.<br>
   d. In the Sender field, enter the sender's email address.<br>
   e. In the Recipients field, enter the recipient's email address.<br>
   f. In the Subject field, enter the subject for the report.<br>
   g. In the Message field, enter the email message.<br>
   h. Click the checkbox **Schedule** and in the Schedule field, select the schedule option. The options available are: hourly, daily, weekly, and monthly.<br>
   i. Select the day and time for daily and weekly schedulings and month for monthly schedulings.<br>
   j. Click **Save**.

   ![image](/images/scheduled_policy_report.png)

{{% notice warning %}}
The examples and sample code provided in this workshop are intended to be consumed as instructional content. These will help you understand how various AWS services can be architected to build a solution while demonstrating best practices along the way. These examples are not intended for use in production environments.
{{% /notice %}}

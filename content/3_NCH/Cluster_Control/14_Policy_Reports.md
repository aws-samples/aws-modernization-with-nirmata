---
title: "Policy Reports" 
chapter: false
weight: 54 
---

Policy Reports are automatically generated for clusters and namespaces. There is no additional configuration required for this functionality.

**To access the Policy Reports:**
1. Go to **Policies**>**Policy Reports**. The Policy Reports can be viewed based on Categories, Clusters, or Namespaces.

![image](/images/policy_reports.png)

2. To view the Policy Reports in any one category, click on the Category Name (example, **RBAC Best Practices**) link. The findings in this category will be displayed with information related to *Severity, Findings, Impact (Clusters and Resources), and Status (%Pass or Fail)*.

![image](/images/rbac_best_practices.png)

3. To view detailed report of a particular finding, click on any **Findings** link to view the details and its impact. The details contains violation and policy information as the policy name, rule name, severity of the violation, and other metadata. The page also lists the impacted resources for this finding.

![image](/images/policy_findings.png)

4. Click on the **Scheduled Reports** button to receive the periodic email on the policy violations. To do so:

   a. Click on the **+** symbol. The Schedule Email page opens.

   b. Select a cluster and click **Next**.
   
   c. Select the scope by clicking on the radio button either **Cluster** or **Namespaces**.

   d. In the Sender field, enter the sender's email address.

   e. In the Recipients field, enter the recipient's email address.

   f. In the Subject field, enter the subject for the report.

   g. In the Message field, enter the email message.

   h. Click the checkbox **Schedule** and in the Schedule field, select the schedule option. The options available are: hourly, daily, weekly, and monthly.

   i. Select the day and time for daily and weekly schedulings and month for monthly schedulings.

   j. Click **Save**.

   ![image](/images/scheduled_policy_report.png)
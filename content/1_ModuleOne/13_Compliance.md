---
title: "Compliance" # MODIFY THIS TITLE
chapter: true
weight: 3 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES IF APPLICABLE
---

## Cluster Compliance

Kyverno policies map to one or more compliance controls and compliance standards. Policy violations are generated when a resource fails a policy rule. A compliance standard contains various controls or rules and the policies need to comply with the standard.

To view the Compliance report:
1. Go to **Menu**>**Compliance**. The Compliance Standards page with the default standards is displayed. On the top right corner of each of the standard, click on the Kebab Menu to view the options, **Details**, **Generate Report**, **Clone**, **Disable**, and **Delete**. In addition, the **Edit** option is displayed for a User Managed Standard. Select any of the options for the respective action.
2. Click on any of the default standard, for example, CIS Benchmarks v8. The CIS Benchmarks v8 report is displayed.

![image](/images/new_compliance1.png)
<!-- <img src="/images/compliance1.png"> -->

By default the report is for Controls. Click on the **Clusters** button to view the report for clusters. You can also view the report for All Clusters or by searching for a particular cluster in the Search field. In addition, you can filter the status and view the report for a particular status such as *pass* status.

3. Click on the control name to see the details of the control, its status, policy mappings, and findings. Similarly by clicking on the Clusters button, you can view the cluster list and clicking on the cluster name, you can view the controls associated with it. You can also click on the control name to see the details of the control.
4. In the Compliance Standards page, click on the **Add Standard** button to add a standard. A drop-down list of Nirmata Managed Standards and a menu to add User Managed Standard are displayed. When you click on the Nirmata Managed option, it gets directly added. When you click on the User Managed option, the Add Compliance Standard window opens.<br>
    a. Enter **Name, Version and Description** and click **Save**. <br>
    b. In the Controls block, you can either add a control by clicking on the **Add Control** button or import a CSV file by clicking the block, **Click to import a CSV file or drop a CSV file here**.
    >NOTE: Nirmata recommends you to add controls while adding the standard. However, you can skip this step and can add/update/delete controls later.

    c. Click on the **Download CSV Template** link to download the example template. <br>
    d. When you click on the Add Control button, the Add Compliance Control window opens. On this window, add **Control Details** such as Name, ID, SubID, Description, select **Cloud Providers** such as EKS, AKS, GKE, or Self Managed, and add **Policy Mappings** such as Policy Name, Rule, UUID, and click **Save**.

![image](/images/add_standard.png)
<!-- <img src="/images/add-compliance-standard.png"> -->

5. Click on the **Generate Report** button to generate a report on the standard. The Generate Report page opens.<br>
    a. In the Mail block, enter **Sender**, **Recipients**, **Subject**, **Message** details.<br>
    b. Select the **Schedule** checkbox to schedule the email. The options available are hourly, daily, weekly, and monthly.<br>
    c. Click on the **Save** button.
6. Click on the **Scheduled Reports** and click on the **+** symbol to schedule a periodic email to notify any policy violations. The Schedule Email window opens. <br>
    a. Select a cluster from the list or search the cluster and click **Next**. The Schedule Email page is displayed.<br>
    b. In the Scope block, select the radio button, either **Cluster or Namespaces** to choose the scope of the report.<br>
    c. In the Mail block, enter **Sender**, **Recipients**, **Subject**, **Message** details.<br>
    d. Select the **Schedule** checkbox to schedule the email. The options available are hourly, daily, weekly, and monthly.<br>
    e. Click on the **Save** button.

7. Go to **Policies**>**Policy Reports**. Click on the Kebab Menu at the right corner of the screen. The *Enable CIS Benchmarks* option is displayed.
8. Click on the **Enable CIS Benchmarks** option. A window to install kube-bench adapter is displayed. In this window you will see the instructions to execute commands to install the helm chart and run the cron job to see policy violations immediately.
>NOTE: Installing kube-bench adapter prompts the user to check policy reports and report CIS Benchmark violations on a weekly schedule.
1. Click **OK**.

![image](/images/enable_cis_benchmark1.png)

## Namespace Compliance

Compliance Report per Namespace is the compliance report for resources that is generated for a particular namespace within a cluster. A compliance standard contains various controls or rules and policies need to comply with the standard.

To view the Compliance Report per Namespace:

1. Go to **Menu**>**Policy Reports**. The Policy Reports can be viewed based on Categories, Clusters, or Namespaces.
2. Click on the **Namespace** category to view the policy reports generated for the different namespaces associated with multiple clusters. Click on the `Cluster` tab to filter the namespaces for your cluster by selecting your cluster from the dropdown.

![image](/images/namespace_report.png)

3. Next, click on any namespace associated with your cluster to view the detailed policy reports for that particular namespace. The `Findings` tab opens by default with information related to *Severity, Findings, Impact (Resources), and Status (%Pass or Fail)*.
4. After that, click on the `Compliance` tab to view the compliance report generated with the standards for that namespace of the cluster.

![image](/images/compliance_report.png)

5. View more details about the standard by clicking on the compliance card. For example, click on **Pod Security Standards - Restricted**, to view the standard report for that namespace.
6. The page contains the report for `Controls` for the given compliance standard with information related to the Control names, their status, the pass percentage, the number of fail, warn, and pass results, the type of the Controls, and whether the controls are enabled.

![image](/images/compliance_details.png)

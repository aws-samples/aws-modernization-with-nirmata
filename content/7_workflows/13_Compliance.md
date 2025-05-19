---
title: "Compliance" 
chapter: false
weight: 33 
---

## Cluster Compliance

Kyverno policies map to one or more compliance controls and compliance standards. Policy violations are generated when a resource fails a policy rule. A compliance standard contains various controls or rules and the policies need to comply with the standard.

To view the Compliance report:
1. Go to **Menu**>**Compliance**. The Compliance Standards page with the default standards is displayed. Go to any one of the standards by clicking on the `Clusters` meter like icon inside the standard.  Click on the Kebab Menu on the top right to view the options, *Details*, *Generate Report*, *Clone*, *Disable*, and *Delete*. In addition, the **Edit** option is displayed for a User Managed Standard. Select any of the options for the respective action.
2. Click on any of the default standard, for example, Pod Security Standards - Restricted. The Pod Security Standards - Restricted report is displayed.

![image](/images/new_compliance2.png)

Each standard can show the report for `Clusters`, `Repos` and `Overall` or all of them. Click on the **Clusters** button to view the report for clusters. This view shows you the different controls and their status. To check the report for all the clusters, click on the `Clusters` tab. You can also view the report for a specific cluster by searching for a particular cluster in the Search field. In addition, you can filter the status and view the report for a particular status such as *pass* status.

3. Click on the control name to see the details of the control, its status, policy mappings, and findings. 

4. In the Compliance Standards page, click on the **Add Standard** button and you will see a drop-down list of Nirmata Managed Standards and a menu to add User Managed Standard are displayed. Select `CIS Amazon Elastic Kubernetes Service (EKS) Benchmark`. Next Select your newly added cluster and click `Add`. You have now successfully added your first Compliance Standard. Lets now install the Nirmata CIS Adapter.
NOTE: Installing kube-bench adapter prompts the user to check policy reports and report CIS Benchmark violations on a weekly schedule.

    a). Installing the Nirmata CIS Adapter.
   
    The following commands add and update the Kyverno Helm chart repository:

    ```bash
    helm repo add nirmata https://nirmata.github.io/kyverno-charts/
    helm repo update nirmata
    ```

    b). Creating a namespace.
   
    It is recommended to install the CIS-Adapter in its own namespace. This documentation uses kube-bench as the namespace:
    
    ```bash
    kubectl create namespace kube-bench
    ```
    
    c). Install the CIS-Adapter for EKS from nirmata helm repo in the kube-bench namespace, with desired parameters using:

    ```bash
    helm install kube-bench-adapter nirmata/kube-bench-adapter -n kube-bench --set kubeBench.name="cis-eks-1.2.0" --set kubeBench.kubeBenchBenchmark="eks-1.2.0" --set kubeBench.namespace="kube-bench" --set     kubeBench.kubeBenchTargets="controlplane\,node\,policies\,managedservices"
    ```
    d). Verifying installation.
   
    The cronjob with weekly schedule should be created and executing the below command helps you verify that:

    ```bash
    kubectl get cronjob -n kube-bench
    ```
    e). Verify policyreports creation.
   
    Check the policyreports created through the custom resource with:

    ```bash
    kubectl get clusterpolicyreports -n kube-bench
    ```

6. When you click on the User Managed option, the Add Compliance Standard window opens.

    a. Enter *Name, Version and Description* and click **Save**. <br>
   
    b. In the Controls block, you can either add a control by clicking on the **Add Control** button or import a CSV file by clicking the block, **Click to import a CSV file or drop a CSV file here**.
    >NOTE: Nirmata recommends you to add controls while adding the standard. However, you can skip this step and can add/update/delete controls later.

    c. Click on the **Download CSV Template** link to download the example template. 

    d. When you click on the Add Control button, the Add Compliance Control window opens. On this window, add **Control Details** such as Name, ID, SubID, Description, select *Cloud Providers* such as EKS, Self Managed, and etc, and add *Policy Mappings* such as Policy Name, Rule, UUID, and click **Save**.

    ![image](/images/add_standard.png)
    
## Namespace Compliance

Compliance Report per Namespace is the compliance report for resources that is generated for a particular namespace within a cluster. A compliance standard contains various controls or rules and policies need to comply with the standard.

To view the Compliance Report per Namespace:

1. Go to **Menu**>**Policy Reports**. The Policy Reports can be viewed based on Categories, Clusters, or Namespaces.
2. Click on the **Namespace** category to view the policy reports generated for the different namespaces associated with multiple clusters. Click on the **Cluster** tab to filter the namespaces for your cluster by selecting your cluster from the dropdown.

![image](/images/namespace_report.png)

3. Next, click on any namespace associated with your cluster to view the detailed policy reports for that particular namespace. The ***Findings*** tab opens by default with information related to *Severity, Findings, Impact (Resources), and Status (%Pass or Fail)*.
4. After that, click on the **Compliance** tab to view the compliance report generated with the standards for that namespace of the cluster.

![image](/images/compliance_report.png)

5. View more details about the standard by clicking on the compliance card. For example, click on **Pod Security Standards - Restricted**, to view the standard report for that namespace.
6. The page contains the report for ***Controls*** for the given compliance standard with information related to the Control names, their status, the pass percentage, the number of fail, warn, and pass results, the type of the Controls, and whether the controls are enabled.

![image](/images/compliance_details.png)

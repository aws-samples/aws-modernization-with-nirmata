---
title: "Remediation" # MODIFY THIS TITLE
chapter: true
weight: 44 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES
---

Remediations are possible fixes that are suggested for Kubernetes resources when they have a violation against the best practices policies. They are shown in the form of a suggested resource that are generated through annotations existing within the best practices policies.

### Remediation with mutated policies

A policy remediation works in the following way:

1. A Kubernetes resource that violates against a best practices policy is defined as a bad resource.
2. The policy definition file can have an annotation `policies.nirmata.io/remediation`. The annotation contains a reference to a mutate policy that is used for remediating the bad resource. Below is an example of an annotation from a policy:

````bash
policies.nirmata.io/remediation: "https://github.com/nirmata/kyverno-policies/tree/main/pod-security/baseline/disallow-host-namespaces/remediate-disallow-host-namespaces.yaml"
````

3. NPM can then provide remediation suggestions based on the mutate policy.
>Note: It is important to annotate a ClusterPolicy with its corresponding mutate policy for the NPM remediation feature to work as expected.

![image](/images/remediation_diffs2.png)

## Remediation in Policy Reports

To view Remediations in Policy Reports:

1. Go to **Policies**>**Policy Reports**. The Policy Reports can be viewed based on collections such as Categories, Clusters, or Namespaces. Under each collection, there is a `Remediation` column that shows the number of remediations available for that category, cluster, or namespace.

2. Click on the **Remediations Available** button available beside the search bar to filter the list to only contain the rows that have remediations available.

![image](/images/cluster_policy_reports.png)

3. Click on any of the Cluster Name (example, **eks-production**) link to view the list of namespaces containing policy reports that have remediations available.

4. Click on any of the remediation available namespaces to get the list of findings. Go to the Resources tab to find the list of impacted resources in the namespace and filter the ones with remediations available by checking the `Remediations Available` button. The page also shows the namespace grade along with the total number of resources and violations associated with the resources.

![image](/images/namespace_resources.png)

5. Now, click on any of the impacted resources to view the findings related to that particular resource, their severity, status, and whether remediations are available for those findings. The page also summarizes the number of remediations available out of the violations, the namespace grade impact after the remediation is applied, and the number of affected child resources that can be fixed with the applied remediations.

![image](/images/resource_findings.png)

6. Click on the **View Remediation Diffs** button located at the top right-hand section of the screen to get a side-by-side view of the resource YAML and the remediated resource YAML. Both of the resources YAML can be downloaded by clicking on the download button.

![image](/images/remediation_diffs.png)

7. Click on any of the findings link with remediation available to view the detailed report, the remediation suggestions, and the list of impacted resources.

![image](/images/finding_details.png)

{{% notice warning %}}
The examples and sample code provided in this workshop are intended to be consumed as instructional content. These will help you understand how various AWS services can be architected to build a solution while demonstrating best practices along the way. These examples are not intended for use in production environments.
{{% /notice %}}

---
title: "Remediation" 
chapter: false
weight: 44 
---

Remediations are possible fixes that are suggested for Kubernetes resources when they have a violation against the best practices policies. They are shown in the form of a suggested resource that are generated through annotations existing within the best practices policies.

### Remediation with mutated policies

A policy remediation works in the following way:

1. A Kubernetes resource that violates against a best practices policy is defined as a bad resource.
2. The policy definition file can have an annotation `policies.nirmata.io/remediation`. The annotation contains a reference to a mutate policy that is used for remediating the bad resource. Below is an example of an annotation from a policy:

````bash
policies.nirmata.io/remediation: "https://github.com/nirmata/kyverno-policies/tree/main/pod-security/baseline/disallow-host-namespaces/remediate-disallow-host-namespaces.yaml"
````

3. NCH can then provide remediation suggestions based on the mutate policy.
>Note: It is important to annotate a ClusterPolicy with its corresponding mutate policy for the Nirmata Control Hub (NCH) remediation feature to work as expected.

:image[Remediation diffs - mutate policy example]{src="/static/images/remediation_diffs2.png" width=900}

### Remediation in Policy Reports

To view Remediations in Policy Reports:

1. Go to **Policies**>**Policy Reports**. The Policy Reports can be viewed based on collections such as Categories, Clusters, or Namespaces. Under each collection, there is a `Remediation` column that shows the number of remediations available for that category, cluster, or namespace.

2. Click on the **Remediations Available** button available beside the search bar to filter the list to only contain the rows that have remediations available.

:image[Cluster policy reports with remediations available]{src="/static/images/cluster_policy_reports.png" width=900}

3. Click on any of the Cluster Name (example, **eks-production**) link to view the list of namespaces containing policy reports that have remediations available.

4. Click on any of the remediation available namespaces to get the list of findings. Go to the Resources tab to find the list of impacted resources in the namespace and filter the ones with remediations available by checking the `Remediations Available` button. The page also shows the namespace grade along with the total number of resources and violations associated with the resources.

:image[Namespace resources with remediation filter]{src="/static/images/namespace_resources.png" width=900}

5. Now, click on any of the impacted resources to view the findings related to that particular resource, their severity, status, and whether remediations are available for those findings. The page also summarizes the number of remediations available out of the violations, the namespace grade impact after the remediation is applied, and the number of affected child resources that can be fixed with the applied remediations.

:image[Resource findings with remediation status]{src="/static/images/resource_findings.png" width=900}

6. Click on the **View Remediation Diffs** button located at the top right-hand section of the screen to get a side-by-side view of the resource YAML and the remediated resource YAML. Both of the resources YAML can be downloaded by clicking on the download button.

:image[Side-by-side remediation diffs]{src="/static/images/remediation_diffs.png" width=900}

7. Click on any of the findings link with remediation available to view the detailed report, the remediation suggestions, and the list of impacted resources.

:image[Finding details with remediation suggestions]{src="/static/images/finding_details.png" width=900}
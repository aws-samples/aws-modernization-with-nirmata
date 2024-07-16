---
title: "Cluster On-boarding" # MODIFY THIS TITLE
chapter: true
weight: 32 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES IF APPLICABLE
---

To onboard a cluster with Nirmata,
 Click on the `Add Cluster` button on the `Clusters` panel. If you are trying out NPM for the first time, it is highly recommended to use the default onboarding process instead of the manual onboarding flow.

## Cluster Onboarding
This workflow requires `nctl`. Refer to the [documentation](https://docs.nirmata.io/nctl/) for installation.

Enter the cluster name (required) and labels (optional).

![image](/images/add_cluster_1.png)
<!-- <img src="/images/add_cluster_1.png" alt="adding cluster to NPM" /> -->

After entering the cluster information, click on `Select Compliance Standards` to proceed to the next step.
The Pod Security Standards Baseline is added by default. It is highly recommended to opt for Pod Security Standards Restricted and RBAC Best Practices to improve the overall security posture of the cluster.
Select the set of policies to be configured on the cluster as default policy sets. These policies will be deployed in audit mode. After selecting the policy sets, click on `Add Cluster` to proceed to the final step.

![image](/images/add_cluster_2.png)
<!-- <img src="/images/add_cluster_2.png" alt="adding cluster to NPM" /> -->
<!-- <img src="../../images/add_cluster_2.png" width="500" /> -->

Use the `nctl login` command to login to NPM. If the token is not auto generated, visit the profile page and click on `Generate API Key` button to generate the token.

![image](/images/add_cluster_3.png)
<!-- <img src="/images/add_cluster_3.png" alt="adding cluster to NPM" /> -->
<!-- <img src="../../images/add_cluster_3.png" width="500" /> -->

Once the command has run successfully, it will display a message notifying that:
```bash
Validating user credentials...done!
Wrote configuration to /home/username/.nirmata/config
```
Next, copy the `nctl clusters add` command displayed in the final step from the web UI. Run this command to add your cluster to NPM.

After running the above command, a confirmation message will be displayed, notifying that Nirmata Operator has been deployed successfully  on the cluster. Following this, the policy sets selected in the previous step will become ready.
Next, you can click on  `I Have Run the Command` in the web UI to complete the onboarding process and navigate to the Clusters dashboard. The new cluster added can be seen in the dashboard.

![image](/images/onboarding_confirmation.png)
<!-- <img src="/images/onboarding_confirmation.png"> -->

{{% notice warning %}}
The examples and sample code provided in this workshop are intended to be consumed as instructional content. These will help you understand how various AWS services can be architected to build a solution while demonstrating best practices along the way. These examples are not intended for use in production environments.
{{% /notice %}}

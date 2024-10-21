---
title: "Cluster onboarding" 
chapter: false
weight: 32 
---

After the user account is fully activated, it’s now time to begin to configure the Kubernetes clusters that Nirmata Policy Manager (NPM) will be working with. On the opening screen, there is an introduction page explaining what NPM looks to accomplish. There are two buttons at the bottom of the page. One listed as See Features and the other listed as Get Started.

For the purposes of this tutorial, click the **Get Started** button.

The next page will list out the steps that will need to be completed. This includes installing the Nirmata Controller, connecting the cluster to NPM and then working with the systems within the framework, by monitoring the cluster details and policy violations.

![Register an Existing Kubernetes Cluster](/images/register.jpg)

## Cluster Onboarding
To onboard a cluster with Nirmata,
 Click on the **Add Cluster** button on the *Clusters* panel. If you are trying out NPM for the first time, it is highly recommended to use the default onboarding process instead of the manual onboarding flow.

This workflow requires [Nirmata Command Line Interface, nctl](https://downloads.nirmata.io/nctl/downloads/). Lets first install `nctl` on our Cloud9 instance:

```bash
curl -LO https://dl.nirmata.io/nctl/nctl_4.2.3-rc.6/nctl_4.2.3-rc.6_linux_amd64.zip
```

To unzip and add proper permissions:

```bash
unzip nctl_4.2.3-rc.6_linux_amd64.zip
chmod +x nctl
sudo mv nctl /usr/local/bin/nctl
```

Let's verify the installation:

```bash
nctl version
```

Output should look similar to below as the version may have been updated since this publishing:

```bash
nctl version
Version: 4.2.3-rc.6
Time: 2024-10-05T17:31:43Z
Git commit ID: 696cc45ea3547b5bdfd1d46b58854572596bbbed
```

You can also refer to the [documentation](https://docs.nirmata.io/docs/nctl/installation/) for installation.

Enter the cluster name (required) and labels (optional).

![image](/images/add_cluster_1.png)

After entering the cluster information, click on **Select Compliance Standards** to proceed to the next step.
The Pod Security Standards Baseline is added by default. It is highly recommended to opt for Pod Security Standards Restricted and RBAC Best Practices to improve the overall security posture of the cluster.
Select the set of policies to be configured on the cluster as default policy sets. These policies will be deployed in audit mode. After selecting the policy sets, click on **Add Cluster** to proceed to the final step.

![image](/images/add_cluster_2.png)

Use the `nctl login` command to login to NPM. If the token is not auto generated, visit the profile page and click on **Generate API Key** button to generate the token.

![image](/images/add_cluster_3.png)

Once the command has run successfully, it will display a message notifying that:
```bash
Validating user credentials...done!
Wrote configuration to /home/username/.nirmata/config
```
Next, copy the `nctl add cluster` command displayed in the final step from the web UI. Run this command to add your cluster to NPM.

After running the above command, a confirmation message will be displayed, notifying that Nirmata Operator has been deployed successfully  on the cluster. Following this, the policy sets selected in the previous step will become ready.

Next, you can click on  **I Have Run the Command** in the web UI to complete the onboarding process and navigate to the Clusters dashboard. The new cluster added can be seen in the dashboard.

![image](/images/onboarding_confirmation.png)

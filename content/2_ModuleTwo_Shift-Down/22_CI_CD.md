---
title: "CI/CD Pipelines" # MODIFY THIS TITLE
chapter: true
weight: 2 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES
---

## Pipeline Actions

The platform or security team admins are responsible for defining the policies that an organization needs to adhere to. They are stored as YAML files in Git repositories or as OCI images in the OCI registry, or also are made available as Helm charts. The DevOps user or the IT team is responsible for managing the configuration files, be it a Kubernetes manifest, or an IaC file, or any JSON spec stored in Git repositories.

User who wants to make any changes to the manifests in these repositories will create a pull request (PR). CI pipelines are configured to trigger on various actions, such as, creating a PR, merging the code to main branch, or even setup to run at regular intervals.

### Understanding the GitHub Action Workflow

A dedicated GitHub Action is available through the [GitHub marketplace](https://github.com/marketplace/actions/nctl-scan-installer). With this action, `nctl scan` can be used in the GitHub actions workflows to scan the configuration files present in the repository against the policies that are defined centrally. In case of a failure, the entire action can be configured to fail, meaning that the test pipeline will fail, and the users will get quick feedback for their changes. The results of the scan are available in NPM for viewing. NPM provides insights to platform administrators on overall governance of different code repositories in their organization.

To have a look at the workflow manifest file, refer to the `scan-outputs.yaml` file in the `.github/workflows` section of the **nctl-shift-left** [Github repository](https://github.com/nirmata/nctl-shift-left/).

Use the readily available `nctl` action.

````bash
- name: nctl-scan-installer
        uses: nirmata/action-install-nctl-scan@v0.0.5
````

Set the right environment secrets.
````bash
env:
  NIRMATA_TOKEN: ${{secrets.NIRMATA_TOKEN}}
  NIRMATA_URL: ${{secrets.NIRMATA_URL}}
````

Perform repository scan.
````bash
- name: NCTL Scan Repository
        run: nctl scan repository --policies <path|url to policy folder|repo>

````

`Nctl` works with the Jenkins CI pipeline and can be used in the Jenkins workflow to scan the configuration files present in a repository against the centrally defined policies. The Jenkins job will trigger the scanning of the files and if unsuccessful, the entire job will fail, which will prompt the user with feedback for their changes. Upon successful completion of the job, the scan results will be published to the NPM for viewing. NPM provides insights to platform administrators on overall governance of different code repositories in their organization.

### Understanding the Jenkins Workflow

To see pipeline scanning with Jenkins CI in action:

1. Create a Bitbucket or Git repository with some Docker, Kubernetes, and Terraform files. Refer to the [official GitHub docs](https://docs.github.com/en/repositories/creating-and-managing-repositories/quickstart-for-repositories) for creating a repository.
2. Next, log into Jenkins with your credentials which will take you the dashboard page.
3. After that, click on `New Item` located on the left hand top corner of the screen. It will initiate the creation of a Jenkins job.

4. Enter the name of the job. Select `Freestyle project` from the list of options to choose from and click `OK`.

![image](/images/jenkins-job-initiate.png)

5. Now that the job is created, it needs to be configured accordingly for the pipeline scanning to take place. To do so:<br>
    a. Click on **General** and fill out `Jira site` with your organization's Atlassian URL.<br>
    b. After that, select `Git` as the **Source Code Management** and provide the repository details.<br>
    c. Enter the `URL` of your repository and provide the credentials if its a private one. Leave credentials as none if it is a public repository.<br>
    d. Next, mention the branch name where the scanning will take place. Type `./main` if there is no other branches in the repository.<br>
    e. Next, under **Additional Behaviours**, click `Add` and select `Check out to specific local branch` from the dropdown and type `main` under **Branch Name**.<br>
    f. After this, scroll down to **Build Steps** and click on **Add build step**.<br>
    g. Select `Run Nctl Scan` from the dropdown.<br>
    g. Fill out the `NCTL Binary Link` field with the URL of the latest Nctl binary version to download. The required URL is available [here](https://downloads.nirmata.io/nctl/allreleases/).<br>
    h. Now, check the box for **Scan Only Repository** which will help integrate NPM.<br>
    i. Under `API Key`, add the API key that can be found in the profile section of the NPM tenant.<br>
    j. Fill out `Path to Policies files` with the path to your policy files and `Nirmata URL` with the NPM URL (https://www.nirmata.io)<br>
    k. Finally, click **Save** to complete the job configuration.

![image](/images/job-configure.png)

6. Next, click on **Build Now** on the left hand side navigation bar to run the job. This will trigger the pipeline scan and the process can be seen under `Build History` located on the left hand lower corner of the screen.

![image](/images/job-build.png)

7. Now, click on the build number under **Build History** to see details on the completion of the job. The status page will open by default where the information on the success or the failure of the job will be visible.
8. Click on **Console Output** on the navigation bar to have a look at the scan results.

![image](/images/job-output.png)

9. Scroll down to the end of the page to verify the publishing of the report to the NPM tenant.

## View Scan Reports in NPM

By default, the results of the scan action are published to NPM. This allows administrators to govern their repositories alongside clusters and namespaces.

To have a look at the scan report in NPM:

1. Log into the NPM and go to **Policies**>**Policy Reports**. The Policy Reports can be viewed based on Categories, Clusters, Namespaces, and Repositories.
2. Navigate to the Repositories tab to find the scanned repository under the list of repositories.

![image](/images/npm-repositories.png)

3. Click on the repository hyperlink to get a detailed view of the scanned report.

{{% notice warning %}}
The examples and sample code provided in this workshop are intended to be consumed as instructional content. These will help you understand how various AWS services can be architected to build a solution while demonstrating best practices along the way. These examples are not intended for use in production environments.
{{% /notice %}}

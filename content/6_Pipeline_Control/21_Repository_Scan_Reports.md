---
title: "Repository Scan Reports" 
chapter: false
weight: 41 
---

Repository scan reports provide deep insights on the performance of different repositories containing configuration files. Below is the guidance for publishing Repository Scan Results to Nirmata Contro Hub.

#### Step 1: Login to NCTL

Navigate to [Nirmata - Settings - Profile](https://nirmata.io/webclient/#profile), and copy the nctl login command to authenticate nctl with Nirmata.

:image[Copy nctl login command from Nirmata profile]{src="/static/images/nctl-copy.png" width=900}

Enter this command in the VS Code Server\terminal. Output should look similar to below but with your Nirmata User ID and Token:
```
nctl login --url https://nirmata.io --userid youremail@email.com --token 7nE***
```
#### Step 2: Clone the repository locally

```
git clone https://github.com/nirmata/demo-resources
```

:image[Cloning Nirmata demo resources repository in VS Code]{src="/static/images/nirmata_local_repo_vscode.png" width=900}

The cloned repository contains a `policies/` folder with Kyverno policy YAML files that define validation rules (e.g., requiring resource limits, disallowing privileged containers) and a `resources/` folder with sample Kubernetes manifests, Dockerfiles, and Terraform files to scan against those policies.

#### Step 3: Run the repo scan command

```
cd demo-resources/
nctl scan repository --publish
```

:image[Repository scan results output]{src="/static/images/nirmata_repo_scan.png" width=900}

>NOTE: When implementing it as part of CI\CD pipeline, a DevOps user can publish scan reports to NCH **only** if they belong to a team that has permission to publish scan results. Please contact Admin user for granting permission.

>NOTE: All users belonging to the team will be able to view the repository scan results.

The scan reports are generated after the results from [Github scan action](https://help.nirmata.com/docs/control-hub/workflows/pipelinescanning/github-action/) gets pushed to NCH.

### Viewing Repository Scan Reports in NCH

To view the scan reports:

1. Go to **Policies**>**Policy Reports**. The Policy Reports can be viewed based on Categories, Clusters, Namespaces, and Repositories.
2. Click on the **Repositories** tab to get a list of the available repositories. The information related to the repositories are displayed with the type and number of files present within, the grade obtained, and the status of the repository with the number of *Failed, Warning, Passed, Error, and Skipped*.

:image[Repositories view in Policy Reports]{src="/static/images/repositories_view.png" width=900}

3. Click on any of the repositories to view the list of findings under the **Findings** tab. The findings are displayed with information related to *Impact (File Types and # Files), and Status (%Pass or Fail)*. The page also shows the repository status with the overall grade, % Pass, and Fail.
4. To view the findings on different available branches of the repository, change the branch by clicking on the `All Branches` tab on the top of the screen. Filter the violations according to severity by clicking on the `Severity` tab located beside the search bar. Click on the `Filter File Type` tab to filter the type of file with which the violation is associated.

:image[Repository findings filtered by branch and severity]{src="/static/images/view_findings.png" width=900}

5. Now, click on any of the findings to view the details of the finding. The details contain violation and policy information as the policy name, rule name, severity of the violation, and other metadata. The page also lists the impacted files associated with the violation, their branch name, and status.

:image[Finding detail with impacted files]{src="/static/images/finding_detail.png" width=900}

6. Go back to the previous page and click on the **Files** tab to view the list of impacted files. The files are listed with information detailing the Status of the file with *Failed, Warning, Passed, Error, and Skip* and the number of violations that has impacted the file. The files can be viewed in different branches of the repository and can be filtered according to the type and status of the file.

:image[Impacted files list with status]{src="/static/images/view_file.png" width=900}

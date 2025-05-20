---
title: "Repository Scan Reports" 
chapter: false
weight: 41 
---

Repository scan reports provide deep insights on the performance of different repositories containing configuration files.

### Publishing Repository Scan Results to Nirmata Contro Hub (NCH)

#### Admin User
Login to the NCH tenant with the user's API Key.
```
nctl login --url https://nirmata.io --userid admin@acme.corp --token <admin-api-key>
```

Clone the repository locally.
```
git clone https://github.com/nirmata/demo-resources
```

Run the repo scan command
```
cd demo-resources/
nctl scan repository --publish

```

#### DevOps User
Login to the NCH tenant with the user's API Key.
```
nctl login --url https://nirmata.io --userid user@acme.corp --token <user-api-key>
```

Clone the repository locally.
```
git clone https://github.com/nirmata/demo-resources
```

>NOTE: A DevOps user can publish scan reports to NCH **only** if they belong to a team that has permission to publish scan results. Please contact Admin user for granting permission.

Run the repo scan command
```
cd demo-resources/
nctl scan repository --publish
```

>NOTE: All users belonging to the team will be able to view the repository scan results.

The scan reports are generated after the results from [Github scan action](https://docs.nirmata.io/npmk/workflows/github-action/) gets pushed to NCH.

### Viewing Repository Scan Reports in NCH

To view the scan reports:

1. Go to **Policies**>**Policy Reports**. The Policy Reports can be viewed based on Categories, Clusters, Namespaces, and Repositories.
2. Click on the **Repositories** tab to get a list of the available repositories. The information related to the repositories are displayed with the type and number of files present within, the grade obtained, and the status of the repository with the number of *Failed, Warning, Passed, Error, and Skipped*.

![image](/images/repositories_view.png)

3. Click on any of the repositories to view the list of findings under the **Findings** tab. The findings are displayed with information related to *Impact (File Types and # Files), and Status (%Pass or Fail)*. The page also shows the repository status with the overall grade, % Pass, and Fail.
4. To view the findings on different available branches of the repository, change the branch by clicking on the `All Branches` tab on the top of the screen. Filter the violations according to severity by clicking on the `Severity` tab located beside the search bar. Click on the `Filter File Type` tab to filter the type of file with which the violation is associated.

![image](/images/view_findings.png)

5. Now, click on any of the findings to view the details of the finding. The details contain violation and policy information as the policy name, rule name, severity of the violation, and other metadata. The page also lists the impacted files associated with the violation, their branch name, and status.

![image](/images/finding_detail.png)

6. Go back to the previous page and click on the **Files** tab to view the list of impacted files. The files are listed with information detailing the Status of the file with *Failed, Warning, Passed, Error, and Skip* and the number of violations that has impacted the file. The files can be viewed in different branches of the repository and can be filtered according to the type and status of the file.

![image](/images/view_file.png)

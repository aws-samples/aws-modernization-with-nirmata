---
title: "Shift-Down with nctl" # MODIFY THIS TITLE IF APPLICABLE
chapter: true
weight: 40
---

## Pipeline Scanning Workflow

[nctl scan](https://docs.nirmata.io/docs/nctl/commands/nctl_scan_repository/) can be used within the local CLI environment; however, its true potential is unlocked when utilized within CI pipelines to scan code repositories.

![Scan Action Workflow](/images/scan_action_workflow.png)

The platform or security team admins are responsible for defining the policies that an organization needs to adhere to. They are stored as YAML files in Git repositories or as OCI images in the OCI registry, or also are made available as Helm charts. The DevOps user or the IT team is responsible for managing the configuration files, be it a Kubernetes manifest, or an IaC file, or any JSON spec stored in Git repositories.

User who wants to make any changes to the manifests in these repositories will create a pull request (PR). CI pipelines are configured to trigger on various actions, such as, creating a PR, merging the code to main branch, or even setup to run at regular intervals.

{{% children %}}
---
title: "Pipeline Control Point"
chapter: true
weight: 60
---

## Shifting Security Left with Pipeline Control Point

Now that you've secured both your cloud infrastructure and Kubernetes clusters, it's time to shift security even further left - into your CI/CD pipelines. Pipeline Control Point allows you to catch policy violations before code is even committed to your repositories.

### The Value of Pipeline Scanning
By integrating policy checks into your CI/CD pipelines, you can:

1. **Catch issues early**: Identify policy violations during development, not production
2. **Accelerate feedback loops**: Give developers immediate feedback on policy compliance
3. **Reduce remediation costs**: Fix issues when they're cheapest to address
4. **Maintain deployment velocity**: Prevent non-compliant changes from slowing down releases

### What You'll Implement
In this section, you'll:
- Set up GitHub CLI and fork the example repository
- Configure repository scanning with `nctl scan`
- Integrate policy checks into GitHub Actions workflows
- Configure Jenkins pipeline integration
- View and analyze repository scan reports in Nirmata Control Hub

## Pipeline Scanning Workflow

[nctl scan](https://docs.nirmata.io/docs/nctl/commands/nctl_scan_repository/) can be used within the local CLI environment; however, its true potential is unlocked when utilized within CI pipelines to scan code repositories.

![Scan Action Workflow](/images/scan_action_workflow.png)

The platform or security team admins are responsible for defining the policies that an organization needs to adhere to. They are stored as YAML files in Git repositories or as OCI images in the OCI registry, or also are made available as Helm charts. The DevOps user or the IT team is responsible for managing the configuration files, be it a Kubernetes manifest, or an IaC file, or any JSON spec stored in Git repositories.

User who wants to make any changes to the manifests in these repositories will create a pull request (PR). CI pipelines are configured to trigger on various actions, such as, creating a PR, merging the code to main branch, or even setup to run at regular intervals.

{{% children %}}

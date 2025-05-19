---
title: "Cluster Control Point"
chapter: true
weight: 50
---

## Extending Governance to Kubernetes Clusters

With Cloud Control Point protecting your cloud infrastructure, it's time to implement governance at the Kubernetes cluster level. Cluster Control Point leverages Kyverno to enforce policies directly within your Kubernetes clusters.

### The Power of In-Cluster Policy Enforcement
Cluster Control Point provides:

1. **Runtime Policy Enforcement**: Block non-compliant resources from being created or modified
2. **Background Scanning**: Identify existing resources that violate policies
3. **Automated Remediation**: Fix non-compliant resources automatically
4. **Centralized Reporting**: View policy violations across all clusters in one place

### What You'll Implement
In this section, you'll:
- Register your Kubernetes cluster with Nirmata Control Hub
- Deploy the Nirmata Controller to enable policy enforcement
- Apply policy sets for security and compliance
- View and analyze policy violation reports

By the end of this section, you'll have a fully governed Kubernetes cluster with policies enforcing security best practices and compliance requirements.

Let's begin by registering your cluster with Nirmata Control Hub.

![Register an Existing Kubernetes Cluster](/images/register.jpg)

---
title: "Cloud Control Point"
chapter: true
weight: 40
---

## From Policy to Practice: Implementing Cloud Control Point

Now that you understand the core capabilities of Nirmata Control Hub, let's implement our first control point - Cloud Control Point. This component extends Nirmata's policy enforcement beyond Kubernetes clusters to your cloud infrastructure.

### Why Cloud Control Matters
While Kubernetes admission controllers like Kyverno can secure your clusters, they can't prevent misconfigurations in the underlying cloud infrastructure. Cloud Control Point fills this gap by:

1. **Preventing misconfigurations before deployment**: Stop non-compliant cloud resources from being created
2. **Providing consistent governance across clouds**: Apply the same policies regardless of cloud provider
3. **Maintaining continuous compliance**: Scan existing resources to identify drift from policy

In the following sections, you'll set up Cloud Control Point for AWS and see how it prevents common misconfigurations that could lead to security vulnerabilities or compliance violations.

### What You'll Implement
- Pod identity configuration for secure AWS access
- Cloud Control Point deployment via Helm
- Cloud admission controller setup
- Example policy enforcement for ECS clusters
- Integration with Nirmata Control Hub for centralized reporting

Let's start by configuring pod identity for secure AWS access.

## Key Features
* Cloud Admission Control: Cloud Control Point introduces admission control for cloud environments, allowing you to prevent misconfigurations before they impact production. It enforces policies at the moment resources are created or modified, ensuring compliance from the start.
* Comprehensive Multi-Cloud Compatibility: Designed to work with any cloud provider and service, Cloud Control Point offers flexibility for diverse environments. Its policies can be applied universally, giving organizations consistent security and governance across all cloud platforms.
* Continuous Background Scanning: Beyond initial admission control, Cloud Control Point performs ongoing scans of cloud resources, identifying and alerting teams to misconfigurations and potential vulnerabilities as environments evolve. This continuous monitoring enhances long-term compliance and security.
* Event-Driven Reporting: Cloud Control Point generates detailed reports based on events, similar to Kyverno's report formats, and integrates with the working group policy API. These reports provide insights into policy compliance, security posture, and operational effectiveness.
* Integration with Nirmata Control Hub: As part of the Nirmata Control Hub, Cloud Control Point enables centralized visibility into pipeline, cluster, and cloud security. By consolidating governance data in one platform, it empowers teams to proactively manage their security and compliance postures across all stages of the deployment lifecycle.

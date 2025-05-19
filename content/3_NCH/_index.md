---
title: "Getting started with Nirmata"
chapter: true
weight: 10
---
## Nirmata Control Hub: Your Unified Kubernetes Governance Platform

Nirmata Control Hub (NCH) provides a comprehensive solution for securing and governing Kubernetes environments at scale. As the creator of [Kyverno](https://kyverno.io/), Nirmata brings enterprise-grade policy management to Kubernetes.

### The Governance Challenge
Organizations adopting Kubernetes face significant challenges in maintaining security, compliance, and operational best practices across multiple clusters and teams. Without proper governance:
- Security vulnerabilities can proliferate through misconfigured workloads
- Compliance requirements become difficult to enforce consistently
- Operational standards vary across teams and environments
- Visibility into policy violations remains fragmented

### The Nirmata Solution
NCH addresses these challenges through a unified approach to policy management across three critical control points:

![npm dashboard](/images/npm-dashboard-new.png)

1. **Cloud Control Point**: Prevents misconfigurations in cloud resources before deployment
2. **Cluster Control Point**: Enforces policies across Kubernetes clusters in runtime
3. **Pipeline Control Point**: Shifts security left by validating configurations in CI/CD pipelines

In this workshop, you'll implement each of these control points and learn how they work together to create a comprehensive governance framework for your Kubernetes environments.

### Key Benefits
- **Operational Compliance**: Apply curated policy sets for security, multitenancy, and best practices
- **Automated Policy Management**: Centrally manage policies across clusters and namespaces
- **Comprehensive Reporting**: Gain visibility into policy violations across your environment
- **Collaborative Workflows**: Streamline exception handling and remediation processes

Let's begin by exploring the Cloud Control Point, which helps prevent misconfigurations in your cloud resources.

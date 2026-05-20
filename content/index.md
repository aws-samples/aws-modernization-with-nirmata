---
title: "Security, Governance and Compliance with Nirmata on AWS"
chapter: true
weight: 1
---

:image[Nirmata Logo]{src="/static/images/nirmata-logo-horizontal-small.png"}

## Workshop Overview

This workshop demonstrates how to implement AWS and Kubernetes governance and compliance controls using **Nirmata Control Hub** on AWS. You'll deploy policy-based controls on an **Amazon EKS** cluster (with EKS Auto Mode enabled) to govern AWS cloud infrastructure, Kubernetes workloads, and CI/CD pipelines.

**AWS Services Used:**
- **Amazon EKS** (Elastic Kubernetes Service) with Auto Mode — managed Kubernetes cluster
- **AWS CloudFormation** — infrastructure provisioning
- **Amazon EC2** — VS Code development environment (for guided events)
- **AWS IAM** — identity and access management for EKS Pod Identity

### The Problem

Organizations running Kubernetes on AWS face a governance gap. As clusters multiply across regions and teams, enforcing consistent security policies, compliance standards, and operational best practices becomes increasingly difficult. Misconfigurations in cloud infrastructure, Kubernetes clusters, and CI/CD pipelines create security vulnerabilities and compliance violations that are costly to remediate after deployment.

Without centralized policy enforcement, teams rely on manual reviews, tribal knowledge, and fragmented tooling — leading to inconsistent configurations, security drift, and audit failures. The challenge intensifies when governance must span cloud resources (like EKS clusters and IAM roles), in-cluster workloads, and the pipelines that deploy them.

### Solution Overview

Nirmata Control Hub provides a unified governance platform that enforces policies across three critical control points in your AWS environment:

1. **Cloud Control Point** — Prevents misconfigurations in AWS cloud resources before they are provisioned, using admission control for cloud APIs.

2. **Cluster Control Point** — Enforces security and compliance policies directly within Kubernetes clusters using Kyverno, blocking non-compliant workloads at admission time and continuously scanning for drift.

3. **Pipeline Control Point** — Shifts security left by integrating policy checks into CI/CD pipelines, catching violations before code reaches production.

This workshop deploys these controls on an **Amazon EKS cluster with EKS Auto Mode** enabled, demonstrating how Nirmata integrates natively with AWS managed Kubernetes services.

<!-- TODO: Add architecture diagram at static/images/nirmata-architecture-diagram.png -->

### What You'll Learn
- How to set up and configure Nirmata Control Hub for Kubernetes governance
- Implementing Cloud Control Point to prevent cloud resource misconfigurations
- Deploying Cluster Control Point to enforce policies across Kubernetes clusters
- Integrating Pipeline Control Point into your CI/CD workflows for shift-left security
- Managing policy exceptions and remediations through collaborative workflows

### Workshop Journey
This workshop follows a logical progression that mirrors a real-world implementation:
1. **Setup & Prerequisites**: Prepare your environment and Nirmata account
2. **Nirmata Control Hub**: Understand the core platform capabilities
3. **Cloud Control Point**: Prevent misconfigurations in cloud resources
4. **Cluster Control Point**: Enforce policies across Kubernetes clusters
5. **Pipeline Control Point**: Shift security left into your CI/CD pipelines
6. **Nirmata Workflows**: Manage exceptions and remediations collaboratively

### Key Benefits

* **Policy-based Intelligent Guardrails**: Rapidly deploy Intelligent Guardrails by implementing curated best practices, and community crowd-sourced or custom-developed policies
* **Policy-as-Code**: Easily manage policies through their entire lifecycle, ensuring consistent deployment and governance
* **Policy Reporting and Alerts**: Gain insights on the effectiveness of policy through reporting and contextual correlation and insight engine
* **Continuous Compliance**: Protect the software supply chain with continuous compliance through policies as a standard part of the DevOps pipeline
* **Collaboration**: Leverage existing processes and workflows by integrating with tools like Git, Slack, Jira, and others

### Target Audience
- DevOps Engineers
- Site Reliability Engineers (SREs)
- Platform Engineers
- Cloud Architects and Infrastructure Engineers
- Kubernetes Administrators
- Software Developers working with containerized applications

### Required Knowledge Level
- Basic understanding of AWS services (EKS, IAM, CloudFormation)
- Familiarity with Kubernetes concepts
- Working knowledge of container technologies
- Basic command-line experience

### Expected Costs

If you are running this workshop at an AWS-organized event, the lab environment will be provided to you at no cost.

If you are running this workshop in your own AWS account, the estimated cost is approximately **$5–$10 USD** for a 2-3 hour session, primarily from:
- Amazon EKS cluster (control plane + Auto Mode managed nodes)
- EC2 instance for VS Code (if used)

Follow the [Clean Up Resources](/8_cleanup.html) section immediately after completing the workshop to avoid ongoing charges.

::alert[The examples and sample code provided in this workshop are intended to be consumed as instructional content. These will help you understand how various AWS services can be architected to build a solution while demonstrating best practices along the way. These examples are not intended for use in production environments.]{type="warning"}

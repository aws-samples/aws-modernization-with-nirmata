---
title: "Cloud Control Point"
chapter: true
weight: 10
---

Cloud Control Point is an admission controller designed for cloud environments. Inspired by Kubernetes admission controllers like Kyverno, Cloud Control Point fills a critical gap in cloud-native operations by enforcing policy-as-code standards directly in cloud resource configurations. This capability enables organizations to prevent misconfigurations from reaching production environments, ensuring that resources adhere to defined policies for security and compliance.

As a core component of the Nirmata Control Hub, Cloud Control Point provides a unified solution for managing security and governance across pipelines, clusters, and cloud environments. With admission control, continuous background scanning, and event-driven reporting, Cloud Control Point helps teams maintain a consistent and secure posture across their entire cloud infrastructure.

## Key Features
* Cloud Admission Control: Cloud Control Point introduces admission control for cloud environments, allowing you to prevent misconfigurations before they impact production. It enforces policies at the moment resources are created or modified, ensuring compliance from the start.
* Comprehensive Multi-Cloud Compatibility: Designed to work with any cloud provider and service, Cloud Control Point offers flexibility for diverse environments. Its policies can be applied universally, giving organizations consistent security and governance across all cloud platforms.
* Continuous Background Scanning: Beyond initial admission control, Cloud Control Point performs ongoing scans of cloud resources, identifying and alerting teams to misconfigurations and potential vulnerabilities as environments evolve. This continuous monitoring enhances long-term compliance and security.
* Event-Driven Reporting: Cloud Control Point generates detailed reports based on events, similar to Kyverno’s report formats, and integrates with the working group policy API. These reports provide insights into policy compliance, security posture, and operational effectiveness.
* Integration with Nirmata Control Hub: As part of the Nirmata Control Hub, Cloud Control Point enables centralized visibility into pipeline, cluster, and cloud security. By consolidating governance data in one platform, it empowers teams to proactively manage their security and compliance postures across all stages of the deployment lifecycle.



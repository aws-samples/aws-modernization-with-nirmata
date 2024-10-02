---
title: "Exception Management" 
chapter: false
weight: 43 
---

Policy exceptions are temporary deviations that are required when following the policy practices might not be possible because it can hinder operational needs.

### Policy exception workflow

Every policy exception request is sent to an admin for review. The admin can either accept or reject the request. If the request gets accepted, the PolicyException resource gets deployed and the user who requested the exception gets notified via email.

### Requesting a policy exception

To request a **policy exception**:

1. Go to **Policy** -> **PolicyReports**. Click on the namespaces to view the violations in that namespace.
2. Click on the violation that needs an exception. This will display a detailed report of that particular violation.
3. On the same page, click on the `Request Policy Exception` button located on the top right-hand section of the screen. It will display a dialog box as shown below.

![image](/images/request_policy_exception_2.png)

<!-- <img src="../../images/request_policy_exception_2.png" height="150" width="150" /> -->

4. Fill the details along with a reason for requesting the exception, and click the `Create` button.
5. A new **Policy Exception Request** is now created and it will send an alert to the admin for a review.

### Viewing Policy Exception Requests

To view the policy exception requests:

1. Go to **Policies** -> **Policy Exceptions** and click on Policy Exception Requests. This page displays all the pending policy exception requests.
2. By default, only pending requests are displayed. To view all approved and rejected requests, change the `Status` filter accordingly.

{{% notice warning %}}
The examples and sample code provided in this workshop are intended to be consumed as instructional content. These will help you understand how various AWS services can be architected to build a solution while demonstrating best practices along the way. These examples are not intended for use in production environments.
{{% /notice %}}

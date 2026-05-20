---
title: "Create AWS account"
chapter: false
weight: 31
---

Please follow below instructions if you don't have and want to create your own AWS account

::alert[Your account must have the ability to create new IAM roles and scope other IAM permissions.]{type="warning"}

1. If you don't already have an AWS account with Administrator access: [create
one now by clicking here](https://aws.amazon.com/getting-started/)

1. Once you have an AWS account, ensure you are following the remaining workshop steps
as an IAM user with administrator access to the AWS account:
[Create a new IAM user to use for the workshop](https://console.aws.amazon.com/iam/home?#/users$new)

1. Enter the user details:
:image[IAM - Create new user]{src="/static/images/iam-1-create-user.png" width=900}

1. Attach the AdministratorAccess IAM Policy:
:image[IAM - Attach AdministratorAccess policy]{src="/static/images/iam-2-attach-policy.png" width=900}

1. Click to create the new user:
:image[IAM - Confirm user creation]{src="/static/images/iam-3-create-user.png" width=900}

1. Take note of the login URL and save:
:image[IAM - Save login URL]{src="/static/images/iam-4-save-url.png" width=900}

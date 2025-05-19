---
title: "Example: Creating an ECS Cluster"
chapter: false
weight: 34
---

The following examples demonstrate how the admission controller enforces a policy requiring all ECS task definitions to run in a non-privileged mode.

1. Create an ECS task definition in privieleged mode. In the container-definitions, privileged field is set to true.:

    ```bash
    aws ecs register-task-definition \
        --family privileged-task \
        --requires-compatibilities EC2 \
        --network-mode bridge \
        --cpu "256" \
        --memory "512" \
        --container-definitions '[
            {
            "name": "privileged-container",
            "image": "amazonlinux",
            "essential": true,
            "memory": 512,
            "cpu": 256,
            "privileged": true,
            "entryPoint": ["sh", "-c"],
            "command": ["sleep 3600"]
            }
        ]'
    ```

    The output should be similar to the following:

    ```
    An error occurred (406) when calling the RegisterTaskDefinition operation: validate-ecs-containers-nonprivileged.validate-ecs-containers-nonprivileged TEST: -> The `privileged` field, if present, should be set to `false`
    ```

    As expected, the request was blocked since it violates the ValidatingPolicy that requires all ECS task definitions to run in non-privielged mode, i.e., if privileged field is present, it should be set to false.

2. Create an ECS task definition with privileged field set to false:

    ```bash
    aws ecs register-task-definition \
        --family privileged-task \
        --requires-compatibilities EC2 \
        --network-mode bridge \
        --cpu "256" \
        --memory "512" \
        --container-definitions '[
            {
            "name": "privileged-container",
            "image": "amazonlinux",
            "essential": true,
            "memory": 512,
            "cpu": 256,
            "privileged": false,
            "entryPoint": ["sh", "-c"],
            "command": ["sleep 3600"]
            }
        ]'
    ```

    The task definition should be successfully created.

    The request was successful since it complies with the ValidatingPolicy.

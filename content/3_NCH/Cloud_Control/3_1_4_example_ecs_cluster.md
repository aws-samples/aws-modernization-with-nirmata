---
title: "Example: Creating an ECS Cluster"
chapter: false
weight: 34
---

The following examples demonstrate how the admission controller enforces a policy requiring all ECS clusters to have a ***group*** tag.

1. Create an ECS cluster without the ***group*** tag:

    ```bash
    aws ecs create-cluster --cluster-name bad-cluster
    ```

    The output should be similar to the following:

    ```
    An error occurred (406) when calling the CreateCluster operation: ecs-cluster.check-tags bad-cluster: -> A 'group' tag is required
    -> all[0].check.data.(tags[?key=='group'] || `[]`).(length(@) > `0`): Invalid value: false: Expected value: true
    ```

    As expected, the request was blocked since it violates the ValidatingPolicy that requires all ECS clusters to have the ***group*** tag.

2. Create an ECS cluster with the ***group*** tag:

    ```bash
    aws ecs create-cluster --cluster-name good-cluster --tags key=group,value=test key=owner,value=test
    ```

    The output should be similar to the following:

    ```
    {
        "cluster": {
            "clusterArn": "arn:aws:ecs:us-east-1:844333597536:cluster/good-cluster",
            "clusterName": "good-cluster",
            "status": "ACTIVE",
            "registeredContainerInstancesCount": 0,
            "runningTasksCount": 0,
            "pendingTasksCount": 0,
            "activeServicesCount": 0,
            "statistics": [],
            "tags": [
                {
                    "key": "owner",
                    "value": "test"
                },
                {
                    "key": "group",
                    "value": "test"
                }
            ],
            "settings": [
                {
                    "name": "containerInsights",
                    "value": "disabled"
                }
            ],
            "capacityProviders": [],
            "defaultCapacityProviderStrategy": []
        }
    }
    ```

    The request was successful since it complies with the ValidatingPolicy that requires all ECS clusters to have the ***group*** tag.

### ECS Clusters and Task Definitions

To test the scanner, we will create ECS resources, both compliant and non-compliant with the ValidatingPolicies that check the ***group*** tag, by creating ECS clusters and task definitions, some with and some without the required ***group*** tag.

1. Create an ECS cluster named ***bad-cluster*** without the ***group*** tag:
```bash
aws ecs create-cluster --cluster-name bad-cluster
```

2. Register a task definition named ***bad-task*** without the ***group*** tag:
```bash
aws ecs register-task-definition \
    --family bad-task \
    --container-definitions '[{"name": "my-app", "image": "nginx:latest", "essential": true, "portMappings": [{"containerPort": 80, "hostPort": 80}]}]' \
    --requires-compatibilities FARGATE \
    --cpu 256 \
    --memory 512 \
    --network-mode awsvpc
```

3. Create an ECS cluster named ***good-cluster*** with the ***group*** tag:
```bash
aws ecs create-cluster --cluster-name good-cluster --tags key=group,value=development
```

4. Register a task definition named ***good-task*** with the ***group*** tag:
```bash
aws ecs register-task-definition \
    --family good-task \
    --container-definitions '[{"name": "my-app", "image": "nginx:latest", "essential": true, "portMappings": [{"containerPort": 80, "hostPort": 80}]}]' \
    --requires-compatibilities FARGATE \
    --cpu 256 \
    --memory 512 \
    --network-mode awsvpc \
    --tags '[{"key": "group", "value": "production"}]'
```


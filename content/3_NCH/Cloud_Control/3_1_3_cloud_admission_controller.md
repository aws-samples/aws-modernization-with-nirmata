---
title: "Cloud Admission Controller"
chapter: false
weight: 33
---

This section provides a step-by-step guide on how to use the admission controller to intercept AWS requests and apply policies to them.

### Setting up a Proxy

To intercept AWS requests, we need a proxy server that listens on a specific port. The proxy server will apply the policies to the requests and forward them to the AWS cloud if they are compliant.

As part of the helm installation, this proxy server is automatically created for us and runs within the EKS Cluster, listening on port 8443. To use this proxy from your local machine, you need to establish a connection between your local port 8443 and the proxy server's port 8443 within the cluster. This is achieved using port forwarding.

```bash
kubectl port-forward svc/cloud-controller-admission-controller-svc 8443:8443 -n nirmata
```

By running this command, any traffic sent to ***localhost:8443*** on your machine will be forwarded to the proxy server in the cluster. 
This allows you to interact with the proxy and, consequently, enforce your policies on AWS requests as if the proxy server was running locally.

### Using the AWS CLI

You need to configure your AWS CLI to route requests through the proxy server. Perform the following steps in a new terminal.

This involves setting two environment variables:

1. **AWS_CA_BUNDLE**: The controller uses a self-signed security certificate. This variable tells the AWS CLI to trust that certificate. 
   
    First, download the certificate:
    
    ```bash
    kubectl get secrets -n nirmata cloud-control-admission-controller-svc.nirmata.svc.tls-ca -o jsonpath="{.data.tls\.crt}" | base64 --decode > ca.crt
    ```
    
    Then, set the environment variable:

    ```bash
    export AWS_CA_BUNDLE=ca.crt
    ```

    It tells the AWS CLI which Certificate Authority (CA) to trust for verifying the proxy's SSL certificate. 
    Because the cloud admission controller is using a self-signed certificate (not issued by a publicly trusted CA), 
    the AWS CLI won't trust it by default. By setting **AWS_CA_BUNDLE** to the path of the controller's CA certificate (ca.crt), 
    you're explicitly telling the AWS CLI that this certificate is valid and should be used to establish a secure connection with the proxy.
    Without this, the AWS CLI would reject the connection due to the untrusted certificate.

1. **HTTPS_PROXY**: This tells the AWS CLI to send all requests through the controller acting as a local proxy.

    ```bash
    export HTTPS_PROXY=http://localhost:8443
    ```

Once configured, your AWS CLI commands will be checked against the defined policies before being sent to AWS.
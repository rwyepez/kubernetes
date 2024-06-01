
# Installing and Verifying Metrics Server in Kubernetes

This document provides the steps to install and verify the Metrics Server in your Kubernetes cluster.

## Step 1: Install Metrics Server

To install the Metrics Server, apply the following YAML manifest provided by the project:

```sh
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

## Step 2: Verify the Installation

After applying the manifest, verify that the Metrics Server is running with the following command:

```sh
kubectl get pods -n kube-system
```

You should see a pod with a name similar to `metrics-server-xxxxx` in the `kube-system` namespace. Ensure that the pod is in the `Running` state.

### Example of Expected Output

```sh
NAME                                  READY   STATUS    RESTARTS   AGE
metrics-server-58d9f9876-abcde        1/1     Running   0          1m
```

### Example commands
| Command                                      | Description                                                       |
|----------------------------------------------|-------------------------------------------------------------------|
| `kubectl top pod pod_name`                   | Shows CPU and memory metrics for a pod.                           |
| `kubectl top node node_name`                 | Shows CPU and memory metrics for a node.                          |

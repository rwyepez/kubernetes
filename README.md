# kubernetes

## Commands

### General
| Command                                      | Description                                                       |
|----------------------------------------------|-------------------------------------------------------------------|
| `kubectl --help`                             | List all kubectl commands.                                        |
| `kubectl config get-contexts`                | Obtiene los contextos cargados en /Users/YourUser/.kube/config.   |
| `kubectl get ns`                             | Lists all namespaces in the cluster.                              |
| `kubectl get all`                            | Lists all resources.                                              |
| `-o wide`                                    | Include at the end of the command to see more info                |
| `-o yaml`                                    | Include at the end of the command to see yaml configuration       |
| `-n namespace_name`                          | Include to search resources by namespace.                         |

### Pods
| Command                                      | Description                                                       |
|----------------------------------------------|-------------------------------------------------------------------|
| `kubectl get pods`                           | Lists all pods in the current namespace.                          |
| `kubectl get pods -A`                        | Lists all pods including namespace information.                   |
| `kubectl get pod pod_name --show-labels`     | Lists a pod including label information.                          |
| `kubectl get pod pod_name -o yaml`           | Retrieves detailed information about a pod in YAML format.        |
| `kubectl describe pod pod_name`              | Displays detailed information about a specific pod.               |
| `kubectl logs pod_name`                      | Retrieves logs from a specific pod.                               |
| `kubectl logs pod_name --tail=20`            | Retrieves the last 20 lines of logs from a specific pod.          |
| `kubectl logs pod_name --since=1h`           | Retrieves logs from a specific pod from the last hour.            |
| `kubectl logs pod_name -f`                   | Follows the log output from a specific pod in real time.          |
| `kubectl exec -it pod_name -- sh`            | Opens an interactive shell session inside a running pod.          |
| `kubectl exec -it pod_name -- bash`          | Opens an interactive bash session inside a running pod.           |
| `kubectl top pod pod_name`                   | Show CPU and memory metrics. It's necessary install API metrics   |
| `kubectl delete pod pod_name`                | Deletes a specific pod.                                           |
| `kubectl delete pods --field-selector status.phase=Failed` | Deletes pods that are in a failed status.           |

### Deployments
| Command                                      | Description                                                       |
|----------------------------------------------|-------------------------------------------------------------------|
| `kubectl apply -f file.yaml`                 | Applies the configurations defined in the YAML file.              |
| `kubectl delete -f file.yaml`                | Deletes the resources defined in the YAML file.                   |
| `kubectl get deployments`                    | Lists all deployments in the current namespace.                   |
| `kubectl get sts`                            | Lists all stateful sets in the current namespace.                 |
| `kubectl delete deployment deployment_name`  | Deletes a specific deployment.                                    |


### Services
| Command                                      | Description                                                       |
|----------------------------------------------|-------------------------------------------------------------------|
| `kubectl get svc`                            | Lists all services in the current namespace.                      |
| `kubectl describe svc service_name`          | Displays detailed information about a specific service.           |


### Nodes
| Command                                      | Description                                                       |
|----------------------------------------------|-------------------------------------------------------------------|
| `kubectl get nodes`                          | Lists all nodes in the cluster.                                   |
| `kubectl describe node node_name`            | Displays detailed information about a specific node.              |
| `kubectl get node node_name -o json`         | Retrieves detailed information about a node in JSON format.       |
| `kubectl delete node node_name`              | Deletes a specific node.                                          |
| `kubectl drain node_name`                    | Evicts all pods from the node.                                    |
| `kubectl cordon node_name`                   | Marks a node as unschedulable to prevent new pods from being assigned to it. |
| `kubectl uncordon node_name`                 | Marks a node as schedulable to allow new pods to be assigned to it. |
| `kubectl top node node_name`                 | Shows CPU and memory metrics for a node.                          |



## Resource and Probe Configuration in Kubernetes

This section details how to configure resources and readiness and liveness probes for a container in a Kubernetes manifest. Example file 02-pod.yaml

### Resources

```yaml
resources:
  requests:
    memory: "64Mi"
    cpu: "200m"
  limits:
    memory: "128Mi"
    cpu: "500m"
```

- **requests**: Specifies the minimum amount of resources guaranteed for the container. In this case:
  - `64Mi` of memory.
  - `200m` (millicores) of CPU.
- **limits**: Defines the maximum amount of resources the container can use. In this case:
  - `128Mi` of memory.
  - `500m` (millicores) of CPU.

#### Note

- **1000m (millicores)** is equivalent to **1 core**.

### Readiness Probe

```yaml
readinessProbe:
  httpGet:
    path: /
    port: 80
  initialDelaySeconds: 5
  periodSeconds: 10
```

- **httpGet**: Performs an HTTP GET request to the specified path (`/`) on port `80` to check if the container is ready to receive traffic.
- **initialDelaySeconds**: Waits `5` seconds before performing the first check.
- **periodSeconds**: Performs the check every `10` seconds.

### Liveness Probe

```yaml
livenessProbe:
  tcpSocket:
    port: 80
  initialDelaySeconds: 15
  periodSeconds: 20
```

- **tcpSocket**: Checks that the container is alive by attempting to connect to port `80` via TCP.
- **initialDelaySeconds**: Waits `15` seconds before performing the first check.
- **periodSeconds**: Performs the check every `20` seconds.

These configurations ensure that the container has the necessary resources to function correctly and that Kubernetes can continuously monitor its health and availability.



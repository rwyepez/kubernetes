# kubernetes

## Commands

### General
| Command                                      | Description                                                       |
|----------------------------------------------|-------------------------------------------------------------------|
| `kubectl --help`                             | List all kubectl commands.                                        |
| `kubectl config get-contexts`                | Obtiene los contextos cargados en /Users/YourUser/.kube/config.   |
| `kubectl get ns`                             | Lists all namespaces in the cluster.                              |
| `kubectl create ns namespace_name`           | Create a new namespace.                                           |
| `kubectl get all`                            | Lists all resources.                                              |
| `-o wide`                                    | Include at the end of the command to see more info                |
| `-o yaml`                                    | Include at the end of the command to see yaml configuration       |
| `-A yaml`                                    | Include at the end of the command to see all resources with namespace info           |
| `-n namespace_name`                          | Include to search resources by namespace.                         |

### Pods
| Command                                      | Description                                                       |
|----------------------------------------------|-------------------------------------------------------------------|
| `kubectl get pods`                           | Lists all pods in the current namespace.                          |
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
| Command                                                      | Description                                                       |
|--------------------------------------------------------------|-------------------------------------------------------------------|
| `kubectl apply -f file.yaml`                                 | Applies the configurations defined in the YAML file.              |
| `kubectl delete -f file.yaml`                                | Deletes the resources defined in the YAML file.                   |
| `kubectl get deployments`                                    | Lists all deployments in the current namespace.                   |
| `kubectl get sts`                                            | Lists all stateful sets in the current namespace.                 |
| `kubectl delete deployment deployment_name`                  | Deletes a specific deployment.                                    |
| `kubectl rollout restart deployment deployment_name`         | Restarts a specific deployment.                                   |
| `kubectl edit deployment deployment_name`                    | Live or Hot editing a specific deployment.                        |
| `kubectl rollout history deployment deployment_name`         | Shows the rollout history of a specific deployment.               |
| `kubectl set image deployment deployment_name container_name=new_image` | Updates the image of a specific container in a deployment. |
| `kubectl set image deployment deployment_name container_name=new_image --record=true` | Updates the image and records the change for a specific container in a deployment. |
| `kubectl rollout undo deployment deployment_name`            | Reverts a deployment to a previous revision.                      |
| `kubectl rollout undo deployment deployment_name --to-revision=revision_id` | Reverts a deployment to a specific revision, see revisions in history command.  |

### Services
| Command                                      | Description                                                       |
|----------------------------------------------|-------------------------------------------------------------------|
| `kubectl get svc`                            | Lists all services in the current namespace.                      |
| `kubectl describe svc service_name`          | Show detailed information about a specific service, include IP addresses of pods associated.           |


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


## Resource Kinds

1. **Pod**
    - **Description:** The basic unit of execution in Kubernetes, encapsulating one or more containers, storage, network, and configuration options.
    - **Use Case:** Suitable for running a single instance of an application or microservice.

2. **Deployment**
    - **Description:** Manages a set of identical pods, ensuring that the specified number of pods are running at any time.
    - **Use Case:** Ideal for stateless applications, where the state is not saved locally on the pods but rather in external storage.
    - [Example](./04-deployment.yaml)

3. **DaemonSet**
    - **Description:** Ensures that a copy of a pod is running on all (or specific) nodes in the cluster.
    - **Use Case:** Useful for background tasks such as log collection, monitoring, or other node-specific tasks.
    - [Example](./03-daemonset.yaml)

4. **StatefulSet**
    - **Description:** Manages stateful applications, providing unique network identities and stable, persistent storage for each pod.
    - **Use Case:** Suitable for applications that require stable identities and persistent storage, such as databases.

5. **Job**
    - **Description:** Creates one or more pods and ensures that a specified number of them successfully terminate.
    - **Use Case:** Suitable for batch processing tasks, such as data processing jobs.

6. **CronJob**
    - **Description:** Manages jobs that are run on a schedule, similar to cron jobs in Unix-like systems.
    - **Use Case:** Ideal for periodic tasks like backups, report generation, or other scheduled tasks.

7. **ReplicaSet**
    - **Description:** Ensures a specified number of pod replicas are running at any given time.
    - **Use Case:** Often used by Deployments as a means to maintain a stable set of replica pods.

8. **ConfigMap**
    - **Description:** Provides a way to inject configuration data into pods.
    - **Use Case:** Useful for decoupling environment-specific configurations from the application code.

9. **Secret**
    - **Description:** Similar to ConfigMaps but designed to store sensitive information, such as passwords, tokens, and keys.
    - **Use Case:** Ensures sensitive information is stored securely and injected into pods as needed.

10. **PersistentVolume (PV)**
    - **Description:** Represents a piece of storage in the cluster that has been provisioned by an administrator or dynamically provisioned.
    - **Use Case:** Provides storage resources for applications that need persistent storage.

11. **PersistentVolumeClaim (PVC)**
    - **Description:** A request for storage by a user, specifying size and access modes.
    - **Use Case:** Used to bind to a PersistentVolume and mount it to a pod.

## Types of Kubernetes Services

Kubernetes provides several types of services to manage network access to a set of Pods.

### 1. ClusterIP

**ClusterIP** is the default type of service in Kubernetes. It provides a stable internal IP address that allows communication between services **within** the cluster.

- **Use Case**: Internal communication within the cluster.

### Example
  - Apply both manifests
  - [Example clusterIp](./06-clusterIP.yaml) [Example bastion to test connectivity](./05-bastion.yaml)
  - Opens interactive bash session in bastion pod
    - `kubectl exec -it ubuntu -- bash`
  - Install ping and curl in bastion pod
    - `apt-get update -y`
    - `apt-get install -y iputils-ping`
    - `apt install curl`
  - Test clusterIp service
    - Execute
      - `ping hello-svc`
      - `curl http://hello-svc:8080`

### 2. NodePort

**NodePort** exposes the service on each node’s IP at a static port. A ClusterIP service, to which the NodePort service routes, is automatically created.

- **Use Case**: Exposing a service to external traffic.

### 3. LoadBalancer

**LoadBalancer** creates an external load balancer (if supported by the cloud provider) and assigns a fixed external IP to the service.

- **Use Case**: Exposing a service to the internet.


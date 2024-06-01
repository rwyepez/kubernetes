# kubernetes

| Command                                      | Description                                                       |
|----------------------------------------------|-------------------------------------------------------------------|
| `kubectl --help`                             | List all kubectl commands                                         |
| `kubectl config get-contexts`                | Obtiene los contextos cargados en /Users/YourUser/.kube/config    |


| Command                                      | Description                                                       |
|----------------------------------------------|-------------------------------------------------------------------|
| `kubectl get ns`                             | Lists all namespaces in the cluster.                              |
| `kubectl get pods`                           | Lists all pods in the current namespace.                          |


| Command                                      | Description                                                       |
|----------------------------------------------|-------------------------------------------------------------------|
| `kubectl get pod pod_name -o yaml`           | Retrieves detailed information about a pod in YAML format.        |
| `kubectl describe pod pod_name`              | Displays detailed information about a specific pod.               |
| `kubectl delete pod pod_name`                | Deletes a specific pod.                                           |
| `kubectl exec -it pod_name -- sh`            | Opens an interactive shell session inside a running pod.          |


| Command                                      | Description                                                       |
|----------------------------------------------|-------------------------------------------------------------------|
| `kubectl apply -f file.yaml`                 | Applies the configurations defined in the YAML file.              |
| `kubectl delete -f file.yaml`                | Deletes the resources defined in the YAML file.                   |



### Resource and Probe Configuration in Kubernetes

This section details how to configure resources and readiness and liveness probes for a container in a Kubernetes manifest. Example file 02-pod.yaml

#### Resources

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

#### Readiness Probe

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

#### Liveness Probe

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




# Kubernetes Dashboard Installation Guide

This guide will help you install and access the Kubernetes Dashboard on your Kubernetes cluster.

## Step 1: Deploy the Dashboard

Run the following command to deploy the Dashboard using `kubectl apply`:

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml
```

## Step 2: Create an Admin User

We will create an admin user to access the Dashboard. First, create a file named `dashboard-adminuser.yaml` with the following content:

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-user
  namespace: kubernetes-dashboard
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: admin-user
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: admin-user
  namespace: kubernetes-dashboard
```

Then, apply this file with `kubectl`:

```bash
kubectl apply -f dashboard-adminuser.yaml
```

## Step 3: Get the Authentication Token

Run the following command to get the authentication token that you will use to access the Dashboard:

```bash
kubectl -n kubernetes-dashboard create token admin-user
```

This command will return a token. Copy this token as you will need it to log in to the Dashboard.

## Step 4: Access the Dashboard

To access the Kubernetes Dashboard, start a `kubectl` proxy:

```bash
kubectl proxy
```

Once the proxy is running, open your web browser and go to:

```
http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/
```

Enter the token you obtained earlier when prompted.

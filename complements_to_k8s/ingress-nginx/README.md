
# Installing Ingress-NGINX on AWS EKS

This guide provides step-by-step instructions to install Ingress-NGINX on an Amazon EKS cluster.

## Prerequisites

1. AWS CLI configured.
2. `kubectl` installed and configured for accessing your EKS cluster.
3. Helm installed.

## Installing Helm

### On macOS

You can install Helm using Homebrew:
```bash
brew install helm
```

### On Windows

You can install Helm using Chocolatey:
```powershell
choco install kubernetes-helm
```

Or using Scoop:
```powershell
scoop install helm
```

### On Linux

You can install Helm by downloading the binary:
```bash
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
```

## Installing INGRESS-NGINX

## Step 1: Configure `kubectl` for EKS

Make sure `kubectl` is configured for your EKS cluster:
```bash
aws eks --region <your-region> update-kubeconfig --name <your-cluster-name>
```

## Step 2: Add the Helm Repository for Ingress-NGINX

Add the Helm repository for Ingress-NGINX and update the repository list:
```bash
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
```

## Step 3: Install Ingress-NGINX using Helm

Install Ingress-NGINX on your EKS cluster:
```bash
helm install ingress-nginx ingress-nginx/ingress-nginx --namespace ingress-nginx --create-namespace
```

This will deploy Ingress-NGINX in the `ingress-nginx` namespace. You can verify that the pods are running correctly:
```bash
kubectl get pods --namespace ingress-nginx
```

## Step 4: Verify the Installation

Ensure that the Ingress-NGINX service has an external IP assigned:
```bash
kubectl get svc --namespace ingress-nginx
```

You should see something like this:
```
NAME                                 TYPE           CLUSTER-IP       EXTERNAL-IP                                                               PORT(S)                      AGE
ingress-nginx-controller             LoadBalancer   172.20.46.131    a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7-1234567890.us-west-2.elb.amazonaws.com   80:31513/TCP,443:31429/TCP   2m
ingress-nginx-controller-admission   ClusterIP      172.20.36.107    <none>                                                                    443/TCP                      2m
```

# DevPulse Manifests

Infrastructure-as-Code (IaC) repository for the DevPulse application. This repository uses **Kustomize** to manage environment-specific configurations and is designed to work with **ArgoCD**.

## Repository Structure

- `base/`: Common Kubernetes manifests shared across all environments.
- `overlays/`: Environment-specific overrides (patches).
  - `dev/`: Development environment settings.
  - `staging/`: Staging environment settings.
  - `prod/`: Production environment settings (High availability, resource limits).
- `argocd/`: ArgoCD `Application` manifests to bootstrap the GitOps lifecycle.

## Getting Started

### 1. Locally Preview Manifests
You can use `kustomize` (built into `kubectl`) to see what will be applied to the cluster:

```bash
# Preview Dev environment
kubectl kustomize overlays/dev

# Preview Prod environment
kubectl kustomize overlays/prod
```

## Accessing the Application

After deploying, you can access your application via the AWS Application Load Balancer created by the Ingress Controller.

### 1. Find the URL
Run the following command in your terminal:
```bash
kubectl get ingress -n devpulse-dev
```
Look for the **ADDRESS** column. Copy that DNS name (e.g., `k8s-devpulse-dev-devpulse-123456.us-east-1.elb.amazonaws.com`).

### 2. Wait for Provisioning
AWS can take **2-5 minutes** to fully provision the Load Balancer and for the DNS to propagate. If you see a "404" or timeout initially, wait a few minutes and try again.

### 3. Open in Browser
Paste the DNS name from step 1 into your browser's address bar.

---

## GitOps Workflow

1.  **Modify:** Make changes to the manifests in this repository.
2.  **Commit & Push:** Push your changes to GitHub.
3.  **Sync:** ArgoCD will detect the changes and automatically synchronize the cluster state.
4.  **Observe:** Visit your ArgoCD dashboard to see the live status of your application.

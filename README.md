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

### 2. Connect to ArgoCD
To enable the "deployment map" and automated syncing, apply the ArgoCD application manifest:

```bash
# Bootstrapping the Dev environment in ArgoCD
kubectl apply -f argocd/application-dev.yaml
```

## GitOps Workflow

1.  **Modify:** Make changes to the manifests in this repository.
2.  **Commit & Push:** Push your changes to GitHub.
3.  **Sync:** ArgoCD will detect the changes and automatically synchronize the cluster state.
4.  **Observe:** Visit your ArgoCD dashboard to see the live status of your application.

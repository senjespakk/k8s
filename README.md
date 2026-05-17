# Kubernetes GitOps Repository

This repository manages Kubernetes deployments using GitOps principles with Kustomize. All environment configurations are version-controlled and deployed automatically through a continuous delivery system.

---

## 📁 Repository Structure
k8s/
├── base/                  # Shared application manifests
│   ├── deployment.yaml
│   ├── service.yaml
│   ├── ingress.yaml
│   └── kustomization.yaml
│
└── overlays/
    ├── staging/          # Staging environment config
    │   ├── kustomization.yaml
    │   └── patch.yaml
    │
    └── production/       # Production environment config
        ├── kustomization.yaml
        └── patch.yaml


---

## ⚙️ How It Works

- Base contains common Kubernetes resources  
- Overlays apply environment-specific changes  
- Kustomize builds final manifests per environment  
- Git acts as the single source of truth  
- Deployment is handled via GitOps tools like:
  - Argo CD
  - Flux CD  

---

## 🚀 Deployment Flow

1. Developer creates a feature branch  
2. Changes are made in base or overlays  
3. Pull Request is opened  
4. After merge to `main`:
   - CI validates manifests  
   - GitOps controller syncs cluster automatically  

---

## 🧪 Local Testing

Validate manifests before commit:

```bash
kubectl kustomize k8s/overlays/staging
kubectl apply -k k8s/overlays/staging --dry-run=client

## 📦 Apply Manifests Manually (Optional)

```bash
kubectl apply -k k8s/overlays/staging
kubectl apply -k k8s/overlays/production

## 🔐 Authentication Note (Important)

If using cloud clusters:

- Ensure kubeconfig is valid  
- Remove deprecated credentials (e.g., old `doctl` configs)  
- Verify context:

```bash
kubectl config get-contexts
kubectl config use-context <context-name>

## 🔄 Branch Strategy (Recommended)

- `main` → production-ready manifests  
- `develop` → staging integration  
- feature branches → isolated changes  

## 🧠 Best Practices

- Never edit cluster directly  
- Always go through Git PRs  
- Keep base minimal and reusable  
- Use overlays for environment drift control  
- Version every deployment change  
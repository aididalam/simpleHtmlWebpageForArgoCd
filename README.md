# Learning ArgoCD with GitOps

A hands-on project for learning ArgoCD and GitOps on a local Kubernetes cluster using KIND.

## Structure

```
simpleHtmlWebpageForArgoCd/
├── simple-webpage/     # App source — HTML, Dockerfile, Makefile
└── simple-gitops/      # Kubernetes manifests — ArgoCD watches this
    └── simple-webpage/
        ├── namespace.yml
        ├── deployment.yml
        └── service.yml
```

## How it works

1. Edit `simple-webpage/index.html`
2. Build and push a new image to Docker Hub
3. Update the image tag in `simple-gitops/simple-webpage/deployment.yml`
4. Push to GitHub — ArgoCD detects the change and syncs automatically

## Setup

### 1. Create KIND cluster

```bash
kind create cluster --config kind-cluster.yaml
```

### 2. Install ArgoCD

```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

### 3. Access ArgoCD UI

```bash
kubectl port-forward svc/argocd-server -n argocd 8081:443
```

Open https://localhost:8081 — username: `admin`

```bash
# get password
kubectl -n argocd get secret argocd-initial-admin-secret \
  -o jsonpath="{.data.password}" | base64 --decode
```

### 4. Add private repo (if needed)

```bash
argocd repo add https://github.com/aididalam/simple-gitops \
  --username aididalam \
  --password YOUR_GITHUB_TOKEN
```

### 5. Build and push image

```bash
cd simple-webpage
make TAG=v1.0.0
```

## Repos

| Purpose | Repository |
|---|---|
| App source | https://github.com/aididalam/simple-webpage |
| GitOps manifests | https://github.com/aididalam/simple-gitops |

## Key lessons learned

- KIND port mapping: `hostPort` on the KIND config must match the `nodePort` on the Kubernetes Service
- ArgoCD needs a GitHub token (not password) to access private repos
- Mac M1 is `arm64` — Docker images must be built with `--platform linux/arm64` or multi-arch (`linux/amd64,linux/arm64`)

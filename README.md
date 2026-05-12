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

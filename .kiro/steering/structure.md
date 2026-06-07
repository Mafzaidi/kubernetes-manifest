# Project Structure

Repository ini mengikuti konvensi GitOps dengan pemisahan concern yang jelas.

```
kubernetes-manifest/
├── apps/                                    # Helm charts per-aplikasi
│   └── authorizer/
│       ├── Chart.yaml                       # Helm chart metadata (apiVersion v2, type: application)
│       ├── values.production.yaml           # Values untuk environment production
│       ├── .argocd-source-authorizer-production.yaml  # ArgoCD Image Updater write-back file
│       ├── files/
│       │   └── config.yaml                  # Non-sensitive app config (mounted as ConfigMap)
│       └── templates/
│           ├── _helpers.tpl                 # Helm template helpers (name, fullname, labels)
│           ├── configmap.yaml               # ConfigMap dari files/config.yaml
│           ├── deployment.yaml              # Deployment spec
│           ├── service.yaml                 # ClusterIP Service
│           ├── ingress.yaml                 # Nginx Ingress
│           ├── hpa.yaml                     # HorizontalPodAutoscaler
│           ├── serviceaccount.yaml          # ServiceAccount
│           └── NOTES.txt                    # Helm install notes
├── argocd/                                  # ArgoCD Application definitions
│   ├── authorizer/
│   │   └── application-production.yaml      # ArgoCD App untuk Helm chart
│   └── secrets/
│       └── authorizer/
│           └── secret-app-production.yaml   # ArgoCD App untuk secrets (dari kubernetes-secrets repo)
├── core/                                    # Cluster-wide tooling configs
│   └── argocd/
│       └── image-updater/
│           └── authorizer-production.yaml   # Image Updater config
└── tools/                                   # Utility scripts (empty)
```

## Konvensi

1. Setiap aplikasi mendapat folder sendiri di `apps/<app-name>/` berisi Helm chart lengkap
2. ArgoCD Application manifest disimpan di `argocd/<app-name>/application-<env>.yaml`
3. Secret references disimpan di `argocd/secrets/<app-name>/secret-app-<env>.yaml`
4. Image Updater config di `core/argocd/image-updater/<app-name>-<env>.yaml`
5. Values file per-environment: `values.<env>.yaml`
6. Non-sensitive config files di `apps/<app-name>/files/`
7. ArgoCD Image Updater write-back file: `.argocd-source-<app-name>-<env>.yaml`

## Naming Pattern

- ArgoCD Application name: `<app-name>-<env>` (e.g., `authorizer-production`)
- Namespace: `<app-name>-<env>` (e.g., `authorizer-production`)
- Secret name: `<app-name>-secret`
- ConfigMap name: `<app-name>-config`
- JWT secret name: `<app-name>-jwt-key`

# Tech Stack & Tools

## Templating & Packaging
- Helm v3 (Chart apiVersion v2)
- Helm chart type: `application`

## GitOps & CD
- ArgoCD — continuous deployment dari Git ke cluster
- ArgoCD Image Updater — auto-update image tag berdasarkan semver/latest dari registry
- Write-back method: `git` (commit langsung ke branch)

## Kubernetes Resources
- Deployment, Service (ClusterIP), Ingress (nginx), HPA, ServiceAccount, ConfigMap
- autoscaling/v2 HPA dengan CPU target

## Ingress
- Ingress class: `nginx`
- Path type: `Prefix`

## Image Strategy
- Registry: DockerHub (`docker.io/mafzaidi93/<app>`)
- Tag strategy: SemVer (`^[0-9]+\.[0-9]+\.[0-9]+$`)
- Pull policy: `IfNotPresent`

## Security Defaults
- Pod security context: `fsGroup: 2000`
- Container security: drop ALL capabilities, readOnlyRootFilesystem, runAsNonRoot, runAsUser 1000
- No privilege escalation

## Versioning & Commits
- Conventional Commits via Commitizen (`cz_conventional_commits`)
- SemVer tagging

## Branching
- `production` branch — ArgoCD syncs from this branch
- Image Updater commits tag updates to `production` branch

## Common Operations

```bash
# Lint Helm chart
helm lint apps/authorizer -f apps/authorizer/values.production.yaml

# Template render (dry-run)
helm template authorizer apps/authorizer -f apps/authorizer/values.production.yaml

# Bump version
cz bump
```

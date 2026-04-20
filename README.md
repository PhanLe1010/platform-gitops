# platform-gitops

GitOps repository watched by ArgoCD. Contains Kubernetes manifests
for all platform applications.

## Structure

- `apps/<app>/base/` — common manifests
- `apps/<app>/overlays/<env>/` — environment-specific patches

## How changes reach the cluster

1. CI in the app repo builds and pushes a new image
2. CI opens a PR here updating the image tag in the relevant overlay
3. PR is reviewed and merged
4. ArgoCD detects the commit and syncs to the cluster

## Local validation

```bash
kustomize build apps/platform-app/overlays/staging
kustomize build apps/platform-app/overlays/production
```

## Never do

- Don't `kubectl apply` directly to the cluster — ArgoCD will revert it
- Don't commit secrets here — use sealed-secrets or external-secrets
# argocd

## Preface

**Important**: Initially, this repository only contains configuration for the ecs-cs ArgoCD.

## Preparation

### Install kustomize

There are two ways to use kustomize:

1. Install natively: <https://kubectl.docs.kubernetes.io/installation/kustomize/>
2. Use with `kubectl`: <https://kubernetes.io/docs/tasks/manage-kubernetes-objects/kustomization/>

## Deployment

### Deploy the ECS-CS ArgoCD

```bash
kustomize build . > zz_generated_manifests.yaml
kubectl -n argocd apply -f zz_generated_manifests.yaml
```

### Get initial admin credentials

Get the initial admin password:

```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```


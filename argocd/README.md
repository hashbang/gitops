[ArgoCD](https://argoproj.github.io/argo-cd/)

With [KSOPS](https://github.com/viaduct-ai/kustomize-sops) integration.

# Bootstrapping

On a new kubernetes cluster you can run:

```
kustomize build . | kubectl apply -f -
```


# Upgrading

1. Get the new commit hash of the release tag in the ArgoCD repository
2. Update the new commit hash in kustomization.yaml
3. Update any other places image names appear, e.g. argo-cd-import-pgp-key.yaml
4. Hash lock the new images:
   - `kustomize build argocd/ | grep image:`
   - For each image, get the image hash (e.g. by visiting dockerhub)
   - Update the image hashes in kustomization.yaml
   - Run `kustomize build argocd/ | grep image:` and confirm that all images are hash-locked

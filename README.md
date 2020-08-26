# Hashbang GitOps

## About

This repository contains the configuration for our Kubernetes cluster.
Each folder contains an "application" that is deployed into the cluster.

Deployment is done with a self-managed [ArgoCD](https://argoproj.github.io/argo-cd/) instance.
Secrets are encrypted in this repository using [SOPS](https://github.com/mozilla/sops) and applied via [KSOPS](https://github.com/viaduct-ai/kustomize-sops).

If you'd like to change anything about hashbang's infrastructure, please send a PR!


## Common Tasks

### Adding New Admin(s)

Add the new admin's PGP key to `.sops.yaml`, then run:

```sh
find . -name '*.enc.yaml' | while read file; do
	sops updatekeys -y $file
done
```

Create a new argocd local user for the admin (`argocd/users.patch.yaml`).
Add the new user to the admin group (`argocd/argo-cd-rbac.patch.yaml`).
Have the new user create a password for accessing argocd and hash it with e.g. `htpasswd -n -B -C 10 adminusername`. Add it to `argocd/argocd-secret.enc.yaml`.

Have the new user create a password for accessing metrics and hash it with e.g. `htpasswd -n -B -C 10 adminusername`. Add it to `monitoring/user-auth.enc.yaml`.

Add the admin's PGP key to `argocd/gpg-keys/KEYID` (and update the list in `argocd/kustomization.yaml`) e.g.
```shell
gpg -a --export --export-options export-minimal C91A9911192C187A > argocd/gpg-keys/C91A9911192C187A
```

Add the admin's PGP key to the allowed list in `argocd/projects/default.yaml`.

Add the admin's PGP key to `mtls/files/admin_seeds/` (and update the list in `mtls/kustomization.yaml`)

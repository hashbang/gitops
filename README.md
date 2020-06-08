# Hashbang GitOps

## About

This repository contains the configuration for our Kubernetes cluster.
Each folder contains an "application" that is deployed into the cluster.

Deployment is done with a self-managed [ArgoCD](https://argoproj.github.io/argo-cd/) instance.
Secrets are encrypted in this repository using [SOPS](https://github.com/mozilla/sops) and applied via [KSOPS](https://github.com/viaduct-ai/kustomize-sops).

If you'd like to change anything about hashbang's infrastructure, please send a PR!

## Setup

### Dependencies

1. kubectl
2. kustomize
3. sops
4. [ksops][ksops]
5. gnupg

For encrypting passwords please ensure that you import the Hashbang ArgoCD GPG Key.
This can be done by running `gpg --import extras/deploy-key.pub`


## Common Tasks

### Adding New Admin(s)

Add the new admin's PGP key to `.sops.yaml`, then run:

```sh
find . -name '*.enc.yaml' | while read file; do
	sops updatekeys -y $file
done
```

Create a new argocd local user for the admin (`argocd/users.yaml`).
An existing admin will need to generate a password for the new admin.

Add the new user to the admin group (`argocd/argo-cd-rbac.yaml`).

Have the new user create a password for accessing metrics and hash it with `htpasswd -n -B adminusername`. Add it to `monitoring/user-auth.env.yaml`.

Add the admin's PGP key to `mtls/files/admin_seeds/` (and update the list in `mtls/kustomization.yaml`)

[ksops]: https://github.com/viaduct-ai/kustomize-sops

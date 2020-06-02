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
for file in */*.enc.yaml ; do 
	sops updatekeys -y $file
done
```

Create a new argocd local user for the admin (`argocd/users.yaml`).
An existing admin will need to generate a password for the new admin.

Add the new user to the default argo project (`argocd/projects/default.yaml`).

Have the new user create a password for accessing metrics and hash it with `htpasswd -n -B adminusername`. Add it to `monitoring/user-auth.env.yaml`.

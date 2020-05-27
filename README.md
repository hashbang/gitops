# Hashbang GitOps

## About

This repository contains the configuration for our Kubernetes cluster.
Each folder contains an "application" that is deployed into the cluster.

Deployment is done with a self-managed [ArgoCD](https://argoproj.github.io/argo-cd/) instance.

If you'd like to change anything about hashbang's infrastructure, please send a PR!


## Common Tasks

### Adding new admins:

```sh
for file in */*.enc.yaml ; do 
	sops updatekeys -y $file
done
```

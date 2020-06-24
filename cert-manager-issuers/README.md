# Cert-Manager Issuer
![Cert-Manager Issuer Status Indicator](https://argocd.hashbang.sh/api/badge?name=cert-manager-issuer)

# variables

prod_issuer.yaml:
- spec.acme.solvers.1.dns01.route53.accessKeyID outputted from [Terraform](https://github.com/hashbang/admin-tools).

# secrets

iam-key.enc.yaml: data.iam-key outputted from [Terraform](https://github.com/hashbang/admin-tools).

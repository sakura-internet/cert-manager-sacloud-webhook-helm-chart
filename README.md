# Helm chart of Cert manager webhook for sacloud 📦

![](https://img.shields.io/badge/version-v1.0.0-green)

Helm chart of Cert manager webhook for sacloud.

## Get started

### Cert manager

first of all, you deploy Cert manager.

```bash
$ kubectl create namespace cert-manager
$ helm repo add jetstack https://charts.jetstack.io
$ helm repo update

$ kubectl apply --validate=false -f https://github.com/jetstack/cert-manager/releases/download/v1.0.4/cert-manager.crds.yaml

$ helm install cert-manager jetstack/cert-manager --namespace cert-manager --version v1.0.4
```

### Webhook for sacloud

Next, you deploy Cert manager webhook for sacloud as Helm chart.

```bash
$ helm repo add cert-manager-sacloud-webhook https://0n1shi.github.io/cert-manager-sacloud-webhook-helm-chart/
$ helm install cert-manager-sacloud-webhook cert-manager-sacloud-webhook/cert-manager-sacloud-webhook --namespace cert-manager --version v1.0.0
```

### Nginx to test

Now you can create issuer and certificate to deploy Nginx which we can access over TLS.

```bash
# you need to edit manifests below before apply

kubectl apply -f examples/cert-manager-sacloud-webhook/issuer.yaml
kubectl apply -f examples/cert-manager-sacloud-webhook/certificate.yaml
kubectl apply -f examples/cert-manager-sacloud-webhook/nginx.yaml
```

## Development

```bash
helm lint charts/cert-manager-sacloud-webhook
helm package charts/cert-manager-sacloud-webhook

# genereted charts/cert-manager-sacloud-webhook-<VERSION>.tgz in the current directory.
```

# Refs

- https://cert-manager.io/docs/concepts/webhook/
- https://helm.sh/docs/topics/chart_repository/


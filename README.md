
## nginx Ingress

### Add repository

```bash
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
```

### Install

This installs the `ingress-nginx` HELM-Chart to the default namespace.

```bash
helm install nginx-ingress ingress-nginx/ingress-nginx
```

Check nginx is ready:

```bash
kubectl get services nginx-ingress-ingress-nginx-controller 
```

### Update

```bash
helm upgrade nginx-ingress ingress-nginx/ingress-nginx
```

### Remove

```bash
helm uninstall nginx-ingress
```

> HINT: Use the `--dry-run` flag to see which releases will be uninstalled without actually uninstalling them.

## hello world app

### Deployment

The `Deployment` starts 3 replicas of our `helloworld` container in the default namespace of Kubernetes.

```bash
kubectl apply -f helloworld-deployment.yaml
```

### Ingress

```bash
kubectl apply -f helloworld-ingress.yaml
```

## cert-manager for SSL,TLS

### Add repository

```bash
helm repo add jetstack https://charts.jetstack.io
helm repo update
```

### Install

```bash
helm install \
  cert-manager jetstack/cert-manager \
  --namespace pxio-dev \
  --version v1.1.0 \
  --set installCRDs=true
```

> NOTE: `--set installCRDs=true` installs **Custom Resource Definitions** into the Kubernetes cluster

## Add TLS capabilitites

1. Install cert-manager certificate issuer resource

```bash
kubectl create  -f letsencrypt-staging.yaml
```

2. Apply TLS config to deployment

```bash
kubectl apply -f helloworld-ingress-tls.yaml
```
## Notes

- Get available external IPs: `kubectl get svc -n kube-system`
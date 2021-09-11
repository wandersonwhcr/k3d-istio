# kubernetes-ingress

```
k3d cluster create region-1 \
    --servers 1 \
    --agents 1 \
    --port 9080:80@loadbalancer \
    --k3s-server-arg --no-deploy=traefik

istioctl install --skip-confirmation --set profile=demo
```

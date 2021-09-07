# k3d-istio

```
k3d cluster create hello \
    --servers 1 \
    --agents 3 \
    --port 9080:80@loadbalancer \
    --port 9443:443@loadbalancer \
    --api-port 6443 \
    --k3s-server-arg '--no-deploy=traefik'

kubectl config use-context k3d-hello

kubectl cluster-info
```

## References

* [k3d - Kubernetes & Istio (Service Mesh)](https://brettmostert.medium.com/k3d-kubernetes-istio-service-mesh-286a7ba3a64f)
* [istio - Getting Started](https://istio.io/latest/docs/setup/getting-started/)
